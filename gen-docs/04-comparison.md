# SpacetimeDB 與其他技術的比較

本文件比較 SpacetimeDB 與其他常見技術的異同，幫助你評估是否應該在專案中採用 SpacetimeDB。

---

## 總覽

| 特性 | SpacetimeDB | 傳統 DB + 伺服器 | Firebase | Supabase | Redis |
|------|-------------|----------------|----------|----------|-------|
| 即時同步 | ✅ 原生支援 | ❌ 需自行實作 | ✅ 原生支援 | ✅ 透過 Realtime | ✅ Pub/Sub |
| 應用邏輯位置 | 資料庫內部（模組） | 獨立應用伺服器 | 雲端函式（Cloud Functions） | 資料庫函式 + Edge Functions | 客戶端 |
| 資料模型 | 關聯式 | 關聯式 | NoSQL（文件型） | 關聯式（PostgreSQL） | 鍵值 / 資料結構 |
| 程式語言 | Rust / C# | 任意 | JS / TS / Python 等 | 任意（Edge Functions） | 任意（客戶端） |
| 本地資料快取 | ✅ SDK 內建 | ❌ 需自行管理 | ✅ 內建 | ⚠️ 部分支援 | ❌ |
| 適合即時遊戲 | ✅ 極佳 | ⚠️ 需大量工程 | ⚠️ 有延遲上限 | ⚠️ 有限制 | ⚠️ 需自行整合 |
| 授權 | BSL 1.1 → AGPL | 依各資料庫 | 專有（Google） | 開源（Apache 2.0） | BSD 3-Clause |

---

## SpacetimeDB vs 傳統「資料庫 + 應用伺服器」架構

### 傳統架構

```
客戶端 ──HTTP/WS──► 應用伺服器（Node.js / Go / Java）──SQL──► PostgreSQL / MySQL
```

**優點：**
- 技術棧成熟，工具豐富
- 可自由選擇語言與框架
- 容易橫向擴展各層

**缺點：**
- 需要管理多個元件（DB + App Server）
- 即時功能需要額外實作（WebSocket + 訂閱邏輯）
- 資料序列化與反序列化開銷大
- 延遲較高（客戶端 → 伺服器 → 資料庫 → 伺服器 → 客戶端）

### SpacetimeDB 架構

```
客戶端 ──WebSocket──► SpacetimeDB（資料庫 + 模組邏輯）
```

**優點：**
- 架構極簡，只有一個部署目標
- 即時同步是一等公民，不需要額外實作
- 低延遲（應用邏輯在記憶體中執行，無需網路往返）
- 型別安全的端對端程式碼生成

**缺點：**
- 語言選擇受限（模組目前只支援 Rust 和 C#）
- 生態系較新，工具不如傳統技術棧成熟
- 不適合批次處理或 OLAP 工作負載
- 需要學習新的程式設計模型（Reducer、訂閱等）

---

## SpacetimeDB vs Firebase

### Firebase

Firebase 是 Google 提供的即時後端服務，提供 Firestore（NoSQL 資料庫）和 Realtime Database。

**Firebase 優勢：**
- 完全託管，零運維
- 前端開發者友好
- 龐大的 Google 生態系整合

**Firebase 劣勢：**
- NoSQL 資料模型，複雜查詢困難
- 定價隨使用量快速增加
- 資料綁定在 Google 生態（廠商鎖定）
- 複雜的業務邏輯需要 Cloud Functions，冷啟動延遲高

**何時選擇 SpacetimeDB 而非 Firebase：**
- 需要關聯式資料模型和複雜查詢
- 建置低延遲的即時應用（遊戲、協作工具）
- 不希望被 Google 生態系鎖定
- 需要在本地或私有雲自托管

**何時選擇 Firebase 而非 SpacetimeDB：**
- 快速原型開發，不需要高效能
- 已深度整合 Google 生態系
- 團隊對 Rust/C# 模組不熟悉

---

## SpacetimeDB vs Supabase

### Supabase

Supabase 是 Firebase 的開源替代方案，基於 PostgreSQL。

**Supabase 優勢：**
- 完整的 PostgreSQL 功能
- 開源，可自托管
- 豐富的生態系工具（PostgREST、pg_vector 等）
- Row Level Security（行級安全性）
- 完整的 SQL 支援

**Supabase 劣勢：**
- 即時功能（Realtime）有效能上限
- 需要額外的邊緣函式（Edge Functions）執行業務邏輯
- 不像 SpacetimeDB 那樣將邏輯深度整合進資料庫

**何時選擇 SpacetimeDB 而非 Supabase：**
- 需要極低延遲的即時同步
- 應用邏輯高度集中（如多人遊戲的物理模擬）
- 客戶端需要本地快取和離線支援

**何時選擇 Supabase 而非 SpacetimeDB：**
- 需要完整的 PostgreSQL 功能（複雜 JOIN、窗口函式等）
- 已有大量 SQL 查詢和工具
- 需要豐富的第三方整合（pg_vector、PostGIS 等）

---

## SpacetimeDB vs Redis

### Redis

Redis 是一個高效能的記憶體資料結構儲存，常用作快取或訊息佇列。

**比較觀點：**

SpacetimeDB 和 Redis 都是記憶體優先的系統，但用途完全不同：

| 面向 | SpacetimeDB | Redis |
|------|-------------|-------|
| 主要用途 | 完整的應用後端 | 快取 / 訊息佇列 / 輔助儲存 |
| 資料模型 | 關聯式（表格） | 鍵值、Hash、List、Set、Stream 等 |
| 查詢語言 | SQL | 專有指令集 |
| 應用邏輯 | 可在資料庫內執行（模組） | 不支援 |
| 即時推送 | 原生支援（訂閱） | Pub/Sub（較低階） |

SpacetimeDB 和 Redis 通常不是直接競爭關係，Redis 更常作為 SpacetimeDB 或傳統資料庫的補充。

---

## SpacetimeDB vs 智能合約（Smart Contracts）

README 中提到 SpacetimeDB 的概念類似智能合約。讓我們來看看異同：

| 面向 | SpacetimeDB 模組 | 智能合約（Ethereum 等） |
|------|-----------------|----------------------|
| 執行環境 | 私有伺服器（可信任） | 去中心化區塊鏈節點 |
| 執行速度 | 毫秒級 | 秒到分鐘級 |
| 資料儲存 | 記憶體 + WAL | 鏈上狀態（極其昂貴） |
| 可信任性來源 | 服務提供者 | 去中心化共識 |
| 適用場景 | 高效能應用 | 需要去信任的金融應用 |

SpacetimeDB 借鑒了智能合約「邏輯即狀態」的概念，但捨棄了區塊鏈的去中心化特性，換取了極高的效能。

---

## 選擇建議

### 適合使用 SpacetimeDB 的場景

✅ **多人即時遊戲**（特別是 MMORPG、FPS、RTS）
- 需要毫秒級延遲
- 需要大量玩家狀態同步
- 後端邏輯可以用 Rust/C# 撰寫

✅ **即時協作工具**（共同編輯、即時白板）
- 多用戶同時修改同一份資料
- 需要衝突解決和強一致性

✅ **即時聊天與社交應用**
- 需要訊息即時推送
- 需要線上狀態追蹤

✅ **物聯網（IoT）資料串流**
- 高頻率的感測器資料寫入
- 需要即時監控和訂閱

---

### 不適合使用 SpacetimeDB 的場景

❌ **複雜的分析查詢（OLAP）**
- 需要聚合大量歷史資料
- 建議使用 ClickHouse、DuckDB 或 BigQuery

❌ **需要豐富 SQL 生態系工具**
- 已有大量 SQL Migration、ORM 等工具
- 建議使用 PostgreSQL + Supabase

❌ **批次資料處理**
- ETL 管道、報表生成
- 建議使用傳統資料庫 + Spark / dbt

❌ **模組語言不支援的情況**
- 如果你的後端邏輯只能用 Python / Java / Go 等語言撰寫
- SpacetimeDB 模組目前只支援 Rust 和 C#

---

## 總結

SpacetimeDB 代表了一種**全新的後端架構思維**：與其讓資料庫和應用伺服器分離，不如讓它們合二為一。這種架構在特定場景（特別是即時多人應用）下能夠帶來顯著的效能優勢和開發體驗改善。

如果你正在建置一個需要**高頻率即時資料同步**的應用，並且願意採用 Rust 或 C# 作為後端語言，SpacetimeDB 值得認真考慮。

對於其他場景，傳統的 PostgreSQL + 應用伺服器架構或 Firebase / Supabase 等 BaaS 服務可能仍是更成熟的選擇。
