# A - Java 开发者 TS 速查

> OpenCode 源码学习 · 附录
> 手机可读 · GitHub 原生渲染

---

## 一、TypeScript 基础语法与 Java 对比

### 1.1 类型系统对比

| 概念 | TypeScript | Java | 说明 |
|------|-----------|------|------|
| **基础类型** | `string`, `number`, `boolean` | `String`, `int/long`, `boolean` | TS 的 `number` = Java 的 double |
| **空值** | `null \| undefined` | `null` | TS 有两种空值 |
| **任意类型** | `any` | `Object` | 逃逸类型检查 |
| **未知类型** | `unknown` | N/A | 类型安全的 any |
| **void** | `void` | `void` | 无返回值 |
| **never** | `never` | N/A | 永不返回 |
| **数组** | `T[]` 或 `Array<T>` | `T[]` 或 `List<T>` | 语法相同 |
| **元组** | `[string, number]` | N/A | 固定长度数组 |
| **联合** | `string \| number` | N/A | 多类型之一 |
| **交叉** | `A & B` | N/A | 同时满足多个类型 |
| **字面量** | `"hello"` | N/A | 值即类型 |
| **类型断言** | `x as Type` | `(Type) x` | 强制转换 |
| **类型守卫** | `typeof x === "string"` | `instanceof Type` | 窄化类型 |

### 1.2 接口对比

```typescript
// ====== TypeScript 接口 ======
interface User {
  name: string
  age: number
  email?: string          // 可选属性
  readonly id: string     // 只读属性
}

// 扩展接口
interface Admin extends User {
  permissions: string[]
}

// 类型别名（更灵活）
type Status = "active" | "inactive" | "banned"

// 索引签名
interface Dictionary {
  [key: string]: string
}
```

```java
// ====== Java 等价写法 ======
public interface User {
    String getName();
    int getAge();
    default Optional<String> getEmail() { return Optional.empty(); }
    // readonly 无直接等价，通常用不可变类
}

public interface Admin extends User {
    List<String> getPermissions();
}

// 联合类型无直接等价，通常用 enum
public enum Status { ACTIVE, INACTIVE, BANNED }

// Map 等价索引签名
Map<String, String> dictionary = new HashMap<>();
```

### 1.3 泛型对比

| 特性 | TypeScript | Java |
|------|-----------|------|
| **声明** | `function fn<T>(x: T): T` | `<T> T fn(T x)` |
| **约束** | `<T extends Comparable>` | `<T extends Comparable<T>>` |
| **默认类型** | `<T = string>` | N/A |
| **条件类型** | `T extends U ? X : Y` | N/A |
| **映射类型** | `{ [K in keyof T]: ... }` | N/A |
| **infer** | `type ReturnType<T> = T extends (...args) => infer R ? R : never` | N/A |
| **协变/逆变** | 自动推断 | `? extends T` / `? super T` |

### 1.4 枚举对比

```typescript
// TypeScript 数字枚举
enum Direction {
  Up,      // 0
  Down,    // 1
  Left,    // 2
  Right,   // 3
}

// TypeScript 字符串枚举（OpenCode 中大量使用）
type Status = "connected" | "failed" | "disabled" | "needs_auth"

// TypeScript const enum（编译时内联）
const enum Flags {
  Read = 1 << 0,
  Write = 1 << 1,
}
```

```java
// Java 枚举
public enum Direction {
    UP, DOWN, LEFT, RIGHT;
}

// Java 字符串枚举
public enum Status {
    CONNECTED, FAILED, DISABLED, NEEDS_AUTH;
}
```

### 1.5 装饰器对比

| 装饰器 | TypeScript | Java |
|--------|-----------|------|
| **类** | `@sealed class Foo` | N/A |
| **方法** | N/A（TS5.0 移除） | `@Override` `@Transactional` |
| **属性** | N/A（TS5.0 移除） | `@Autowired` `@Column` |
| **参数** | N/A | `@PathParam` |

> 💡 TS 装饰器在 TS5.0 进入 Stage 3，OpenCode 未使用装饰器，而是使用 Effect 的 `Schema.Class` 和 `ServiceMap.Service`。

---

## 二、现代 TS 特性速查

### 2.1 可选链与空值合并

```typescript
// 可选链 (?.) — 类似 Java 的 Optional.map()
const name = user?.profile?.name        // 如果 user 为空，返回 undefined

// 空值合并 (??) — 只在 null/undefined 时使用默认值
const value = data ?? "default"          // 类似 data != null ? data : "default"

// 空值赋值 (??=)
let config = {}
config.debug ??= true                    // 只在 config.debug 为 null/undefined 时赋值

// 可选链 + 方法调用
const result = list?.find(item => item.id === 1)?.toString()

// Java 等价:
// Optional.ofNullable(user)
//   .map(u -> u.getProfile())
//   .map(p -> p.getName())
//   .orElse(null);
```

### 2.2 解构与展开

```typescript
// 对象解构
const { name, age, ...rest } = user

// 数组解构
const [first, second, ...others] = items

// 函数参数解构
function process({ id, name }: { id: string; name: string }) { }

// 展开运算符
const merged = { ...defaults, ...userConfig }
const combined = [...listA, ...listB]

// Java 等价: 使用 Builder 或 record 模式
// record User(String name, int age) {}
// var merged = new User(defaults.name(), config.age());
```

### 2.3 模板字面量类型

```typescript
// OpenCode 中大量使用
type EventName = `${string}.changed`
type HookName = `chat.${string}`

// 条件类型
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T
// UnwrapPromise<Promise<string>> → string
// UnwrapPromise<number> → number

// 映射类型
type Readonly<T> = { readonly [K in keyof T]: T[K] }
type Partial<T> = { [K in keyof T]?: T[K] }

// Java 等价: 无直接对应，映射类型是 TS 特有的元编程能力
```

### 2.4 常用类型工具

| 工具类型 | 作用 | Java 类比 |
|---------|------|----------|
| `Partial<T>` | 所有属性变可选 | 无 |
| `Required<T>` | 所有属性变必填 | 无 |
| `Readonly<T>` | 所有属性变只读 | `Collections.unmodifiableMap()` |
| `Pick<T, K>` | 选取部分属性 | `record` 投影 |
| `Omit<T, K>` | 排除部分属性 | 无 |
| `Record<K, V>` | 构造对象类型 | `Map<K, V>` |
| `Exclude<T, U>` | 排除联合成员 | 无 |
| `Extract<T, U>` | 提取联合成员 | 无 |
| `ReturnType<T>` | 提取返回类型 | 反射 API |
| `Parameters<T>` | 提取参数类型 | 反射 API |
| `Awaited<T>` | 提取 Promise 内部类型 | 无 |

---

## 三、运行时对比

### 3.1 Bun vs Node.js vs JVM

| 特性 | Bun | Node.js | JVM |
|------|-----|---------|-----|
| **启动速度** | ~5ms | ~100ms | ~500ms-2s |
| **包管理** | 内置 bun install | npm/yarn/pnpm | Maven/Gradle |
| **测试** | 内置 bun test | Jest/Mocha | JUnit |
| **TypeScript** | 原生支持 | 需要编译 | 无 |
| **SQLite** | 内置 bun:sqlite | 需要依赖 | JDBC |
| **FFI** | 内置 | node-ffi-napi | JNI |
| **ESM** | 原生支持 | 部分支持 | 无 |
| **WebSocket** | 内置 | ws 库 | 无 |
| **生态** | 兼容 npm | 最大 | 最大 |
| **GC** | Go 风格 GC | V8 GC | 分代 GC |
| **内存** | ~50MB 基线 | ~80MB 基线 | ~200MB 基线 |
| **并发** | 单线程 + Worker | 单线程 + Worker | 多线程 |

### 3.2 Bun 特有 API

```typescript
// BunShell — 内置 Shell 执行
const result = await Bun.$`git status --porcelain`

// Bun.file — 高效文件读取
const content = await Bun.file("config.json").json()

// Bun.build — 快速打包
await Bun.build({
  entrypoints: ["src/index.ts"],
  outdir: "dist",
})

// Bun.serve — 内置 HTTP 服务器
Bun.serve({
  port: 3000,
  fetch(req) {
    return new Response("Hello")
  },
})

// Bun.write — 文件写入
await Bun.write("output.txt", "content")
```

---

## 四、tsconfig.json 关键字段

| 字段 | 值 | 说明 |
|------|-----|------|
| `compilerOptions.target` | `"ES2022"` | 编译目标 |
| `compilerOptions.module` | `"ESNext"` | 模块系统 |
| `compilerOptions.moduleResolution` | `"bundler"` | 模块解析策略 |
| `compilerOptions.strict` | `true` | 启用所有严格检查 |
| `compilerOptions.noUncheckedIndexedAccess` | `true` | 数组索引可能 undefined |
| `compilerOptions.paths` | `{ "@/*": ["./src/*"] }` | 路径别名 |
| `compilerOptions.jsx` | `"preserve"` | JSX 处理 |
| `compilerOptions.jsxImportSource` | `"solid-js"` | JSX 运行时 |
| `compilerOptions.types` | `["bun-types"]` | 类型声明 |
| `include` | `["src"]` | 编译范围 |
| `exclude` | `["node_modules", "dist"]` | 排除目录 |

---

## 五、OpenCode 中的 TS 模式速查

| 模式 | 示例 | 说明 |
|------|------|------|
| **namespace** | `export namespace Provider { }` | 逻辑分组，非 Java package |
| **type + interface** | `type Model = z.infer<typeof Model>` | 类型别名（编译后消失） |
| **const assertion** | `const enum` | 编译时常量 |
| **branded types** | `Schema.brand("ProviderID")` | 运行时标记类型 |
| **satisfies** | `obj satisfies Interface` | 类型检查但不拓宽 |
| **as const** | `{ status: "active" } as const` | 字面量类型推断 |
| **template literal** | `` `${providerID}_${toolName}` `` | 字符串模板 |
| **utility pipe** | `.pipe(Effect.map(...))` | 函数组合 |
