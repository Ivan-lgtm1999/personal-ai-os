# 04_Workflow_Capability_Library.md

# Personal AI OS — Workflow & Capability Library

> **版本**：v2.0  
> **性質**：功能規格文件  
> **狀態**：經常更新  
> **適用對象**：所有 AI 開發者

---

## 目錄

1. [Workflow 清單](#1-workflow-清單)
2. [Capability 清單](#2-capability-清單)
3. [Response Template](#3-response-template)
4. [Workflow 詳細規格](#4-workflow-詳細規格)
5. [Capability 詳細規格](#5-capability-詳細規格)
6. [新增 Workflow 流程](#6-新增-workflow-流程)
7. [新增 Capability 流程](#7-新增-capability-流程)

---

## 1. Workflow 清單

### 1.1 目前 Workflow

| Workflow | 狀態 | 說明 | 優先順序 |
|----------|------|------|----------|
| **Stock Analysis** | ⏳ 規劃中 | 股票分析 | Phase 2 優先 |
| **Daily Information** | ⏳ 規劃中 | 每日資訊整理 | Phase 2 |
| **Email Summary** | ⏳ 規劃中 | Email 整理與回覆 | Phase 2 |
| **Document Analysis** | ⏳ 規劃中 | 文件分析 | Phase 2 |
| **Coding Assistant** | ⏳ 規劃中 | 程式開發協助 | Phase 2 |

### 1.2 Workflow 狀態說明

| 狀態 | 說明 |
|------|------|
| ✅ 已完成 | 已建立、測試、驗證 |
| 🔄 開發中 | 正在開發 |
| ⏳ 規劃中 | 已規劃，尚未開始 |
| 💡 構想中 | 僅有構想 |

---

## 2. Capability 清單

### 2.1 目前 Capability

| Capability | 狀態 | 說明 | 所屬 Workflow |
|------------|------|------|---------------|
| **Stock Data Query** | ⏳ 規劃中 | 股票資料查詢 | Stock Analysis, Portfolio Check |
| **Technical Analysis** | ⏳ 規劃中 | 技術分析 | Stock Analysis |
| **Fundamental Analysis** | ⏳ 規劃中 | 基本面分析 | Stock Analysis, Portfolio Check |
| **News Analysis** | ⏳ 規劃中 | 新聞分析 | Stock Analysis, Daily Info |
| **Risk Analysis** | ⏳ 規劃中 | 風險分析 | Stock Analysis, Portfolio Check |
| **Report Generator** | ⏳ 規劃中 | 報告生成 | 多個 Workflow 共用 |
| **Email Parser** | ⏳ 規劃中 | Email 解析 | Email Summary |
| **Email Draft** | ⏳ 規劃中 | Email 草稿生成 | Email Summary |
| **Document Parser** | ⏳ 規劃中 | 文件解析 | Document Analysis |
| **Web Search** | ⏳ 規劃中 | 網路搜尋 | Daily Info |
| **Calendar Query** | ⏳ 規劃中 | 行事曆查詢 | Daily Info |
| **Code Generator** | ⏳ 規劃中 | 程式碼生成 | Coding Assistant |
| **Code Review** | ⏳ 規劃中 | 程式碼審查 | Coding Assistant |

---

## 3. Response Template

### 3.1 通用 Template

```markdown
## Summary
（摘要：簡短說明核心結論）

## Analysis
（分析：詳細分析內容）

## Data Source
（資料來源：列出使用的資料來源）

## Confidence
（信心度：High / Medium / Low）

## Risk / Limitation
（風險與限制：說明潛在風險與限制）

## Next Step
（下一步建議：建議後續行動）
```

### 3.2 股票分析 Template

```markdown
# 股票分析報告

## 市場資訊
- 股票名稱/代號：
- 目前價格：
- 漲跌幅：
- 成交量：

## 技術分析
- 趨勢：
- 支撐/壓力：
- 技術指標：
  - MACD：
  - RSI：
  - KDJ：

## 基本面
- EPS：
- P/E：
- 營收：

## 新聞與情緒
- 重要新聞：
- 情緒分析：

## 風險評估
- 風險等級：
- 主要風險：

## AI 觀察
（AI 的綜合觀察）

## 注意事項
- 本分析僅供參考，不構成投資建議
- 資料來源：
- 最後更新：
```

### 3.3 Email 整理 Template

```markdown
# Email 整理報告

## 重要郵件
（列出重要郵件摘要）

## 待回覆郵件
（列出需要回覆的郵件）

## 待辦事項
（從郵件提取的待辦事項）

## 草稿建議
（AI 建議的回覆草稿）
```

### 3.4 每日資訊 Template

```markdown
# 每日資訊整理

## 今日行程
## 重要新聞
## 股票動態
## 待辦事項
## 建議
```

---

## 4. Workflow 詳細規格

### 4.1 Stock Analysis Workflow

#### 基本資訊

| 項目 | 內容 |
|------|------|
| **名稱** | Stock Analysis Workflow |
| **用途** | 分析單一股票或投資標的 |
| **輸入** | 股票名稱或代號 |
| **輸出** | 股票分析報告 |
| **優先順序** | Phase 2 優先 |
| **風險等級** | Medium |

#### 流程

```
Stock Analysis Workflow
    ↓
Stock Data Query（查詢股票資料）
    ↓
Technical Analysis（技術分析）
    ↓
Fundamental Analysis（基本面分析）
    ↓
News Analysis（新聞分析）
    ↓
Risk Analysis（風險分析）
    ↓
Report Generator（生成報告）
    ↓
Response Formatter（格式化回應）
```

#### 使用 Capability

| Capability | 用途 |
|------------|------|
| Stock Data Query | 查詢股票價格、成交量、歷史資料 |
| Technical Analysis | 計算技術指標（MACD、RSI、KDJ） |
| Fundamental Analysis | 分析財務數據（EPS、P/E、營收） |
| News Analysis | 分析相關新聞與市場情緒 |
| Risk Analysis | 評估投資風險 |
| Report Generator | 生成統一格式報告 |

#### 使用 Tool

| Tool | 用途 |
|------|------|
| Stock API | 股票資料查詢 |
| News API | 新聞資料查詢 |
| Database | 儲存分析結果 |
| LLM | 生成分析文字 |

#### Permission

| 項目 | 等級 |
|------|------|
| 讀取股票資料 | Level 0 |
| 讀取新聞 | Level 0 |
| 寫入資料庫 | Level 2（需確認） |
| 外送資料 | Level 2（需確認） |

#### 安全規則

- ❌ 禁止自動交易
- ❌ 禁止修改程式
- ❌ 禁止自動寄送 Email
- ✅ 分析結果僅供參考，不構成投資建議

---

### 4.2 Email Summary Workflow

#### 基本資訊

| 項目 | 內容 |
|------|------|
| **名稱** | Email Summary Workflow |
| **用途** | 整理 Email 並產生摘要與回覆草稿 |
| **輸入** | 今日 Email |
| **輸出** | Email 摘要、待辦事項、回覆草稿 |
| **優先順序** | Phase 2 |
| **風險等級** | Medium |

#### 流程

```
Email Summary Workflow
    ↓
Email Parser（解析 Email）
    ↓
分類與重要性判斷
    ↓
生成摘要
    ↓
提取待辦事項
    ↓
Email Draft（生成回覆草稿）
    ↓
人工確認（回覆草稿）
    ↓
（可選）寄送
```

#### 使用 Capability

| Capability | 用途 |
|------------|------|
| Email Parser | 解析 Email 內容與結構 |
| Email Draft | 生成回覆草稿 |

#### 安全規則

- Email 內容優先本地模型處理
- 寄送 Email 一定人工確認
- 禁止自動寄送

---

### 4.3 Daily Information Workflow

#### 基本資訊

| 項目 | 內容 |
|------|------|
| **名稱** | Daily Information Workflow |
| **用途** | 整理今日重要資訊 |
| **輸入** | 行事曆、新聞、股票、待辦 |
| **輸出** | 每日資訊摘要 |
| **優先順序** | Phase 2 |
| **風險等級** | Low |

#### 流程

```
Daily Information Workflow
    ↓
Calendar Query（查詢行程）
    ↓
Web Search（搜尋重要新聞）
    ↓
Stock Data Query（查詢持股動態）
    ↓
生成摘要
    ↓
Report Generator
```

---

### 4.4 Document Analysis Workflow

#### 基本資訊

| 項目 | 內容 |
|------|------|
| **名稱** | Document Analysis Workflow |
| **用途** | 分析文件內容 |
| **輸入** | 文件（PDF、Word、TXT） |
| **輸出** | 文件摘要、重點、分析 |
| **優先順序** | Phase 2 |
| **風險等級** | Low |

#### 流程

```
Document Analysis Workflow
    ↓
Document Parser（解析文件）
    ↓
內容分析
    ↓
生成摘要
    ↓
Report Generator
```

---

### 4.5 Coding Assistant Workflow

#### 基本資訊

| 項目 | 內容 |
|------|------|
| **名稱** | Coding Assistant Workflow |
| **用途** | 協助程式開發 |
| **輸入** | 開發需求或問題 |
| **輸出** | 程式碼、建議、Diff |
| **優先順序** | Phase 2 |
| **風險等級** | High |

#### 流程

```
Coding Assistant Workflow
    ↓
需求分析
    ↓
Code Generator（生成程式碼）
    ↓
Code Review（程式碼審查）
    ↓
顯示 Diff
    ↓
人工確認
    ↓
（可選）Sandbox 測試
    ↓
套用
```

#### 安全規則

- 修改程式需確認
- 安裝套件需確認
- 系統修改需確認
- AI 產生程式先進 Sandbox 測試

---

## 5. Capability 詳細規格

### 5.1 Stock Data Query

| 項目 | 內容 |
|------|------|
| **Purpose** | 查詢股票資料 |
| **Input** | 股票代號、日期範圍 |
| **Output** | 股票價格、成交量、歷史資料 |
| **Tool** | Stock API |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.2 Technical Analysis

| 項目 | 內容 |
|------|------|
| **Purpose** | 技術分析 |
| **Input** | 股票歷史資料 |
| **Output** | 技術指標（MACD、RSI、KDJ） |
| **Tool** | Stock API、Database |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.3 Fundamental Analysis

| 項目 | 內容 |
|------|------|
| **Purpose** | 基本面分析 |
| **Input** | 財務報表資料 |
| **Output** | EPS、P/E、營收分析 |
| **Tool** | Financial API |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.4 News Analysis

| 項目 | 內容 |
|------|------|
| **Purpose** | 分析新聞 |
| **Input** | 新聞內容 |
| **Output** | 情緒分析、關鍵字、摘要 |
| **Tool** | News API |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.5 Risk Analysis

| 項目 | 內容 |
|------|------|
| **Purpose** | 風險分析 |
| **Input** | 股票資料、新聞、市場資料 |
| **Output** | 風險等級、主要風險 |
| **Tool** | Stock API、News API、LLM |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Medium |

### 5.6 Report Generator

| 項目 | 內容 |
|------|------|
| **Purpose** | 生成報告 |
| **Input** | 分析結果 |
| **Output** | 統一格式的報告 |
| **Tool** | LLM |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.7 Document Parser

| 項目 | 內容 |
|------|------|
| **Purpose** | 解析文件 |
| **Input** | PDF、Word、TXT |
| **Output** | 結構化文字 |
| **Tool** | File System、Parser Library |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

### 5.8 Code Generator

| 項目 | 內容 |
|------|------|
| **Purpose** | 生成程式碼 |
| **Input** | 需求描述 |
| **Output** | 程式碼草稿 |
| **Tool** | LLM |
| **Model** | Local First / External（需確認） |
| **Permission** | Level 1（產生 Draft） |
| **Risk** | High |

### 5.9 Code Review

| 項目 | 內容 |
|------|------|
| **Purpose** | 審查程式碼 |
| **Input** | 程式碼 |
| **Output** | 審查意見、建議 |
| **Tool** | LLM |
| **Model** | Local First |
| **Permission** | Level 0（只讀） |
| **Risk** | Low |

---

## 6. 新增 Workflow 流程

### 6.1 新增 Workflow 步驟

```
1. 提出需求
    ↓
2. Planner 分析
    ↓
3. 產生 Workflow Proposal
    ↓
4. 人工確認（第一次：設計方向）
    ↓
5. 建立 Draft
    ↓
6. 測試
    ↓
7. 人工確認（第二次：正式加入）
    ↓
8. 註冊到 Workflow Registry
    ↓
9. 更新本文件
```

### 6.2 Workflow 註冊資訊

新增 Workflow 必須包含：
- 名稱
- 用途
- 輸入/輸出
- 流程圖
- 使用 Capability
- 使用 Tool
- Permission 需求
- 風險等級
- Response Template

---

## 7. 新增 Capability 流程

### 7.1 新增 Capability 步驟

```
1. 提出需求
    ↓
2. 分析是否需要新增（或可用現有組合）
    ↓
3. 產生 Capability Proposal
    ↓
4. 人工確認
    ↓
5. 建立 Draft
    ↓
6. 測試
    ↓
7. 人工確認
    ↓
8. 註冊到 Capability Registry
    ↓
9. 更新本文件
```

### 7.2 Capability 註冊資訊

新增 Capability 必須包含：
- 名稱
- Purpose
- Input
- Output
- Tool
- Model
- Permission
- Risk
- Capability Sandbox 設定

---

> **本文件為最常更新的文件，每次新增或修改 Workflow / Capability 都必須更新本文件。**
