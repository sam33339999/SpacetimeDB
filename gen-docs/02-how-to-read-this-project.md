# 如何閱讀與理解此專案

本文件是為想深入理解 SpacetimeDB 原始碼的開發者所準備的導覽指南。

---

## 專案根目錄結構

```
SpacetimeDB/
├── crates/                # 核心 Rust 程式碼（所有子套件）
├── docs/                  # 官方文件（Docusaurus 網站）
├── sdks/                  # 客戶端 SDK（TypeScript 等）
├── modules/               # 範例與測試模組
├── smoketests/            # 整合測試（冒煙測試）
├── templates/             # 模組專案範本
├── demo/                  # 示範程式
├── tools/                 # 開發輔助工具
├── images/                # 圖片資源
├── gen-docs/              # 繁體中文說明文件（本資料夾）
├── Cargo.toml             # Rust 工作區設定（Workspace）
├── Cargo.lock             # 相依套件版本鎖定
├── README.md              # 英文 README
├── README_zh-TW.md        # 繁體中文 README（翻譯版）
└── docker-compose.yml     # Docker Compose 設定
```

---

## crates/ 目錄詳解

這是整個專案最重要的目錄，包含所有核心 Rust 套件（crate）。

```
crates/
├── standalone/            # ★ 獨立執行模式的主程式（入口點）
├── cli/                   # ★ spacetime CLI 工具
├── update/                # spacetime 版本管理工具
├── core/                  # ★ 核心資料型別與基礎結構
├── lib/                   # 公開 API 函式庫（供模組作者使用）
├── sats/                  # ★ SpacetimeDB 代數型別系統（SATS）
├── schema/                # 資料庫 Schema 定義與驗證
├── table/                 # 資料表實作
├── datastore/             # ★ 資料儲存引擎
├── commitlog/             # 預寫日誌（WAL）實作
├── durability/            # 持久化介面與實作
├── snapshot/              # 資料庫快照功能
├── execution/             # 查詢執行引擎
├── vm/                    # WebAssembly 虛擬機器介面
├── query/                 # 查詢語言解析與執行
├── query-builder/         # 查詢建構器
├── sql-parser/            # SQL 語法解析器
├── sqltest/               # SQL 測試框架
├── physical-plan/         # 查詢物理計畫
├── expr/                  # 查詢運算式（表示式樹）
├── subscription/          # ★ 訂閱機制實作
├── client-api/            # 客戶端 API 伺服器
├── client-api-messages/   # 客戶端 ↔ 伺服器通訊協定訊息
├── codegen/               # ★ 程式碼生成（為客戶端 SDK 生成型別）
├── bindings/              # Rust 模組綁定（#[spacetimedb::table] 等巨集）
├── bindings-macro/        # 程序宏（Procedural Macros）
├── bindings-sys/          # 底層系統綁定（WASM host functions）
├── bindings-cpp/          # C++ 綁定
├── bindings-csharp/       # C# 綁定
├── bindings-typescript/   # TypeScript 綁定
├── auth/                  # 身份認證模組
├── primitives/            # 基礎型別（如 TableId、ReducerId）
├── data-structures/       # 通用資料結構（如 Trie、BloomFilter）
├── metrics/               # 效能指標收集
├── memory-usage/          # 記憶體使用量追蹤
├── bench/                 # 效能基準測試
├── paths/                 # 檔案路徑管理
├── fs-utils/              # 檔案系統工具
├── guard/                 # 安全防護工具
├── testing/               # 測試輔助工具
└── pg/                    # PostgreSQL 相容層（實驗性）
```

---

## 閱讀順序建議

如果你想從零開始理解這個專案，建議按以下順序閱讀：

### 第一步：理解資料型別系統（SATS）

**crates/sats/**

SATS（SpacetimeDB Algebraic Type System）是整個系統的型別基礎，理解它是理解一切的前提。

閱讀重點：
- `lib.rs` / `src/` — 代數型別的定義（積型、和型、內建型別）
- `bsatn/` — 二進位序列化格式（Binary Serialization for Algebraic Types in N dimensions）

### 第二步：理解資料表與資料儲存

**crates/table/** → **crates/datastore/**

- `table/` — 單一資料表的實作（列的儲存、索引）
- `datastore/` — 多資料表的整合管理，事務處理

### 第三步：理解持久化機制

**crates/commitlog/** → **crates/durability/** → **crates/snapshot/**

- `commitlog/` — WAL（預寫日誌）的實作，確保資料不因崩潰而丟失
- `durability/` — 定義持久化的抽象介面
- `snapshot/` — 資料庫快照，加速重啟後的狀態恢復

### 第四步：理解查詢引擎

**crates/sql-parser/** → **crates/expr/** → **crates/physical-plan/** → **crates/execution/**

- `sql-parser/` — 將 SQL 字串解析為 AST
- `expr/` — 查詢表示式樹
- `physical-plan/` — 查詢的物理執行計畫
- `execution/` — 實際執行查詢

### 第五步：理解訂閱機制

**crates/subscription/**

這是 SpacetimeDB 即時同步的核心。閱讀此模組了解如何：
- 追蹤客戶端的訂閱查詢
- 在資料變更時計算差異（delta）
- 將更新推送給相關客戶端

### 第六步：理解模組綁定與程式碼生成

**crates/bindings/** → **crates/bindings-macro/** → **crates/codegen/**

- `bindings/` — `#[spacetimedb::table]`、`#[spacetimedb::reducer]` 等巨集的實作
- `bindings-macro/` — 程序宏的底層實作
- `codegen/` — 根據模組 Schema 為客戶端 SDK 生成型別安全的程式碼

### 第七步：理解 CLI 與獨立模式

**crates/cli/** → **crates/standalone/**

- `cli/` — `spacetime` 指令列工具，包含 `publish`、`start`、`logs` 等子指令
- `standalone/` — 單機版 SpacetimeDB 伺服器，整合所有元件

---

## 重要設計模式

### 1. WebAssembly 沙箱

模組被編譯為 WebAssembly（WASM）位元碼，在安全沙箱中執行。這確保了：
- 模組無法直接存取主機檔案系統或網路
- 多個模組可在同一個程序中安全執行
- 跨語言支援（Rust、C# 都編譯為 WASM）

相關程式碼：`crates/vm/`（WASM 執行環境介面）、`crates/bindings-sys/`（WASM host functions）

### 2. 代數型別系統（SATS）

SpacetimeDB 使用自己的型別系統，而非直接使用 SQL 型別或特定語言的型別系統，這讓跨語言的型別映射更加精確。

### 3. 訂閱差異計算（Subscription Delta）

每當資料變更（Reducer 執行後），系統會：
1. 計算哪些訂閱查詢的結果集發生了變化
2. 只將「差異」（新增/刪除的列）推送給訂閱的客戶端
3. 客戶端在本地應用這些差異，維護一個最新的本地快取

### 4. 預寫日誌（WAL）+ 快照

- **commitlog**：每次 Reducer 執行的結果都寫入日誌
- **snapshot**：定期對完整資料庫狀態進行快照
- 重啟時：從最新快照載入，再重播快照之後的日誌

---

## 如何運行測試

```bash
# 運行所有 Rust 測試
cargo test

# 運行特定套件的測試
cargo test -p spacetimedb-datastore

# 運行冒煙測試（需要先啟動 SpacetimeDB）
cd smoketests
# 查看 smoketests/ 目錄下的說明
```

---

## 如何閱讀 docs/ 目錄

`docs/` 資料夾是官方文件網站的原始碼，使用 Docusaurus 建置。

```
docs/
├── docs/               # 文件原始 Markdown 檔案
│   ├── 00100-intro/    # 入門指南與快速開始
│   ├── 00200-core-concepts/  # 核心概念
│   └── 00300-resources/      # 參考資料
├── versioned_docs/     # 各版本的快照文件
├── src/                # 自訂 React 元件
└── static/             # 靜態資源
```

在本地運行文件網站：
```bash
cd docs
pnpm install
pnpm dev
```

---

## 進階：程式碼生成流程

SpacetimeDB 的「程式碼生成」功能讓開發者無需手動撰寫客戶端的資料型別定義：

1. 你在伺服器端（模組）定義資料表和 Reducer
2. 執行 `spacetime generate` 指令
3. SpacetimeDB 讀取模組的 Schema，自動生成對應語言（TypeScript / C# / Rust）的型別定義
4. 客戶端使用這些生成的型別，與伺服器進行型別安全的通訊

相關實作：`crates/codegen/`
