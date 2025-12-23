# 多代理辯證系統 Kiro Power

[![Kiro Power](https://img.shields.io/badge/Kiro-Power-blue)](https://kiro.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/Version-1.0.0-green.svg)](https://github.com/chinlung/multi-agent-debate-power)

透過協調者、三個不同角度的代理和批判者進行多輪辯證，對複雜需求進行深度分析並產出最優實踐方案的創新決策分析工具。整合 sequential-thinking、context7 和 serena MCP 工具，支援並行代理執行和智能共識檢查。

## 🚀 功能特色

- **五個專業角色**：Orchestrator（協調者）、三個 Perspective Agents、Critic（批判者）
- **並行代理執行**：Phase 1 和 Phase 3 支援三個代理並行處理，大幅提升效率
- **智能角度配置**：根據需求類型自動選擇最適合的思考角度
- **多輪辯證流程**：完整的 6 個階段實作，包含共識檢查和使用者互動
- **量化評估系統**：可行性、效益、風險控制三維度評分（總分 30 分）
- **結構化辯證**：透過多輪批判和改進達成共識或 Critic 最終裁決
- **完整記錄追蹤**：保存完整的辯證過程和決策依據
- **MCP 工具整合**：整合 sequential-thinking、context7、serena 工具
- **Kiro Subagent 支援**：使用 Kiro 原生的 subagent 系統執行
- **關鍵字自動觸發**：智能檢測需求並自動啟動辯證分析
- **使用者互動機制**：支援介入調整、重設角度、階段性決策

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
Phase 1: Perspective A/B/C → ⚡ 並行生成方案
    ↓
Phase 2: Critic → 審查挑戰 → 量化評分
    ↓
Phase 3: 代理回應修正 → ⚡ 並行處理
    ↓
Phase 4: 共識檢查 → 使用者互動 → 決策選擇
    ↓
Phase 5: 最終方案 → 結構化報告
```

### 完整辯證流程（6 個階段）

- **Phase 0**：需求分析與角度配置
- **Phase 1**：初始方案生成（⚡ 並行執行）
- **Phase 2**：批判審查與量化評分
- **Phase 3**：反駁與修正（⚡ 並行執行）
- **Phase 4**：共識檢查與分數分析
- **Phase 5**：使用者互動與選擇
- **Phase 6**：最終輸出與報告生成

### 核心角色

- **🎯 Orchestrator（協調者）**：分析需求並決定三個代理的思考角度
- **🅰️ Perspective Agent A**：從指定角度提出解決方案
- **🅱️ Perspective Agent B**：從不同角度提出替代方案  
- **🅲 Perspective Agent C**：提供第三種視角的解決方案
- **🔍 Critic（批判者）**：客觀審查所有方案並提出挑戰，進行量化評分

### 並行執行機制

系統在關鍵階段支援並行處理：
- **Phase 1**：三個 Perspective Agents 同時生成初始方案
- **Phase 3**：三個 Agents 同時回應 Critic 的挑戰
- 透過 Kiro 的 `invokeSubAgent` 在同一個 function_calls block 中實現真正的並行執行

## 📊 評分標準

系統使用三個維度對方案進行量化評估（每項 10 分，總分 30 分）：

| 維度 | 評分標準 | 權重 |
|------|----------|------|
| **可行性 (Feasibility)** | 技術可實現性、資源可得性、時程合理性 | 33.3% |
| **效益 (Benefit)** | 解決問題的程度、帶來的正面價值、ROI | 33.3% |
| **風險控制 (Risk Control)** | 風險識別完整度、緩解措施可靠性、失敗影響範圍 | 33.3% |

### 共識檢查機制

- **共識達成**：≥2 個 Agent 同意某方案
- **明顯領先**：最高分與次高分差距 ≥8 分
- **最終裁決**：達到最大輪數時由 Critic 進行最終判決

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

## � 辯關鍵洞察
[辯證過程中的主要發現和學習點]

## 📋 實施建議
[具體的實施步驟和注意事項]

## 📜 辯論過程摘要
**總輪數**: X 輪
**共識達成**: [是/否]
**執行模式**: 並行多代理辯證系統

<details>
<summary>📜 完整辯論記錄（點擊展開）</summary>
[完整的辯論記錄，包含各輪的方案、挑戰和回應]
</details>
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

技術約束：
- 不能影響現有功能
- 資料庫遷移時間 <4 小時
- 效能不能下降超過 10%

業務目標：
- 支援 1000+ 企業客戶
- 降低部署和維護成本
- 提升數據安全性

成功標準：
- 數據完全隔離
- 查詢效能保持
- 部署複雜度降低
```

❌ **避免的描述**：
```
需求：系統太慢了，需要優化
```

### 角度選擇策略

**自訂角度的時機**：
```bash
# 有特定的業務約束
/debate 設計支付系統 --perspectives "安全優先,合規優先,用戶體驗優先"

# 有明確的技術偏好
/debate 選擇前端框架 --perspectives "React生態,Vue生態,原生JavaScript"
```

**讓系統自動選擇的情況**：
```bash
# 通用的技術問題
/debate 優化資料庫查詢效能

# 探索性的需求
/debate 提升用戶留存率
```

### 互動參與策略

- **適時介入**：當辯論偏離方向時，可以調整或追加條件
- **保持開放**：相信辯證過程，不要過早下結論
- **提供回饋**：在每輪結束時提供您的觀點和偏好
- **記錄洞察**：注意辯論過程中產生的新想法和洞察

### 使用模式

#### 模式 1：快速決策
```bash
/debate 修復登入問題 --max-rounds 3
```
適用於緊急問題、簡單需求

#### 模式 2：深度分析
```bash
/debate 設計微服務架構 --max-rounds 8
```
適用於架構決策、長期規劃

#### 模式 3：探索性研究
```bash
/debate 引入 AI 功能提升產品競爭力 --perspectives "技術可行性,商業價值,用戶接受度"
```
適用於新技術評估、創新方案

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

**問題**：並行執行失效，代理順序執行
**解決方案**：
- 確保在同一個 function_calls block 中調用多個 invokeSubAgent
- 檢查 Kiro subagent 系統是否正常運作
- 查看執行日誌確認並行狀態

**問題**：評分結果不合理
**解決方案**：
- 檢查 Critic 的評分邏輯和標準
- 提供更明確的評估維度說明
- 要求重新評分並說明理由

詳細的疑難排解指南請在安裝 Power 後查看 `troubleshooting.md`。

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

### 混合方案策略

當各方案都有優點時，可以要求代理提出混合方案：
```
介入指示：請結合 Agent A 的快取策略和 Agent B 的一致性保證
```

### 效果評估指標

**辯證品質指標**：
- 方案多樣性：三個方案的差異程度
- 挑戰深度：Critic 提出的問題品質
- 回應品質：Agent 對挑戰的回應完整性
- 共識形成：達成共識的輪數和過程

**決策品質指標**：
- 可行性驗證：最終方案的實際可執行性
- 風險識別：是否充分識別和緩解風險
- 效益實現：實際效果是否符合預期
- 學習價值：過程中產生的洞察和知識

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
- 📖 [更新日誌](CHANGELOG.md) | [English Changelog](CHANGELOG_EN.md)

## 📚 相關文件

- [English README](README_EN.md) - 英文版說明文件
- [更新日誌](CHANGELOG.md) - 版本更新記錄
- [授權條款](LICENSE) - MIT 授權說明

---

**讓多代理辯證系統幫助您做出更好的技術決策！** 🚀

**版本**: 1.0.0 | **最後更新**: 2025-12-23 | **作者**: SCL