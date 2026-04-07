# 11 - MCP 协议集成

> OpenCode v1.3.17 · 源码级深度解析
> Java 开发者友好 · 手机可读

---

## 一、MCP 协议概述

### 1.1 什么是 MCP

**MCP（Model Context Protocol）** 是 Anthropic 提出的开放协议，用于让 AI 应用与外部工具/服务进行标准化通信。它定义了统一的 Tool 发现、调用和资源访问接口。

> 💡 **Java 类比**：MCP 就像 Java 的 **JDBC（Java Database Connectivity）**——JDBC 定义了统一的数据库访问接口，各种数据库只需提供 JDBC Driver 即可接入。MCP 则定义了统一的 AI 工具接口，各种服务只需提供 MCP Server 即可被 AI 调用。

### 1.2 MCP 在 OpenCode 中的定位

```mermaid
graph LR
    subgraph "OpenCode MCP 集成"
        direction TB
        USER["用户"]
        LLM["LLM<br/>(Anthropic/OpenAI/...)"]
        OC["OpenCode<br/>MCP Client"]
        MCP_S1["MCP Server A<br/>(filesystem)"]
        MCP_S2["MCP Server B<br/>(github)"]
        MCP_S3["MCP Server C<br/>(自定义)"]
    end

    USER -->|"发送消息"| LLM
    LLM -->|"需要工具"| OC
    OC -->|"listTools + callTool"| MCP_S1
    OC -->|"listTools + callTool"| MCP_S2
    OC -->|"listTools + callTool"| MCP_S3
    MCP_S1 -->|"执行结果"| OC
    MCP_S2 -->|"执行结果"| OC
    MCP_S3 -->|"执行结果"| OC
    OC -->|"工具结果"| LLM
    LLM -->|"最终回复"| USER
```

### 1.3 MCP 支持的能力

| 能力 | 说明 | 状态 |
|------|------|------|
| **Tool** | 发现和调用远程工具 | ✅ 已实现 |
| **Resource** | 读取远程资源 | ✅ 已实现 |
| **Prompt** | 获取预定义提示模板 | ✅ 已实现 |
| **OAuth 认证** | 远程 MCP Server 的 OAuth 流程 | ✅ 已实现 |
| **动态注册** | OAuth Client 动态注册 | ✅ 已实现 |
| **工具变更通知** | Server 端工具列表变更实时通知 | ✅ 已实现 |

---

## 二、MCP 集成架构

### 2.1 三层架构图

```mermaid
graph TB
    subgraph ConfigLayer["配置层"]
        CFG["opencode.json<br/>mcp: { ... }"]
    end

    subgraph ServiceLayer["MCP Service 层"]
        SVC["MCP.Service<br/>Effect 服务"]
        STATE["State<br/>{ status, clients, defs }"]
        AUTH["McpAuth.Service<br/>Token 存储"]
        OAUTH["McpOAuthProvider<br/>OAuth 客户端"]
        CALLBACK["McpOAuthCallback<br/>回调服务器"]
    end

    subgraph TransportLayer["传输层"]
        STDIO["StdioClientTransport<br/>本地进程"]
        HTTP["StreamableHTTPClientTransport<br/>HTTP 流"]
        SSE["SSEClientTransport<br/>Server-Sent Events"]
    end

    subgraph ExternalLayer["外部 MCP Server"]
        LOCAL_S["本地命令<br/>(npx, python 等)"]
        REMOTE_S["远程服务<br/>(HTTP/SSE)"]
    end

    CFG --> SVC
    SVC --> STATE
    SVC --> AUTH
    SVC --> OAUTH
    SVC --> CALLBACK

    SVC --> STDIO --> LOCAL_S
    SVC --> HTTP --> REMOTE_S
    SVC --> SSE --> REMOTE_S
```

### 2.2 MCP Service 状态模型

```typescript
// MCP Service 内部状态
interface State {
  status: Record<string, Status>        // 每个 Server 的连接状态
  clients: Record<string, MCPClient>    // 每个 Server 的客户端实例
  defs: Record<string, MCPToolDef[]>    // 每个 Server 的工具定义缓存
}

// Server 连接状态 — 联合类型（类似 Java sealed interface）
type Status =
  | { status: "connected" }                                    // 已连接
  | { status: "disabled" }                                     // 已禁用
  | { status: "failed"; error: string }                        // 连接失败
  | { status: "needs_auth" }                                   // 需要认证
  | { status: "needs_client_registration"; error: string }     // 需要客户端注册
```

---

## 三、连接类型详解

### 3.1 Local（本地进程）连接

```mermaid
sequenceDiagram
    participant MCP as MCP.Service
    participant Transport as StdioClientTransport
    participant Process as 子进程<br/>(npx/p python)
    participant Server as MCP Server

    MCP->>Transport: new StdioClientTransport({ command, args, env })
    Transport->>Process: spawn 子进程
    Process->>Server: stdin/stdout 通信
    Server-->>Process: MCP 初始化响应
    
    MCP->>Transport: client.connect(transport)
    Transport->>Process: JSON-RPC: initialize
    Process->>Server: 转发请求
    Server-->>Process: 返回 capabilities
    Process-->>Transport: 返回结果
    Transport-->>MCP: 连接成功

    Note over MCP,Server: 运行时
    MCP->>Transport: client.listTools()
    Transport->>Process: JSON-RPC: tools/list
    Process->>Server: 转发
    Server-->>Process: 工具列表
    Process-->>Transport: 返回结果
    Transport-->>MCP: MCPToolDef[]
```

### 3.2 Remote（远程 HTTP）连接

```mermaid
sequenceDiagram
    participant MCP as MCP.Service
    participant HTTP as StreamableHTTP
    participant SSE as SSEClientTransport
    participant Server as 远程 MCP Server

    MCP->>HTTP: 尝试 StreamableHTTP 连接
    HTTP->>Server: POST /mcp (HTTP 流)
    
    alt 需要 OAuth
        Server-->>HTTP: 401 Unauthorized
        HTTP-->>MCP: UnauthorizedError
        MCP->>MCP: 设置 status = "needs_auth"
    else 连接成功
        Server-->>HTTP: 200 OK + MCP 响应
        HTTP-->>MCP: 连接成功
    else StreamableHTTP 不可用
        Server-->>HTTP: 错误
        MCP->>SSE: 回退到 SSE 连接
        SSE->>Server: GET /sse
        Server-->>SSE: SSE 事件流
        SSE-->>MCP: 连接成功
    end
```

> 💡 **Java 类比**：远程连接的回退策略类似于 Spring 的 `@LoadBalanced` + `@Retry`——先尝试首选协议，失败后自动回退。

---

## 四、Tool 发现与调用

### 4.1 Tool 转换流程

```mermaid
flowchart LR
    subgraph "MCP Server 返回"
        MCP_DEF["MCPToolDef<br/>{name, description, inputSchema}"]
    end

    subgraph "OpenCode 转换"
        CONVERT["convertMcpTool()"]
        AI_TOOL["AI SDK Tool<br/>dynamicTool()"]
    end

    subgraph "LLM 调用"
        LLM["LLM 决策"]
        EXEC["execute(args)"]
        RESULT["CallToolResult"]
    end

    MCP_DEF --> CONVERT
    CONVERT -->|"包装为 dynamicTool"| AI_TOOL
    AI_TOOL --> LLM
    LLM -->|"选择工具"| EXEC
    EXEC -->|"client.callTool()"| RESULT
    RESULT --> LLM
```

### 4.2 Tool 转换伪代码

```typescript
// 将 MCP Tool 定义转换为 AI SDK Tool
function convertMcpTool(mcpTool: MCPToolDef, client: MCPClient, timeout?: number): Tool {
  const inputSchema = {
    ...mcpTool.inputSchema,
    type: "object",                    // 强制为 object
    properties: mcpTool.inputSchema?.properties ?? {},
    additionalProperties: false,       // 禁止额外属性
  }

  return dynamicTool({
    description: mcpTool.description ?? "",
    inputSchema: jsonSchema(inputSchema),
    execute: async (args) => {
      // 调用 MCP Server 的工具
      return client.callTool(
        {
          name: mcpTool.name,
          arguments: args ?? {},
        },
        CallToolResultSchema,
        { resetTimeoutOnProgress: true, timeout }
      )
    },
  })
}

// Java 类比: 类似将 JDBC ResultSet 映射为 Java 对象
// MCPToolDef (外部格式) → Tool (AI SDK 格式)
```

### 4.3 Tool 命名规则

MCP 工具在 OpenCode 中的命名格式为：`{sanitizedClientName}_{sanitizedToolName}`

```typescript
// 伪代码
function sanitize(s: string) {
  return s.replace(/[^a-zA-Z0-9_-]/g, "_")
}

// 示例:
// MCP Server "my-server" 的工具 "read-file"
// → OpenCode 工具名: "my-server_read-file"
```

---

## 五、MCP Server 配置

### 5.1 配置格式

```jsonc
// opencode.json
{
  "mcp": {
    // 本地 MCP Server（通过子进程启动）
    "filesystem": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"],
      "environment": {
        "NODE_ENV": "production"
      },
      "timeout": 30000,
      "enabled": true
    },

    // 远程 MCP Server（HTTP/SSE）
    "remote-tools": {
      "type": "remote",
      "url": "https://mcp.example.com/mcp",
      "headers": {
        "X-Custom-Header": "value"
      },
      "timeout": 60000,
      // OAuth 配置
      "oauth": {
        "clientId": "my-client-id",
        "clientSecret": "my-secret",
        "scope": "tools:read tools:write"
      }
      // 禁用 OAuth
      // "oauth": false
    },

    // 禁用某个 Server
    "disabled-server": {
      "type": "local",
      "command": ["echo", "disabled"],
      "enabled": false
    }
  }
}
```

### 5.2 配置字段说明表

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `type` | `"local" \| "remote"` | ✅ | 连接类型 |
| `command` | `string[]` | local 必填 | 启动命令及参数 |
| `url` | `string` | remote 必填 | 远程 Server URL |
| `environment` | `Record<string, string>` | ❌ | 环境变量 |
| `headers` | `Record<string, string>` | ❌ | 自定义 HTTP 头 |
| `timeout` | `number` | ❌ | 超时时间（ms），默认 30000 |
| `enabled` | `boolean` | ❌ | 是否启用，默认 true |
| `oauth` | `object \| false` | ❌ | OAuth 配置或禁用 |

---

## 六、OAuth 认证流程

### 6.1 完整认证时序图

```mermaid
sequenceDiagram
    participant User as 用户
    participant CLI as OpenCode CLI
    participant MCP as MCP.Service
    participant Provider as McpOAuthProvider
    participant Callback as 回调服务器<br/>(:19876)
    participant Server as MCP Server

    User->>CLI: opencode mcp auth my-server
    CLI->>MCP: authenticate("my-server")
    MCP->>MCP: getMcpConfig() 读取配置
    MCP->>Provider: new McpOAuthProvider(name, url, config)
    MCP->>Provider: new StreamableHTTPClientTransport(url, { authProvider })
    MCP->>MCP: client.connect(transport)
    MCP->>Server: HTTP 请求
    Server-->>MCP: 401 Unauthorized + redirect URL
    Provider->>Provider: saveCodeVerifier()
    Provider->>Provider: saveState(oauthState)

    MCP->>Callback: ensureRunning() 启动回调服务器
    MCP->>User: open(authorizationUrl) 打开浏览器
    User->>Server: 用户授权
    Server->>Callback: 重定向 + code + state
    Callback->>Callback: 验证 state
    Callback-->>MCP: authorizationCode
    MCP->>Provider: transport.finishAuth(code)
    Provider->>Server: 用 code 换 token
    Server-->>Provider: accessToken + refreshToken
    Provider->>Provider: saveTokens()

    MCP->>MCP: createAndStore() 重新连接
    MCP->>Server: 带 token 的请求
    Server-->>MCP: 连接成功
    MCP-->>CLI: { status: "connected" }
```

### 6.2 Token 存储结构

```typescript
// mcp-auth.json
{
  "my-server": {
    "tokens": {
      "accessToken": "eyJhbG...",
      "refreshToken": "dGhpcy...",
      "expiresAt": 1735689600,
      "scope": "tools:read tools:write"
    },
    "clientInfo": {
      "clientId": "opencode-abc123",
      "clientSecret": "secret-xyz",
      "clientIdIssuedAt": 1735689600,
      "clientSecretExpiresAt": 1767225600
    },
    "serverUrl": "https://mcp.example.com/mcp"
  }
}
```

---

## 七、运行时管理

### 7.1 工具变更通知

```mermaid
sequenceDiagram
    participant Server as MCP Server
    participant Client as MCP Client
    participant Watch as watch()
    participant Bus as 事件总线
    participant TUI as TUI 组件

    Note over Server,TUI: Server 端工具列表变更

    Server->>Client: notifications/tools/list_changed
    Client->>Watch: setNotificationHandler 触发
    Watch->>Watch: 重新调用 listTools()
    Watch->>Client: defs[name] = newTools
    Watch->>Bus: publish(ToolsChanged, { server: name })
    Bus->>TUI: 更新工具列表 UI
```

### 7.2 生命周期管理

```typescript
// InstanceState 初始化时
// 1. 读取所有 mcp 配置
// 2. 并发连接所有 Server（unbounded concurrency）
// 3. 为每个连接设置工具变更监听
// 4. 注册 Finalizer：关闭时杀掉子进程并断开连接

// 清理逻辑（Effect Finalizer）
Effect.addFinalizer(() =>
  Effect.forEach(Object.values(s.clients), async (client) => {
    // 1. 找到子进程 PID
    const pid = client.transport?.pid
    // 2. 递归杀掉所有子进程
    const pids = await descendants(pid)
    pids.forEach(p => process.kill(p, "SIGTERM"))
    // 3. 关闭客户端连接
    await client.close()
  })
)
```

---

## 八、MCP API 接口一览

```mermaid
classDiagram
    class MCPService {
        <<Interface>>
        +status() Effect~Record~
        +clients() Effect~Record~
        +tools() Effect~Record~
        +prompts() Effect~Record~
        +resources() Effect~Record~
        +add(name, mcp) Effect~Status~
        +connect(name) Effect~void~
        +disconnect(name) Effect~void~
        +getPrompt(client, name, args) Effect
        +readResource(client, uri) Effect
        +startAuth(name) Effect~AuthResult~
        +authenticate(name) Effect~Status~
        +finishAuth(name, code) Effect~Status~
        +removeAuth(name) Effect~void~
        +supportsOAuth(name) Effect~boolean~
        +hasStoredTokens(name) Effect~boolean~
        +getAuthStatus(name) Effect~AuthStatus~
    }

    class McpAuthService {
        <<Interface>>
        +all() Effect~Record~
        +get(name) Effect~Entry~
        +set(name, entry) Effect~void~
        +remove(name) Effect~void~
        +updateTokens(name, tokens) Effect~void~
        +updateClientInfo(name, info) Effect~void~
        +isTokenExpired(name) Effect~boolean~
    }
```

---

## 📦 源码锚点表

| 文件路径 | 核心内容 |
|---------|---------|
| `packages/opencode/src/mcp/index.ts` | MCP Service 主入口（连接、工具、认证） |
| `packages/opencode/src/mcp/auth.ts` | McpAuth Service（Token 存储管理） |
| `packages/opencode/src/mcp/oauth-provider.ts` | McpOAuthProvider（OAuth 客户端实现） |
| `packages/opencode/src/mcp/oauth-callback.ts` | McpOAuthCallback（回调服务器 :19876） |
| `packages/opencode/src/server/routes/mcp.ts` | MCP HTTP API 路由 |
| `packages/opencode/src/config/config.ts` | MCP 配置类型定义（`Config.Mcp`） |
