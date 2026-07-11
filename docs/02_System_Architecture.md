# 02_System_Architecture.md

# Personal AI OS — 系統架構

> **版本**：v2.0  
> **性質**：架構設計文件  
> **約束力**：必須遵循 01_AI_OS_Principles.md  
> **適用對象**：所有 AI 開發者

---

## 目錄

1. [整體架構圖](#1-整體架構圖)
2. [各層介紹](#2-各層介紹)
3. [資料流](#3-資料流)
4. [Agent 分工](#4-agent-分工)
5. [Workflow 定義](#5-workflow-定義)
6. [Capability 定義](#6-capability-定義)
7. [Tool 定義](#7-tool-定義)
8. [Model Layer](#8-model-layer)
9. [Memory 架構](#9-memory-架構)
10. [Response Formatter](#10-response-formatter)
11. [Permission & Audit](#11-permission--audit)
12. [未來預留擴充](#12-未來預留擴充)
13. [目錄結構建議](#13-目錄結構建議)

---

## 1. 整體架構圖

```
┌─────────────────────────────────────────────────────────────┐
│                         User                                │
└──────────────────────┬──────────────────────────────────────┘
                       │ Natural Language
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                    Open WebUI                               │
│              (Agent A: Assistant Agent)                     │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                 Intent Recognition                          │
│              (理解使用者需求)                              │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                    Planner / Router                         │
│              (AI 規劃與建議)                               │
│              ⚠️ 不可自主執行                               │
└──────────────────────┬──────────────────────────────────────┘
                       │ Execution Plan (需人工確認)
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                 Permission Check                            │
│              (權限檢查)                                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                     Workflow                                │
│              (完整任務流程)                                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                    Capability                               │
│              (可重複使用能力)                              │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                      Tool                                   │
│              (實際執行工具)                                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                   Model Router                              │
│              (模型選擇與路由)                              │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                  Model Adapter                              │
│              (模型介接層)                                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                       LLM                                   │
│         (Local / FreeLLMAPI / Paid Model)                 │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│               Response Formatter                            │
│              (統一回應格式)                                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ↓
┌─────────────────────────────────────────────────────────────┐
│                      User                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. 各層介紹

### 2.1 User（使用者）

- 使用自然語言提出需求
- 不需要知道底層流程
- 負責核准、拒絕、執行

### 2.2 Open WebUI（Agent A：Assistant Agent）

**工具**：Open WebUI

**用途**：
- 日常聊天
- 理解需求
- 查詢資訊
- 分析問題
- 呼叫 Workflow
- 執行已建立功能

**使用方式**：自然語言

例如：
```
使用者：分析股票
AI：判斷 Intent → 呼叫 Stock Analysis Workflow
```

### 2.3 Intent Recognition（意圖識別）

使用者不需要指定 Workflow。

例如：
```
使用者：幫我分析股票
AI 判斷：Intent = Stock Analysis
再找：Stock Analysis Workflow
```

### 2.4 Planner / Router（規劃器）

**角色**：Agent 的決策核心

**負責**：
- 理解需求
- 選擇 Workflow
- 判斷是否需要多個 Workflow
- 組合 Capability
- 提出 Workflow Proposal

**不直接執行低階工具**。

**⚠️ 重要限制**：
- Planner 只能規劃與建議，不能自主執行
- 產生的 Execution Plan 需經過 Permission Check 與 Human Approval
- 導入時機：Workflow 數量 > 5 且穩定運作後

### 2.5 Permission Check（權限檢查）

- 檢查 Workflow 是否有權使用特定 Capability 與 Tool
- 檢查是否需要人工核准
- 採用 Fail Closed（預設拒絕）

### 2.6 Workflow（工作流程）

**定義**：完成一個完整任務的流程。

**類型**：
- **Standard Workflow**：已建立並驗證的固定流程
- **Dynamic Workflow**：Planner 自動組合 Capability 形成的臨時流程

**範例**：
- Stock Analysis Workflow
- Daily Report Workflow
- Email Summary Workflow
- News Analysis Workflow
- Document Analysis Workflow
- Coding Workflow

### 2.7 Capability（能力模組）

**定義**：可重複使用的能力，不是完整任務。

**範例**：
- Stock Data Query
- News Analyzer
- Technical Analyzer
- Fundamental Analyzer
- Risk Analyzer
- Report Generator
- Document Parser

**特性**：
- 多個 Workflow 可以共用
- 每個 Capability 有自己的權限（Capability Sandbox）

### 2.8 Tool（工具）

**定義**：最底層實際執行工具。

**範例**：
- Stock API
- News API
- Financial API
- Email API
- Calendar API
- Web Search
- Database
- File System

**特性**：
- Capability 呼叫 Tool
- Tool 可替換

### 2.9 Model Router（模型路由器）

**負責**：
- 根據任務選擇適合的模型
- 根據 Privacy 等級選擇模型
- 根據成本選擇模型

**模型優先順序**：
```
Local Model
    ↓ 能力不足
詢問使用者
    ↓
FreeLLMAPI
    ↓ 仍不足
Paid Model (GPT / Claude)
```

### 2.10 Model Adapter（模型介接層）

**目的**：統一不同模型的介面。

**特性**：
- 不因模型更新而影響系統架構
- 支援 Ollama、OpenAI、Anthropic、FreeLLMAPI 等

### 2.11 LLM（大型語言模型）

**三種模式**：
- **Local Only**：只使用本地模型
- **Local First（預設）**：本地優先，不足時人工確認後外送
- **External Allowed**：使用者指定使用 GPT / Claude 等

### 2.12 Response Formatter（回應格式化）

**目的**：統一不同 AI 的輸出格式。

**統一格式**：
- Summary
- Analysis
- Data Source
- Confidence
- Risk / Limitation
- Next Step

**不同 Workflow 可以有專屬格式**。

---

## 3. 資料流

### 3.1 標準流程（已有 Workflow）

```
User Request
    ↓
Intent Recognition
    ↓
Stock Analysis Workflow
    ↓
Capability
    ↓
Tool
    ↓
LLM
    ↓
Response Formatter
    ↓
User
```

### 3.2 無適合 Workflow（Dynamic Workflow）

```
User Request
    ↓
Intent Recognition
    ↓
Planner
    ↓
組合 Capability
    ↓
Workflow Proposal
    ↓
人工確認
    ↓
執行
    ↓
（可選）保存為 Standard Workflow
```

### 3.3 外部資料進入系統（Zero Trust）

```
Internet
    ↓
Download
    ↓
Security Scan（安全掃描）
    ↓
Content Classification（內容分類）
    ↓
Privacy Filter（隱私過濾）
    ↓
Workflow
```

**注意**：任何外部資料（Email、PDF、網頁、RSS、API、MCP、Plugin、LLM 回覆）都視為不可信資料。

---

## 4. Agent 分工

### 4.1 Agent A：Assistant Agent（使用者 AI 助理）

**工具**：Open WebUI

**用途**：
- 日常聊天
- 分析問題
- 查詢資料
- 理解使用者需求
- 呼叫 Workflow
- 呼叫 Capability
- 管理 Memory

**使用者只需要自然語言**，不需要知道背後使用哪個 Workflow。

### 4.2 Agent B：Development Agent（AI 工程師）

**工具**：Codex / Claude Code

**用途**：
- 建立 Workflow
- 建立 Capability
- 建立程式
- 修改程式
- 建立 Docker
- 建立 API 整合
- 維護 Git

**定位**：不是日常執行 Agent，而是建立與維護 AI OS。

**開發模式**：
- **Design Mode（預設）**：分析需求、提出架構、提出修改方案（禁止修改程式）
- **Implementation Mode**：人工確認後，才允許修改程式、建立檔案、執行測試

---

## 5. Workflow 定義

### 5.1 Workflow 類型

#### Standard Workflow（固定 Workflow）

已建立並驗證的標準流程。

範例：
- Stock Analysis Workflow
- Daily Report Workflow
- Email Summary Workflow
- News Analysis Workflow
- Document Analysis Workflow
- Coding Workflow

#### Dynamic Workflow（動態 Workflow）

當沒有適合的 Standard Workflow 時，Planner 自動組合 Capability 形成。

流程：
```
需求：分析 AI 類股今天是否值得注意
Planner 建議：
Stock Query + Industry Filter + News Analysis + Technical Analysis + Risk Analysis
形成：Workflow Proposal
```

### 5.2 Workflow 使用模式

#### 模式 A：已有 Workflow

```
使用者：分析股票
Intent → Stock Analysis Workflow → Capability → Tool → LLM → 回答
```

#### 模式 B：沒有 Workflow

```
Intent → Planner → 組合 Capability → Workflow Proposal → 人工確認 → 執行
```

#### 模式 C：值得重複使用

執行後，AI 詢問：
> 是否保存成新的 Standard Workflow？

確認後，加入 Workflow Repository。

### 5.3 Workflow 與排程

Workflow 同時支援：
- **手動**：使用者主動觸發
- **排程**：透過 n8n Scheduler 定時觸發

例如：
```
每天 07:00
Scheduler → n8n → Stock Analysis Workflow → Email
```

### 5.4 Workflow Engine

**建議**：n8n

**用途**：
- 排程
- API 串接
- Email
- Automation
- Workflow 執行

**定位**：n8n 不負責理解需求，只負責「已決定好的流程執行」。

---

## 6. Capability 定義

### 6.1 Capability 結構

每個 Capability 包含：
- **Purpose**：用途
- **Input**：輸入
- **Output**：輸出
- **Tool**：使用哪些 Tool
- **Model**：使用哪種模型（Local First）
- **Permission**：需要什麼權限

### 6.2 Capability 範例

#### News Analysis

- **Purpose**：分析新聞
- **Input**：新聞內容
- **Output**：情緒分析、關鍵字、摘要
- **Tool**：News API
- **Model**：Local First
- **Permission**：Level 0（只讀）

#### Technical Analysis

- **Purpose**：技術分析
- **Input**：股票資料
- **Output**：技術指標（MACD、RSI、KDJ 等）
- **Tool**：Stock API、Database
- **Model**：Local First
- **Permission**：Level 0（只讀）

### 6.3 Capability 共用

多個 Workflow 可以共用同一個 Capability。

例如：
```
Workflow：股票分析
使用：Stock Data Query → News Analyzer → Technical Analyzer → Report Generator

Workflow：持股健檢
使用：Stock Data Query → Fundamental Analyzer → Risk Analyzer → Report Generator
```

### 6.4 Capability Sandbox

每個 Capability 有自己的權限限制：

例如：News Analysis Capability 只能：
- ✅ Read News
- ✅ Call LLM
- ✅ Output JSON
- ❌ rm -rf
- ❌ Git Push
- ❌ Docker Exec
- ❌ Broker API

---

## 7. Tool 定義

### 7.1 Tool 層

Tool 是最底層實際執行工具。

### 7.2 Tool 範例

| Tool | 用途 | 類型 |
|------|------|------|
| Stock API | 股票資料查詢 | 資料 API |
| News API | 新聞資料查詢 | 資料 API |
| Financial API | 財務資料查詢 | 資料 API |
| Email API | 郵件讀寫 | 通訊 API |
| Calendar API | 行事曆管理 | 通訊 API |
| Web Search | 網路搜尋 | 搜尋 |
| Database | 資料儲存 | 儲存 |
| File System | 檔案讀寫 | 儲存 |

### 7.3 Tool 註冊

所有 Tool 必須註冊到 Tool Registry，包含：
- 名稱
- 用途
- 輸入/輸出
- 權限需求
- 風險等級

---

## 8. Model Layer

### 8.1 模型策略

**核心**：Local First + Human Approval

### 8.2 模型使用規則

#### Rule 1：預設使用本地模型

例如：
- 文件整理
- 個人資料分析
- 一般聊天

#### Rule 2：私人資料優先本地

包含：
- Email
- 個人文件
- 投資帳戶
- 財務資料
- 私人筆記

#### Rule 3：本地模型不足時

**不能自動外送**。

流程：
```
Local Model
    ↓ 能力不足
詢問使用者
    ↓
選擇：FreeLLMAPI 或 Paid Model
```

### 8.3 模型模式

| 模式 | 說明 | 使用時機 |
|------|------|----------|
| Local Only | 只使用本地模型 | 高度敏感資料 |
| Local First（預設） | 本地優先，不足時人工確認後外送 | 一般使用 |
| External Allowed | 使用者指定 GPT / Claude 等 | 使用者明確要求 |

### 8.4 模型優先順序

```
Local Model (Ollama: Qwen, DeepSeek Coder, Llama)
    ↓
FreeLLMAPI
    ↓
Paid Model (GPT, Claude)
```

---

## 9. Memory 架構

### 9.1 Memory 分類

| 類型 | 用途 | 持久性 |
|------|------|--------|
| Preference Memory | 使用者偏好 | 永久 |
| Knowledge Memory | 知識累積 | 永久 |
| Workflow Memory | Workflow 歷史 | 永久 |
| Temporary Context | 當前對話上下文 | 暫時 |

### 9.2 Memory 安全規則

**禁止永久保存**：
- 密碼
- Token
- API Key
- Cookie
- Session ID
- 完整金融資料

---

## 10. Response Formatter

### 10.1 統一回應格式

所有 AI 回覆至少包含：
- **Summary**：摘要
- **Analysis**：分析
- **Data Source**：資料來源
- **Confidence**：信心度
- **Risk / Limitation**：風險與限制
- **Next Step**：下一步建議

### 10.2 Workflow 專屬格式

不同 Workflow 可以有專屬格式。

例如股票分析：
- 市場資訊
- 技術分析
- 基本面
- 風險
- AI 觀察
- 注意事項

---

## 11. Permission & Audit

### 11.1 Permission 層

- 檢查 Workflow 權限
- 檢查 Capability 權限
- 檢查 Tool 權限
- 採用 Fail Closed

### 11.2 Audit Log

記錄：
- 時間
- Workflow
- Capability
- Tool
- Model
- Data Source
- Action
- Permission
- Result
- Duration
- User Approval

用途：追蹤、除錯、安全檢查。

---

## 12. 未來預留擴充

| 元件 | 說明 | 預計階段 |
|------|------|----------|
| RAG | 檢索增強生成 | Phase 4 |
| Vector DB | 向量資料庫 | Phase 4 |
| MCP | Model Context Protocol | Phase 4 |
| Sandbox | AI 產生程式自動測試 | Phase 3-4 |
| LiteLLM | 模型路由與負載平衡 | Phase 2-3 |
| Plugin System | 外掛擴充 | Phase 4 |

---

## 13. 目錄結構建議

```
personal-ai-os/
│
├── docs/
│   ├── 00_PROJECT_STATUS.md
│   ├── 01_AI_OS_Principles.md
│   ├── 02_System_Architecture.md
│   ├── 03_Development_Standards.md
│   ├── 04_Workflow_Capability_Library.md
│   │
│   ├── workflows/          # Workflow 詳細文件（未來新增）
│   ├── capabilities/     # Capability 詳細文件（未來新增）
│   ├── prompts/           # Prompt 檔案（未來新增）
│   ├── templates/         # Response Template（未來新增）
│   └── diagrams/          # 架構圖（未來新增）
│
├── docker/                # Docker 設定
├── workflows/             # Workflow 程式碼
├── capabilities/          # Capability 程式碼
├── services/             # 微服務
├── models/                # 本地模型設定
├── configs/               # 設定檔
├── scripts/               # 腳本
├── tests/                 # 測試
├── compose.yaml
├── .env.example
├── .gitignore
└── README.md
```

---

> **本文件描述 AI OS 如何運作，後續所有開發必須遵循此架構。**
