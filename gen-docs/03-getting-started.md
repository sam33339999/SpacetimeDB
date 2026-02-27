# 快速上手：安裝、啟動與第一個模組

本指南將帶你從零開始，完成 SpacetimeDB 的安裝與第一個「Hello World」等級的應用程式。

---

## 第一步：安裝 SpacetimeDB CLI

### macOS / Linux

```bash
curl -sSf https://install.spacetimedb.com | sh
```

安裝完成後，重新開啟終端機，確認安裝成功：

```bash
spacetime --version
```

### Windows（PowerShell）

```powershell
iwr https://windows.spacetimedb.com -useb | iex
```

### 使用 Docker（免安裝，快速體驗）

如果你不想在本機安裝，可以直接用 Docker 啟動：

```bash
docker run --rm --pull always -p 3000:3000 clockworklabs/spacetime start
```

---

## 第二步：啟動本地 SpacetimeDB 節點

```bash
spacetime start
```

預設情況下，SpacetimeDB 會在 `http://localhost:3000` 啟動。你可以開啟瀏覽器確認服務正在執行。

---

## 第三步：安裝 Rust 與 WASM 工具鏈（伺服器端模組必要）

如果你要撰寫 Rust 模組，需要安裝 Rust 工具鏈和 WASM 目標：

```bash
# 安裝 Rust
curl https://sh.rustup.rs -sSf | sh

# 新增 WASM 編譯目標
rustup target add wasm32-unknown-unknown
```

---

## 第四步：建立你的第一個 SpacetimeDB 模組（Rust 版本）

### 初始化專案

```bash
# 建立新的模組專案
spacetime init --lang rust my-first-module
cd my-first-module
```

這會建立一個 Rust 專案，結構如下：

```
my-first-module/
├── Cargo.toml
└── src/
    └── lib.rs
```

### 查看預設的 lib.rs

```rust
use spacetimedb::{spacetimedb, Identity, ReducerContext, Table};

// 定義資料表
#[spacetimedb::table(name = messages, public)]
pub struct Message {
    #[primary_key]
    #[auto_inc]
    pub id: u64,
    pub sender: Identity,
    pub text: String,
}

// 定義 Reducer（可修改資料的函式）
#[spacetimedb::reducer]
pub fn send_message(ctx: &ReducerContext, text: String) {
    Message::insert(Message {
        id: 0,  // auto_inc 會自動填入
        sender: ctx.sender,
        text,
    });
}
```

### 撰寫一個簡單的聊天室模組

修改 `src/lib.rs`：

```rust
use spacetimedb::{spacetimedb, Identity, ReducerContext, Table};

// 使用者資料表
#[spacetimedb::table(name = users, public)]
pub struct User {
    #[primary_key]
    pub identity: Identity,
    pub name: String,
    pub online: bool,
}

// 訊息資料表
#[spacetimedb::table(name = messages, public)]
pub struct Message {
    #[auto_inc]
    #[primary_key]
    pub id: u64,
    pub sender_identity: Identity,
    pub text: String,
    pub sent_at: u64,
}

// 設定使用者名稱（首次連線時呼叫）
#[spacetimedb::reducer]
pub fn set_name(ctx: &ReducerContext, name: String) {
    if let Some(user) = ctx.db.users().identity().find(&ctx.sender) {
        ctx.db.users().identity().update(User {
            identity: ctx.sender,
            name,
            ..user
        });
    } else {
        ctx.db.users().insert(User {
            identity: ctx.sender,
            name,
            online: true,
        });
    }
}

// 發送訊息
#[spacetimedb::reducer]
pub fn send_message(ctx: &ReducerContext, text: String) {
    ctx.db.messages().insert(Message {
        id: 0,
        sender_identity: ctx.sender,
        text,
        sent_at: ctx.timestamp.micros_since_epoch,
    });
}

// 使用者連線時自動呼叫
#[spacetimedb::reducer(client_connected)]
pub fn on_connect(ctx: &ReducerContext) {
    if let Some(user) = ctx.db.users().identity().find(&ctx.sender) {
        ctx.db.users().identity().update(User {
            online: true,
            ..user
        });
    }
}

// 使用者斷線時自動呼叫
#[spacetimedb::reducer(client_disconnected)]
pub fn on_disconnect(ctx: &ReducerContext) {
    if let Some(user) = ctx.db.users().identity().find(&ctx.sender) {
        ctx.db.users().identity().update(User {
            online: false,
            ..user
        });
    }
}
```

---

## 第五步：發布模組到本地 SpacetimeDB

```bash
# 在模組專案目錄下執行
spacetime publish --server localhost:3000 my-chat-app
```

成功後，你會看到模組已被發布，並顯示一組資料庫地址（Database Address）。

---

## 第六步：為客戶端生成型別定義

SpacetimeDB 可以根據你的模組 Schema 自動生成客戶端程式碼：

```bash
# 生成 TypeScript 客戶端程式碼
spacetime generate --lang typescript --out-dir ./client/src/module_bindings --server localhost:3000 my-chat-app

# 生成 C# 客戶端程式碼
spacetime generate --lang csharp --out-dir ./client/module_bindings --server localhost:3000 my-chat-app
```

---

## 第七步：撰寫 TypeScript 客戶端

安裝 SpacetimeDB TypeScript SDK：

```bash
npm install @clockworklabs/spacetimedb-sdk
```

基本連線範例（`client.ts`）：

```typescript
import { SpacetimeDBClient, Identity } from "@clockworklabs/spacetimedb-sdk";
import { Message, User, sendMessage, setName } from "./module_bindings";

// 建立客戶端連線
const client = new SpacetimeDBClient(
    "localhost:3000",  // SpacetimeDB 伺服器地址
    "my-chat-app",     // 資料庫名稱
    undefined          // Token（首次連線留空）
);

// 訂閱所有訊息和使用者資料
client.subscribe(["SELECT * FROM messages", "SELECT * FROM users"]);

// 監聽訂閱資料初始化完成事件
client.on("initialStateSync", () => {
    console.log("資料同步完成！");
    
    // 讀取所有訊息
    const allMessages = Message.all();
    console.log("現有訊息數量：", allMessages.length);
    
    // 設定使用者名稱
    setName("我的名字");
    
    // 發送訊息
    sendMessage("Hello, SpacetimeDB！");
});

// 監聽新訊息
Message.onInsert((message, reducerEvent) => {
    const sender = User.filterByIdentity(message.senderIdentity);
    console.log(`${sender?.name ?? "未知使用者"}: ${message.text}`);
});

// 連接到伺服器
client.connect();
```

---

## 常用 CLI 指令

| 指令 | 說明 |
|------|------|
| `spacetime start` | 啟動本地 SpacetimeDB 伺服器 |
| `spacetime publish <name>` | 發布模組到 SpacetimeDB |
| `spacetime generate --lang <lang>` | 生成客戶端程式碼 |
| `spacetime logs <name>` | 查看模組的運行日誌 |
| `spacetime sql <name> "<query>"` | 執行 SQL 查詢 |
| `spacetime delete <name>` | 刪除已發布的模組 |
| `spacetime call <name> <reducer>` | 直接呼叫 Reducer |
| `spacetime version list` | 列出已安裝的版本 |
| `spacetime version use <version>` | 切換使用的版本 |

---

## 下一步

- 閱讀[官方文件](https://spacetimedb.com/docs)了解更多進階功能
- 查看 `modules/` 目錄中的範例模組
- 加入 [Discord 社群](https://discord.gg/spacetimedb)獲得協助
- 閱讀 [04-comparison.md](./04-comparison.md) 了解 SpacetimeDB 與其他技術的比較
