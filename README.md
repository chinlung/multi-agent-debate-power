# 多代理辯證系統 Kiro Power

[![Kiro Power](https://img.shields.io/badge/Kiro-Power-blue)](https://kiro.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

透過協調者、三個不同角度的代理和批判者進行多輪辯證，對複雜需求進行深度分析並產出最優實踐方案的創新決策分析工具。

## 🚀 功能特色

- **五個專業角色**：Orchestrator、三個 Perspective Agents、Critic
- **多角度分析**：從不同視角全面評估問題
- **結構化辯證**：透過多輪批判和改進達成共識
- **量化評估**：可行性、效益、風險控制三維度評分
- **智能角度配置**：根據需求類型自動選擇最適合的思考角度
- **完整記錄**：保存完整的辯證過程和決策依據
- **MCP 工具整合**：整合 sequential-thinking、context7、serena 工具
- **Kiro Subagent 支援**：使用 Kiro 原生的 subagent 系統執行
- **關鍵字自動觸發**：智能檢測需求並自動啟動辯證分析

## 🎯 適用場景

| 場景類型 | 典型應用 | 預期效果 |
|----------|----------|----------|
| **架構設計** | 系統架構選型、技術棧決策 | 平衡效能、可維護性、擴展性 |
| **功能開發** | 產品功能規劃、開發策略 | 兼顧交付速度、品質、用戶體驗 |
| **效能優化** | 系統瓶頸解決、效能提升 | 多角度優化策略比較 |
| **問題修復** | 線上問題處理、技術債務 | 權衡修復速度與根本解決 |
| **技術選型** | 框架選擇、工具評估 | 評估穩定性、創新性、適用性 |

## 🛠️ 安裝方式

### 透過 Kiro Powers UI（推薦）

1. 在 Kiro IDE 中開啟 Powers 面板
2. 點擊 "Available Powers" → "Manage Repos" → "Add Repository"
3. 選擇 "Import power from GitHub" 並輸入：
   ```
   https://github.com/chinlung/multi-agent-debate-power
   ```
4. 在可用 Powers 列表中找到「多代理辯證系統」並安裝

### 本地安裝

1. Clone 此 repository：
   ```bash
   git clone https://github.com/chinlung/multi-agent-debate-power.git
   ```

2. 在 Kiro Powers UI 中新增本地目錄：
   - 在 Kiro IDE 中開啟 Powers 面板
   - 點擊 "Available Powers" → "Manage Repos" → "Add Repository"
   - 選擇 "Local Directory"
   - 選擇路徑：`/path/to/multi-agent-debate-power`

## 📖 使用方式

### 快速開始

1. **安裝 Power** 後，在 Kiro 中激活：
   ```
   Call action "activate" with powerName="multi-agent-debate"
   ```

2. **基本使用範例**：
   ```bash
   # 自動觸發（推薦）
   "請對電商推薦系統進行多角度辯證分析"
   
   # 參數化使用
   "辯證分析微服務架構，最多 5 輪，從效能、維護性、成本角度"
   
   # 手動觸發
   /debate 我想為電商網站新增商品推薦功能
   ```

3. **進階使用範例**：
   ```bash
   # 指定最大輪數
   /debate 重構使用者認證系統 --max-rounds 5
   
   # 自訂思考角度
   /debate 設計快取策略 --perspectives "效能優先,成本優先,簡單優先"
   ```

### 詳細文檔

Power 安裝後，您可以透過以下方式存取完整文檔：

- **主要文檔**：`Call action "activate" with powerName="multi-agent-debate"`
- **代理定義**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="agent-definitions.md"`
- **使用範例**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="usage-examples.md"`
- **實作指南**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="implementation-guide.md"`
- **完整工作流程**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="complete-workflow.md"`
- **關鍵字觸發**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="prompt-interface.md"`
- **疑難排解**：`Call action "readSteering" with powerName="multi-agent-debate", steeringFile="troubleshooting.md"`

## 🏗️ 系統架構

```
需求輸入 → Orchestrator → 角度配置
    ↓
Perspective A/B/C → 並行生成方案
    ↓
Critic → 審查挑戰 → 代理回應修正
    ↓
共識檢查 → 使用者互動 → 最終方案
```

### 核心角色

- **🎯 Orchestrator（協調者）**：分析需求並決定三個代理的思考角度
- **🅰️ Perspective Agent A**：從指定角度提出解決方案
- **🅱️ Perspective Agent B**：從不同角度提出替代方案  
- **🅲 Perspective Agent C**：提供第三種視角的解決方案
- **🔍 Critic（批判者）**：客觀審查所有方案並提出挑戰

## 📊 評分標準

系統使用三個維度對方案進行量化評估（每項 10 分，總分 30 分）：

| 維度 | 評分標準 |
|------|----------|
| **可行性 (Feasibility)** | 技術可實現性、資源可得性、時程合理性 |
| **效益 (Benefit)** | 解決問題的程度、帶來的正面價值、ROI |
| **風險控制 (Risk Control)** | 風險識別完整度、緩解措施可靠性、失敗影響範圍 |

## 🎯 使用範例

### 範例 1：架構設計決策

```bash
/debate 設計支援百萬用戶的電商平台後端架構
```

**預期角度配置**：
- Agent A（效能優先）：微服務 + Redis 集群 + CDN
- Agent B（可維護性優先）：模組化單體 + 清晰分層
- Agent C（擴展性優先）：事件驅動架構 + 水平擴展

### 範例 2：技術選型

```bash
/debate 為新專案選擇前端框架 --perspectives "React生態,Vue生態,原生方案"
```

### 範例 3：效能優化

```bash
/debate 解決資料庫查詢效能問題 --max-rounds 6
```

## 📋 輸出格式

系統會產生結構化的分析報告：

```markdown
# 🎯 最終方案

## 採納方案: [方案名稱]
**來源**: Agent [X]（[共識狀態]）
**總分**: X/30

[完整方案內容]

## 📊 評分明細

| Agent | 可行性 | 效益 | 風險控制 | 總分 |
|-------|--------|------|----------|------|
| A     | X/10   | X/10 | X/10     | X/30 |
| B     | X/10   | X/10 | X/10     | X/30 |
| C     | X/10   | X/10 | X/10     | X/30 |

## 📜 辯論過程
[完整的辯論記錄，包含各輪的方案、挑戰和回應]
```

## 💡 最佳實踐

### 需求描述技巧

✅ **好的需求描述**：
```
需求：為 SaaS 平台新增多租戶支援

背景：
- 現有系統：Django + PostgreSQL
- 用戶規模：100 個企業客戶，每個 50-200 用戶
- 數據隔離要求：嚴格隔離，符合 GDPR

約束：
- 不能影響現有功能
- 資料庫遷移時間 <4 小時
- 效能不能下降超過 10%

目標：
- 支援 1000+ 企業客戶
- 降低部署和維護成本
- 提升數據安全性
```

❌ **避免的描述**：
```
需求：系統太慢了，需要優化
```

### 互動參與策略

- **適時介入**：當辯論偏離方向時，可以調整或追加條件
- **保持開放**：相信辯證過程，不要過早下結論
- **提供回饋**：在每輪結束時提供您的觀點和偏好
- **記錄洞察**：注意辯論過程中產生的新想法和洞察

## 🐛 疑難排解

### 常見問題

**問題**：代理提出的方案過於相似
**解決方案**：
- 重新描述需求，增加更多背景和約束
- 使用 `--perspectives` 參數指定更明確的角度
- 選擇「重設角度」選項重新配置

**問題**：辯論陷入僵局，無法達成共識
**解決方案**：
- 介入辯論，提供額外的評估標準或約束
- 讓 Critic 進行最終裁決
- 考慮混合方案，結合各方案的優點

詳細的疑難排解指南請在安裝 Power 後查看。

## 🔧 自訂設定

### 調整評估標準

您可以在需求描述中指定特殊的評估標準：
```
需求：設計快取策略
特殊要求：特別重視成本控制，效能可以適度犧牲
```

### 階段性決策

對於複雜問題，可以分階段進行辯證：
```
第一階段：確定整體架構方向
第二階段：細化具體實施方案
```

## 🤝 貢獻

歡迎提交 Issues 和 Pull Requests！

1. Fork 此 repository
2. 建立您的功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交您的變更 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 開啟 Pull Request

## 📄 授權

此專案採用 MIT 授權 - 詳見 [LICENSE](LICENSE) 檔案。

## 🙏 致謝

- [Kiro IDE](https://kiro.dev) - 提供強大的 Power 系統
- 靈感來自多代理系統和辯證思維的研究
- 所有貢獻者和使用者的回饋

## 📞 支援

- 🐛 [回報問題](https://github.com/chinlung/multi-agent-debate-power/issues)
- 💬 [討論區](https://github.com/chinlung/multi-agent-debate-power/discussions)
- 📧 聯絡作者：scl@hanchih.com

---

**讓多代理辯證系統幫助您做出更好的技術決策！** 🚀