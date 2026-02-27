# SpacetimeDB 專案概覽

## 什麼是 SpacetimeDB？

SpacetimeDB 是由 [Clockwork Labs](https://clockworklabs.io) 開發的新型資料庫平台，核心理念是**將應用程式伺服器與資料庫合而為一**。

### 傳統架構 vs SpacetimeDB 架構

**傳統三層架構：**
```
客戶端 (Client) ──► 應用伺服器 (App Server) ──► 資料庫 (Database)
```

**SpacetimeDB 架構：**
```
客戶端 (Client) ──► SpacetimeDB（資料庫 + 應用邏輯）
```

在 SpacetimeDB 中，開發者將應用邏輯以「**模組（Module）**」的形式上傳到資料庫，模組就像進階的預存程序（Stored Procedure），但功能遠比傳統預存程序強大。

---

## 核心概念

### 模組（Module）

模組是運行在 SpacetimeDB 內部的應用程式邏輯單元。你可以用 **Rust** 或 **C#** 撰寫模組，並透過 `spacetime publish` 指令上傳部署。

模組包含：
- **資料表（Tables）**：定義資料結構，類似關聯式資料庫的表格
- **Reducer**：可修改資料狀態的交易性函式（類似事件處理器）
- **程序（Procedures）**：可呼叫外部 HTTP 的函式
- **視圖（Views）**：唯讀的計算查詢

### 資料表（Tables）

SpacetimeDB 使用關聯式資料模型。資料表支援：
- 主鍵（Primary Key）、唯一索引（Unique Index）、普通索引
- 豐富的欄位型別（整數、浮點數、字串、布林值、枚舉、結構體、陣列等）
- 存取權限控制（公開 / 私有）
- 排程資料表（Schedule Tables）：用於時間觸發的操作

### Reducer

Reducer 是 SpacetimeDB 中最重要的概念之一。每個 Reducer 都是**原子性**執行的交易：
- 成功時所有變更一次性提交
- 失敗時所有變更全部回滾
- 客戶端可以呼叫 Reducer 來修改資料庫狀態

### 訂閱（Subscriptions）

客戶端可以訂閱特定查詢，SpacetimeDB 會在資料變更時主動推送更新：
- 客戶端在本地維護一個資料快取（Cache）
- 資料變更即時同步，無需輪詢（Polling）
- 這是 SpacetimeDB 實現低延遲的關鍵機制

### 身份認證（Authentication）

SpacetimeDB 支援 OpenID Connect（OIDC）進行身份驗證，並提供 **SpacetimeAuth** 這個受管的認證服務。你可以在模組中透過「Auth Claims」來獲取使用者身份與角色資訊，實作細緻的權限控制。

---

## 技術特性

| 特性 | 說明 |
|------|------|
| **全記憶體運行** | 所有應用狀態保存在記憶體中，實現極低延遲 |
| **WAL 持久化** | 透過預寫日誌（Write-Ahead Log）確保資料不丟失 |
| **即時訂閱** | 客戶端訂閱查詢，資料變更即時推送 |
| **原子性交易** | Reducer 執行保證 ACID 特性 |
| **WebAssembly 模組** | 模組編譯為 WASM，安全沙箱執行 |
| **多語言 SDK** | 支援 Rust、C#、TypeScript 客戶端 |

---

## 適用場景

SpacetimeDB 特別適合以下場景：

- 🎮 **多人線上遊戲**（MMORPG、即時策略遊戲等）
- 💬 **即時聊天與協作工具**
- 📊 **即時儀表板與監控系統**
- 🔄 **需要即時資料同步的應用程式**

SpacetimeDB **不適合**：
- 批次處理（Batch Processing）
- OLAP 分析型工作負載
- 需要傳統 SQL 生態系工具的場景

---

## 主要語言與 SDK

### 伺服器端模組語言
- **Rust**：效能最佳，生態系最完整
- **C#**：適合熟悉 .NET 生態的開發者（特別是 Unity 遊戲開發者）

### 客戶端 SDK
- **Rust SDK**
- **C# SDK**（Unity 整合優先）
- **TypeScript SDK**（Web 前端）
- **Unreal Engine SDK**（遊戲開發）

---

## 授權條款

SpacetimeDB 採用 **BSL 1.1（Business Source License）** 授權，幾年後自動轉換為 **AGPL v3.0（附連結例外）**。

重點：使用 SpacetimeDB 建置應用程式時，你的應用程式碼**不需要**開源（因為有連結例外條款）。
