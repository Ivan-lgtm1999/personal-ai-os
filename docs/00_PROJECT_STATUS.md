# 00_PROJECT_STATUS.md

# Personal AI OS — 專案狀態追蹤

> **版本**：v2.0-draft  
> **最後更新**：2026-07-11  
> **狀態**：規格制定階段（尚未開始實作）

---

## 1. 專案概述

**Personal AI OS** 是一套個人化 AI 作業系統，目標為建立不依賴單一 AI 公司的可攜式 AI 平台。

- **專案名稱**：Personal AI OS
- **Repository**：`personal-ai-os`
- **核心原則**：Safety First → Security First → Privacy First → Human Approval → Least Privilege → Local First → Everything Else
- **AI 定位**：Decision Support System（決策支援系統），非自主 Agent

---

## 2. 目前完成項目

| 項目 | 狀態 | 說明 |
|------|------|------|
| 專案願景與原則 | ✅ 完成 | 已定義核心原則與開發哲學 |
| 系統架構規格 | ✅ 完成 | 整體架構、各層定義、資料流 |
| 開發規範 | ✅ 完成 | Coding Rule、Security、Docker、Git 規範 |
| Workflow & Capability 規格 | ✅ 完成 | 現有 Workflow 與 Capability 定義 |
| Docker Compose 實作 | ⏳ 待開始 | 基礎環境建置 |
| Open WebUI + n8n 串接 | ⏳ 待開始 | Phase 1 目標 |
| 第一個 Workflow（股票分析） | ⏳ 待開始 | Phase 2 目標 |
| Planner 導入 | ⏳ 待開始 | Phase 3 目標（Workflow 數量足夠後） |
| Memory / RAG | ⏳ 待開始 | Phase 4 目標 |
| MCP 整合 | ⏳ 待開始 | 未來擴充 |

---

## 3. 建置階段規劃

### Phase 1：基礎環境（目標：建立可用 AI Chat 環境）
- [ ] 安裝 Docker、Docker Compose
- [ ] 安裝 Ollama（本地模型）
- [ ] 安裝 Open WebUI（Agent A：Assistant Agent）
- [ ] 安裝 n8n（Workflow Engine）
- [ ] 建立 Git Repository 結構
- [ ] 建立 `compose.yaml`
- [ ] 設定 Secrets 管理（`.env` 隔離）
- [ ] 測試本地模型（Qwen / DeepSeek Coder）
- [ ] 測試 FreeLLMAPI 接入

### Phase 2：開發環境與第一批 Workflow（目標：可執行固定 Workflow）
- [ ] 安裝 Codex / Claude Code（Agent B：Development Agent）
- [ ] 建立第一個 Workflow：**股票分析**
- [ ] 建立相關 Capability（Stock Data Query、Technical Analysis、News Analysis、Risk Analysis、Report Generator）
- [ ] 建立 Email Summary Workflow
- [ ] 建立 Daily Information Workflow
- [ ] 建立 Document Analysis Workflow
- [ ] 建立 Response Template
- [ ] 建立 Audit Log 機制
- [ ] 建立 Permission Check 基礎版

### Phase 3：智慧化（目標：AI 自動判斷 Workflow）
- [ ] 導入 Intent Recognition
- [ ] 導入 Planner（Workflow 數量 > 5 後開始）
- [ ] 建立 Workflow Proposal 機制
- [ ] 建立 Dynamic Workflow 組合能力
- [ ] 建立 Model Router
- [ ] 建立 LiteLLM 整合

### Phase 4：成熟 AI OS（目標：完整個人 AI 平台）
- [ ] 導入 Memory 系統
- [ ] 導入 Vector Database（RAG）
- [ ] 導入 MCP（Model Context Protocol）
- [ ] 建立 Sandbox 機制（AI 產生程式自動測試）
- [ ] 建立完整 Capability Sandbox
- [ ] 建立 Plugin 安全管理
- [ ] 建立完整 Audit Log 查詢介面

---

## 4. 技術選型

| 元件 | 選擇 | 狀態 | 備註 |
|------|------|------|------|
| 容器化 | Docker + Docker Compose | ✅ 確定 | 所有服務容器化 |
| 本地模型 | Ollama | ✅ 確定 | Qwen、DeepSeek Coder、Llama |
| 聊天介面 | Open WebUI | ✅ 確定 | Agent A（Assistant Agent） |
| Workflow Engine | n8n | ✅ 確定 | 排程、自動化、API 串接 |
| 開發 Agent | Codex / Claude Code | ✅ 確定 | Agent B（Development Agent） |
| 模型路由 | LiteLLM | ⏳ 待定 | Phase 3 評估是否必要 |
| 記憶資料庫 | 待選 | ⏳ 待定 | Phase 4 評估（Chroma / Milvus / PostgreSQL + pgvector） |
| Secrets 管理 | `.env` + Docker Secrets | ✅ 確定 | 禁止進 Prompt / Memory / Git / Log |
| 版本控制 | Git | ✅ 確定 | Workflow、Capability、Prompt、Docker 設定 |

---

## 5. 已知問題與風險

| 問題 | 嚴重度 | 狀態 | 說明 |
|------|--------|------|------|
| 本地模型效能不足 | 中 | 追蹤中 | 複雜分析可能需要外部模型，需設計降級策略 |
| Planner 過度權限風險 | 高 | 已規劃 | 已設計 Human Approval 機制，禁止 Planner 自動修改系統 |
| Prompt Injection | 高 | 已規劃 | 已設計 Data Classification 與 Security Scan |
| Secrets 洩漏 | 高 | 已規劃 | 已設計 Secrets Vault 隔離機制 |
| Docker 安全性 | 中 | 已規劃 | 已設計 read_only、non-root、network isolate |
| 模型供應商依賴 | 低 | 追蹤中 | 透過 Model Adapter 設計確保可替換性 |

---

## 6. 決策紀錄（Architecture Decision Record）

| 日期 | 決策 | 原因 | 狀態 |
|------|------|------|------|
| 2026-07-11 | 專案名稱定為 Personal AI OS | 簡潔、明確、可擴充 | ✅ 確定 |
| 2026-07-11 | 文件語言：中文為主，技術名詞保留英文 | 方便閱讀，同時保持技術精確性 | ✅ 確定 |
| 2026-07-11 | 全部使用 Markdown 格式 | GitHub、Claude Code、Codex、ChatGPT、VS Code 皆支援 | ✅ 確定 |
| 2026-07-11 | AI 定位為 Decision Support System | 避免 AI 過度自主，確保 Human Approval | ✅ 確定 |
| 2026-07-11 | 核心原則順序：Safety → Security → Privacy → Human Approval → Least Privilege → Local First | 功能之前先確保安全 | ✅ 確定 |
| 2026-07-11 | 禁止 Planner 擁有最終決策權 | Planner 只能規劃與建議，不能自主執行 | ✅ 確定 |
| 2026-07-11 | 任何架構/安全/Flow 變更需人工核准 | Architecture Change Control | ✅ 確定 |
| 2026-07-11 | 採用 Fail Closed（預設拒絕） | 若 AI 不確定是否允許，預設拒絕並詢問 | ✅ 確定 |
| 2026-07-11 | 先建立五份核心文件，後續按需擴充 | 避免初期過度複雜 | ✅ 確定 |
| 2026-07-11 | Planner 延後導入（Workflow 數量足夠後） | 初期先建立固定 Workflow，成熟後再導入 Planner | ✅ 確定 |

---

## 7. 下一步行動（TODO）

1. **立即行動**：
   - [ ] 建立 Git Repository `personal-ai-os`
   - [ ] 建立目錄結構
   - [ ] 將本規格文件放入 `docs/` 目錄
   - [ ] 建立 `compose.yaml`（Phase 1 基礎服務）
   - [ ] 建立 `.env.example`（不含真實 Secrets）

2. **Phase 1 目標**：
   - [ ] Docker 化部署 Ollama + Open WebUI + n8n
   - [ ] 測試本地模型基本功能
   - [ ] 建立第一個測試 Workflow（例如：簡單的問候 Workflow）

3. **Phase 2 目標**：
   - [ ] 設計股票分析 Workflow 完整流程
   - [ ] 建立股票相關 Capability
   - [ ] 建立 Response Template（股票格式）
   - [ ] 建立 Audit Log 實作

4. **長期追蹤**：
   - [ ] 評估 LiteLLM 是否必要
   - [ ] 評估 Vector DB 選型
   - [ ] 評估 MCP 整合方式

---

## 8. 版本變更摘要（Changelog）

### v2.0-draft（2026-07-11）
- 建立完整規格文件（5 份核心文件）
- 導入 Safety First → Security First → Privacy First 原則
- 定義 Planner 權限限制（不可自主執行）
- 定義 Architecture Change Control
- 定義 Capability Sandbox
- 定義 Zero Trust 安全架構
- 定義 Permission Level（0-4）
- 定義 Fail Closed 原則
- 定義 Diff First Policy

### v1.0（先前版本）
- 原始概念與基礎架構
- Agent A / Agent B 分工
- Workflow / Capability / Tool 分層
- Local First 模型策略
- Phase 1~4 建置規劃

---

> **注意**：本文件經常變動，每次新對話開始前請先閱讀本文件以掌握最新狀態。
