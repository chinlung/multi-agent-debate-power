# 實作執行指南

本文件說明如何使用 Kiro 的 subagent 系統和 MCP 工具來實際執行多代理辯證系統。

## 🚀 並行執行流程

### 完全並行模式：所有代理同時啟動

為了最大化執行效率，我們可以讓 Orchestrator 和三個 Perspective Agents 完全並行執行：

```javascript
// 🚀 Phase 0 + 1：並行啟動所有代理
const [orchestratorResult, agentAResult, agentBResult, agentCResult] = await Promise.all([
  
  // Orchestrator：分析需求並提供角度建議
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是多代理辯證系統的 Orchestrator（協調者）。

需求描述：${用戶需求}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 進行結構化需求分析
2. 根據需求類型建議最適合的三個思考角度
3. 輸出需求分析和角度配置建議

參考角度配置：
- 架構設計：效能優先 vs 可維護性優先 vs 擴展性優先
- 功能開發：快速交付 vs 品質優先 vs 使用者體驗優先
- 效能優化：演算法優化 vs 快取策略 vs 架構重構
- 問題修復：快速修補 vs 根本解決 vs 防禦性重構
- 技術選型：主流穩定 vs 新興技術 vs 自研方案

輸出格式參考 agent-definitions.md。
    `,
    explanation: "Orchestrator 並行分析需求"
  }),

  // Agent A：預設角度 - 實用性優先
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Perspective Agent A，思考角度：實用性優先（快速可行的解決方案）

需求描述：${用戶需求}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 mcp_context7_resolve_library_id 和 mcp_context7_get_library_docs
3. 如需程式碼分析，使用 serena 相關工具
4. 從實用性角度提出解決方案（重視可行性、快速實施、風險控制）

輸出格式參考 agent-definitions.md 中的 Agent A 格式。
    `,
    explanation: "Agent A 並行提出實用性方案"
  }),

  // Agent B：預設角度 - 品質優先  
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Perspective Agent B，思考角度：品質優先（高品質長期方案）

需求描述：${用戶需求}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 mcp_context7_resolve_library_id 和 mcp_context7_get_library_docs
3. 如需程式碼分析，使用 serena 相關工具
4. 從品質角度提出解決方案（重視可維護性、擴展性、技術先進性）

輸出格式參考 agent-definitions.md 中的 Agent B 格式。
    `,
    explanation: "Agent B 並行提出品質方案"
  }),

  // Agent C：預設角度 - 平衡性優先
  invokeSubAgent({
    name: "general-task-execution", 
    prompt: `
你是 Perspective Agent C，思考角度：平衡性優先（綜合考量方案）

需求描述：${用戶需求}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 mcp_context7_resolve_library_id 和 mcp_context7_get_library_docs
3. 如需程式碼分析，使用 serena 相關工具
4. 從平衡角度提出解決方案（平衡成本、時程、品質、風險）

輸出格式參考 agent-definitions.md 中的 Agent C 格式。
    `,
    explanation: "Agent C 並行提出平衡方案"
  })
])

// 🎯 整合結果：結合 Orchestrator 的角度建議和 Agents 的方案
console.log("=== Orchestrator 角度分析 ===")
console.log(orchestratorResult)

console.log("=== Agent A 方案（實用性優先）===")
console.log(agentAResult)

console.log("=== Agent B 方案（品質優先）===") 
console.log(agentBResult)

console.log("=== Agent C 方案（平衡性優先）===")
console.log(agentCResult)
```

### 自適應角度模式：根據需求動態調整

如果您希望更精確的角度配置，可以先讓 Orchestrator 單獨執行，然後根據其建議並行啟動 Agents：

```javascript
// 🎯 Phase 0：Orchestrator 先行分析
const orchestratorResult = await invokeSubAgent({
  name: "general-task-execution",
  prompt: `
你是 Orchestrator，請分析需求並決定最適合的三個思考角度：

需求：${用戶需求}

請使用 mcp_sequential_thinking_sequentialthinking 分析並輸出：
1. 需求類型識別
2. 三個最適合的思考角度
3. 每個角度的核心關注點

輸出格式：
角度A：[角度名稱] - [關注點]
角度B：[角度名稱] - [關注點]  
角度C：[角度名稱] - [關注點]
  `,
  explanation: "Orchestrator 分析並決定思考角度"
})

// 解析 Orchestrator 的角度建議
const angles = parseAngles(orchestratorResult) // 您需要實作這個解析函數

// 🚀 Phase 1：根據 Orchestrator 建議並行啟動 Agents
const [agentAResult, agentBResult, agentCResult] = await Promise.all([
  
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent A，思考角度：${angles.A}

需求：${用戶需求}
請從此角度提出解決方案，使用所有可用的 MCP 工具。
    `,
    explanation: `Agent A 執行 ${angles.A} 角度分析`
  }),

  invokeSubAgent({
    name: "general-task-execution", 
    prompt: `
你是 Agent B，思考角度：${angles.B}

需求：${用戶需求}
請從此角度提出解決方案，使用所有可用的 MCP 工具。
    `,
    explanation: `Agent B 執行 ${angles.B} 角度分析`
  }),

  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent C，思考角度：${angles.C}

需求：${用戶需求}
請從此角度提出解決方案，使用所有可用的 MCP 工具。
    `,
    explanation: `Agent C 執行 ${angles.C} 角度分析`
  })
])
```

### Phase 2：並行批判審查

收集所有方案後，可以並行啟動 Critic 和準備下一輪的 Agent 回應：

```javascript
// 🚀 Phase 2：並行啟動 Critic 和 Agent 準備
const [criticResult, ...agentPreparations] = await Promise.all([
  
  // Critic 審查所有方案
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Critic（批判者），請審查以下方案：

Orchestrator 分析：${orchestratorResult}
Agent A 方案：${agentAResult}
Agent B 方案：${agentBResult}  
Agent C 方案：${agentCResult}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 客觀分析每個方案
2. 針對每個方案的弱點提出具體挑戰
3. 根據可行性、效益、風險控制評分（每項10分）
4. 輸出結構化審查報告

輸出格式參考 agent-definitions.md 中的 Critic 格式。
    `,
    explanation: "Critic 並行審查所有方案"
  }),

  // 同時讓各 Agent 準備回應（預先分析自己方案的潛在問題）
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent A，請預先分析你的方案可能面臨的挑戰：

你的方案：${agentAResult}
其他方案：
- Agent B：${agentBResult}
- Agent C：${agentCResult}

請使用 mcp_sequential_thinking_sequentialthinking 分析：
1. 你的方案可能的弱點
2. 其他方案的優勢
3. 準備可能的反駁論點
4. 考慮是否需要修正方案

這將幫助你更好地回應 Critic 的挑戰。
    `,
    explanation: "Agent A 預先準備回應策略"
  }),

  // Agent B 和 C 的類似準備...
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `你是 Agent B，預先分析方案並準備回應...`,
    explanation: "Agent B 預先準備回應策略"
  }),

  invokeSubAgent({
    name: "general-task-execution", 
    prompt: `你是 Agent C，預先分析方案並準備回應...`,
    explanation: "Agent C 預先準備回應策略"
  })
])
```

### Phase 3：並行反駁與修正

根據 Critic 的挑戰，讓所有 Agent 並行回應：

```javascript
// 🚀 Phase 3：並行回應 Critic 挑戰
const [agentAResponse, agentBResponse, agentCResponse] = await Promise.all([
  
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent A，請回應 Critic 的挑戰：

原方案：${agentAResult}
Critic 挑戰：${criticResult.challengesToA}
你的預先分析：${agentPreparations[0]}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 分析挑戰
2. 決定舉證反駁或承認修正
3. 如需修正，提供修正後方案
4. 評估其他 Agent 的方案
5. 表明最終立場

輸出格式參考 agent-definitions.md 反駁回應格式。
    `,
    explanation: "Agent A 並行回應挑戰"
  }),

  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent B，請回應 Critic 的挑戰：

原方案：${agentBResult}
Critic 挑戰：${criticResult.challengesToB}
你的預先分析：${agentPreparations[1]}

請執行相同的回應流程...
    `,
    explanation: "Agent B 並行回應挑戰"
  }),

  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Agent C，請回應 Critic 的挑戰：

原方案：${agentCResult}
Critic 挑戰：${criticResult.challengesToC}
你的預先分析：${agentPreparations[2]}

請執行相同的回應流程...
    `,
    explanation: "Agent C 並行回應挑戰"
  })
])
```

### Phase 4：並行共識檢查與最終評估

```javascript
// 🚀 Phase 4：並行進行共識檢查和最終評估
const [consensusCheck, finalCriticEvaluation, summaryReport] = await Promise.all([
  
  // 共識檢查
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
請分析當前的共識狀態：

Agent A 立場：${agentAResponse.stance}
Agent B 立場：${agentBResponse.stance}  
Agent C 立場：${agentCResponse.stance}

請判斷：
1. 是否有 ≥2 個 Agent 同意某個方案？
2. 評分差距是否 ≥8 分？
3. 是否達成共識？
4. 建議下一步行動

輸出：共識狀態和建議
    `,
    explanation: "並行檢查共識狀態"
  }),

  // Critic 最終評估
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
你是 Critic，請對修正後的方案進行最終評估：

Agent A 修正方案：${agentAResponse.revisedSolution}
Agent B 修正方案：${agentBResponse.revisedSolution}
Agent C 修正方案：${agentCResponse.revisedSolution}

請使用 mcp_sequential_thinking_sequentialthinking：
1. 重新評分（可行性、效益、風險控制）
2. 比較改進程度
3. 如需要，進行最終裁決
4. 推薦最佳方案

輸出最終評估報告。
    `,
    explanation: "Critic 並行進行最終評估"
  }),

  // 生成總結報告
  invokeSubAgent({
    name: "general-task-execution",
    prompt: `
請生成辯證過程的總結報告：

原始需求：${用戶需求}
Orchestrator 分析：${orchestratorResult}
三個方案：${agentAResult}, ${agentBResult}, ${agentCResult}
Critic 挑戰：${criticResult}
Agent 回應：${agentAResponse}, ${agentBResponse}, ${agentCResponse}

請整理成結構化的辯證記錄，包含：
1. 需求分析摘要
2. 方案對比表
3. 關鍵辯論點
4. 最終建議

格式化為 Markdown 報告。
    `,
    explanation: "並行生成總結報告"
  })
])
```

## 🔧 MCP 工具使用指南

### Sequential Thinking 工具

```javascript
// 用於複雜問題分析
mcp_sequential_thinking_sequentialthinking({
  thought: "分析需求的核心問題是什麼？",
  nextThoughtNeeded: true,
  thoughtNumber: 1,
  totalThoughts: 5
})
```

**使用場景**：
- Orchestrator 分析需求類型
- Perspective Agents 深度思考方案
- Critic 客觀評估方案

### Context7 工具

```javascript
// 查詢技術文檔
mcp_context7_resolve_library_id({
  libraryName: "React"
})

mcp_context7_get_library_docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "hooks",
  mode: "code"
})
```

**使用場景**：
- 技術選型時查詢最新文檔
- API 使用方式確認
- 最佳實踐參考

### Serena 工具

```javascript
// 分析現有程式碼
mcp_serena_get_symbols_overview({
  relative_path: "src/components"
})

mcp_serena_search_for_pattern({
  substring_pattern: "useEffect",
  paths_include_glob: "**/*.tsx"
})
```

**使用場景**：
- 現有系統架構分析
- 程式碼品質評估
- 重構影響範圍評估

## 📋 完整並行執行範例

### 範例：電商推薦系統設計（完全並行模式）

```javascript
// 🚀 一次性並行啟動所有代理
async function executeMultiAgentDebate(userRequirement) {
  
  console.log("🚀 啟動多代理辯證系統 - 完全並行模式")
  
  // Phase 1: 並行啟動所有代理（包含 Orchestrator）
  const [orchestrator, agentA, agentB, agentC] = await Promise.all([
    
    // Orchestrator 並行分析
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Orchestrator，請分析以下需求：

需求：${userRequirement}

請使用 mcp_sequential_thinking_sequentialthinking 分析：
1. 需求類型識別
2. 關鍵約束條件
3. 成功標準
4. 建議的三個最佳思考角度

輸出結構化的需求分析報告。
      `,
      explanation: "Orchestrator 並行分析需求"
    }),

    // Agent A: 實用性優先
    invokeSubAgent({
      name: "general-task-execution", 
      prompt: `
你是 Agent A - 實用性優先角度

需求：${userRequirement}

請使用所有可用 MCP 工具：
1. mcp_sequential_thinking_sequentialthinking 進行深度思考
2. mcp_context7_resolve_library_id 查詢相關技術
3. mcp_serena_* 工具分析現有程式碼（如適用）

從實用性角度提出方案：
- 重視快速實施
- 控制開發風險  
- 使用成熟技術
- 考慮團隊能力

輸出完整的方案文件。
      `,
      explanation: "Agent A 並行提出實用方案"
    }),

    // Agent B: 品質優先
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent B - 品質優先角度

需求：${userRequirement}

請使用所有可用 MCP 工具進行分析。

從品質角度提出方案：
- 重視長期可維護性
- 追求技術先進性
- 考慮系統擴展性
- 注重程式碼品質

輸出完整的方案文件。
      `,
      explanation: "Agent B 並行提出品質方案"
    }),

    // Agent C: 平衡性優先  
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent C - 平衡性優先角度

需求：${userRequirement}

請使用所有可用 MCP 工具進行分析。

從平衡角度提出方案：
- 平衡成本與效益
- 兼顧短期與長期
- 考慮風險與機會
- 整合多方考量

輸出完整的方案文件。
      `,
      explanation: "Agent C 並行提出平衡方案"
    })
  ])

  console.log("✅ Phase 1 完成 - 所有代理並行執行完畢")

  // Phase 2: 並行批判審查和準備回應
  const [critic, ...agentPreps] = await Promise.all([
    
    // Critic 審查
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Critic，請審查所有方案：

Orchestrator 分析：
${orchestrator}

Agent A 方案（實用性優先）：
${agentA}

Agent B 方案（品質優先）：
${agentB}

Agent C 方案（平衡性優先）：
${agentC}

請使用 mcp_sequential_thinking_sequentialthinking：
1. 客觀分析每個方案的優缺點
2. 提出具體的挑戰問題
3. 評分（可行性、效益、風險控制各10分）
4. 輸出結構化審查報告

重點關注：技術可行性、實施風險、長期效益
      `,
      explanation: "Critic 並行審查所有方案"
    }),

    // Agent A 預先準備
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent A，請預先分析並準備回應：

你的方案：${agentA}
競爭方案：Agent B: ${agentB}, Agent C: ${agentC}

使用 mcp_sequential_thinking_sequentialthinking 分析：
1. 你方案的潛在弱點
2. 競爭方案的優勢
3. 可能的反駁論點
4. 方案改進空間

準備好回應策略。
      `,
      explanation: "Agent A 預先準備回應"
    }),

    // Agent B 和 C 類似準備
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `Agent B 預先分析和準備...`,
      explanation: "Agent B 預先準備回應"
    }),

    invokeSubAgent({
      name: "general-task-execution", 
      prompt: `Agent C 預先分析和準備...`,
      explanation: "Agent C 預先準備回應"
    })
  ])

  console.log("✅ Phase 2 完成 - 批判審查和準備並行完成")

  // Phase 3: 並行回應和最終評估
  const [agentAFinal, agentBFinal, agentCFinal, finalEval] = await Promise.all([
    
    // 各 Agent 並行回應 Critic
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
Agent A 最終回應：

Critic 挑戰：${critic}
你的準備：${agentPreps[0]}

請：
1. 回應所有挑戰
2. 修正方案（如需要）
3. 評估其他方案
4. 表明最終立場
      `,
      explanation: "Agent A 最終回應"
    }),

    invokeSubAgent({
      name: "general-task-execution",
      prompt: `Agent B 最終回應...`,
      explanation: "Agent B 最終回應"
    }),

    invokeSubAgent({
      name: "general-task-execution",
      prompt: `Agent C 最終回應...`, 
      explanation: "Agent C 最終回應"
    }),

    // 同時進行最終評估
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
最終評估和報告生成：

請整合所有資訊生成最終報告：
- 需求分析摘要
- 三個方案對比
- 辯證過程記錄
- 推薦方案和理由
- 實施建議

格式化為完整的 Markdown 報告。
      `,
      explanation: "生成最終評估報告"
    })
  ])

  console.log("✅ Phase 3 完成 - 最終回應和評估並行完成")

  // 返回完整結果
  return {
    orchestrator,
    agents: { A: agentA, B: agentB, C: agentC },
    critic,
    finalResponses: { A: agentAFinal, B: agentBFinal, C: agentCFinal },
    finalEvaluation: finalEval,
    executionTime: "大幅縮短（並行執行）"
  }
}

// 🚀 使用範例
const result = await executeMultiAgentDebate(
  "為電商網站新增商品推薦功能，提升購買轉換率。現有系統：Spring Boot + MySQL + Redis，日活10萬用戶，3個月內上線。"
)

console.log("🎯 辯證結果：", result)
```

### 效能對比：序列 vs 並行

```javascript
// ❌ 序列執行（慢）- 總時間：約 4-6 分鐘
async function serialExecution() {
  const orchestrator = await invokeSubAgent({...}) // 1-1.5 分鐘
  const agentA = await invokeSubAgent({...})       // 1-1.5 分鐘  
  const agentB = await invokeSubAgent({...})       // 1-1.5 分鐘
  const agentC = await invokeSubAgent({...})       // 1-1.5 分鐘
  // 總計：4-6 分鐘
}

// ✅ 並行執行（快）- 總時間：約 1.5-2 分鐘
async function parallelExecution() {
  const [orchestrator, agentA, agentB, agentC] = await Promise.all([
    invokeSubAgent({...}), // 所有並行執行
    invokeSubAgent({...}), // 最長耗時：1-1.5 分鐘
    invokeSubAgent({...}),
    invokeSubAgent({...})
  ])
  // 總計：1.5-2 分鐘（提升 60-70% 效能）
}
```

## 🎯 最佳實踐

### 工具選擇策略

1. **Sequential Thinking**：
   - 所有代理都應該使用
   - 特別適合複雜決策分析
   - 幫助結構化思考過程

2. **Context7**：
   - 技術選型時必用
   - 需要最新 API 資訊時
   - 查詢最佳實踐時

3. **Serena**：
   - 涉及現有程式碼時
   - 需要架構分析時
   - 評估重構影響時

### 並行執行優化

```javascript
// 好的做法：並行執行獨立任務
const agents = await Promise.all([
  invokeSubAgent({...agentAConfig}),
  invokeSubAgent({...agentBConfig}), 
  invokeSubAgent({...agentCConfig})
])

// 避免：序列執行（效率低）
const agentA = await invokeSubAgent({...agentAConfig})
const agentB = await invokeSubAgent({...agentBConfig})
const agentC = await invokeSubAgent({...agentCConfig})
```

### 錯誤處理

```javascript
try {
  const result = await invokeSubAgent({
    name: "general-task-execution",
    prompt: agentPrompt,
    explanation: "執行代理任務"
  })
  
  if (!result || !result.success) {
    // 處理執行失敗
    console.log("代理執行失敗，嘗試簡化任務")
    // 重試或降級處理
  }
} catch (error) {
  console.log("系統錯誤：", error.message)
  // 錯誤恢復策略
}
```

## 🔄 迭代與優化

### 共識檢查

每輪結束後檢查：
- 是否有 ≥2 個 Agent 同意某個方案？
- 評分差距是否 ≥8 分？
- 是否達到最大輪數？

### 使用者互動

```javascript
// 每輪結束後詢問使用者
const userChoice = await askUser({
  question: "第 N 輪辯論結束，請選擇下一步",
  options: [
    { label: "繼續", value: "continue" },
    { label: "採納", value: "adopt" },
    { label: "介入", value: "intervene" },
    { label: "重設角度", value: "reset" }
  ]
})
```

### 結果輸出

```javascript
// 最終輸出結構化報告
const finalReport = {
  adoptedSolution: bestSolution,
  scores: finalScores,
  debateHistory: allRounds,
  insights: keyInsights
}

// 儲存到檔案
await writeFile("debate-result.md", formatReport(finalReport))
```

---

透過這個實作指南，您可以使用 Kiro 的 subagent 系統和 MCP 工具來實際執行多代理辯證系統，獲得高品質的決策分析結果。

## 🎯 並行執行最佳實踐

### 1. 錯誤處理和容錯機制

```javascript
// 🛡️ 強化的並行執行（含錯誤處理）
async function robustParallelExecution(userRequirement) {
  try {
    // 設定超時和重試機制
    const executeWithRetry = async (agentConfig, maxRetries = 2) => {
      for (let i = 0; i <= maxRetries; i++) {
        try {
          return await Promise.race([
            invokeSubAgent(agentConfig),
            new Promise((_, reject) => 
              setTimeout(() => reject(new Error('超時')), 120000) // 2分鐘超時
            )
          ])
        } catch (error) {
          console.log(`代理執行失敗 (嘗試 ${i + 1}/${maxRetries + 1}):`, error.message)
          if (i === maxRetries) throw error
          await new Promise(resolve => setTimeout(resolve, 1000)) // 等待1秒後重試
        }
      }
    }

    // 並行執行所有代理（含容錯）
    const results = await Promise.allSettled([
      executeWithRetry({
        name: "general-task-execution",
        prompt: `Orchestrator 分析需求：${userRequirement}`,
        explanation: "Orchestrator 分析"
      }),
      executeWithRetry({
        name: "general-task-execution", 
        prompt: `Agent A 實用性方案：${userRequirement}`,
        explanation: "Agent A 方案"
      }),
      executeWithRetry({
        name: "general-task-execution",
        prompt: `Agent B 品質方案：${userRequirement}`,
        explanation: "Agent B 方案"
      }),
      executeWithRetry({
        name: "general-task-execution",
        prompt: `Agent C 平衡方案：${userRequirement}`,
        explanation: "Agent C 方案"
      })
    ])

    // 處理結果和失敗情況
    const [orchestrator, agentA, agentB, agentC] = results.map((result, index) => {
      if (result.status === 'fulfilled') {
        return result.value
      } else {
        console.log(`代理 ${index} 執行失敗:`, result.reason.message)
        return { error: result.reason.message, fallback: true }
      }
    })

    // 檢查是否有足夠的成功結果
    const successfulAgents = results.filter(r => r.status === 'fulfilled').length
    if (successfulAgents < 2) {
      throw new Error(`執行失敗：只有 ${successfulAgents} 個代理成功執行`)
    }

    console.log(`✅ ${successfulAgents}/4 個代理成功執行`)
    return { orchestrator, agentA, agentB, agentC, successCount: successfulAgents }

  } catch (error) {
    console.error("並行執行失敗:", error.message)
    // 降級到序列執行
    console.log("🔄 降級到序列執行模式...")
    return await fallbackSerialExecution(userRequirement)
  }
}
```

### 2. 效能監控和優化

```javascript
// 📊 效能監控
async function monitoredParallelExecution(userRequirement) {
  
  const startTime = Date.now()
  const performanceMetrics = {
    phases: [],
    totalTime: 0,
    parallelEfficiency: 0
  }
  
  // Phase 1: 並行啟動所有代理
  const phase1Start = Date.now()
  const results = await Promise.all([
    timedExecution('Orchestrator', orchestratorPrompt),
    timedExecution('Agent A', agentAPrompt),
    timedExecution('Agent B', agentBPrompt), 
    timedExecution('Agent C', agentCPrompt)
  ])
  
  const phase1Time = Date.now() - phase1Start
  performanceMetrics.phases.push({
    name: 'Phase 1 - 並行方案生成',
    duration: phase1Time,
    agentsCount: 4
  })
  
  // Phase 2: 並行批判和回應
  const phase2Start = Date.now()
  const criticalResults = await Promise.all([
    timedExecution('Critic', criticPrompt),
    timedExecution('Agent A Response', responsePrompt),
    timedExecution('Agent B Response', responsePrompt),
    timedExecution('Agent C Response', responsePrompt)
  ])
  
  const phase2Time = Date.now() - phase2Start
  performanceMetrics.phases.push({
    name: 'Phase 2 - 並行批判回應',
    duration: phase2Time,
    agentsCount: 4
  })
  
  performanceMetrics.totalTime = Date.now() - startTime
  performanceMetrics.parallelEfficiency = calculateEfficiency(performanceMetrics)
  
  console.log("📊 執行效能報告:", performanceMetrics)
  
  return { results, criticalResults, performanceMetrics }
}

async function timedExecution(agentName, prompt) {
  const start = Date.now()
  try {
    const result = await invokeSubAgent({
      name: "general-task-execution",
      prompt: prompt,
      explanation: `執行 ${agentName}`
    })
    const duration = Date.now() - start
    console.log(`✅ ${agentName} 完成 (${duration}ms)`)
    return { ...result, executionTime: duration, agentName }
  } catch (error) {
    const duration = Date.now() - start
    console.log(`❌ ${agentName} 失敗 (${duration}ms):`, error.message)
    return { error: error.message, executionTime: duration, agentName }
  }
}
```

### 3. 智能負載平衡

```javascript
// ⚖️ 動態負載分配
async function smartLoadBalancing(userRequirement) {
  
  // 分析需求複雜度
  const complexity = analyzeRequirementComplexity(userRequirement)
  
  if (complexity.level === 'simple') {
    // 簡單需求：全並行快速執行
    return await fullParallelExecution(userRequirement)
    
  } else if (complexity.level === 'medium') {
    // 中等需求：分階段並行
    const phase1 = await Promise.all([
      lightweightOrchestrator(userRequirement),
      quickAnalysis(userRequirement)
    ])
    
    const phase2 = await Promise.all([
      enhancedAgent('A', userRequirement, phase1[0]),
      enhancedAgent('B', userRequirement, phase1[0]),
      enhancedAgent('C', userRequirement, phase1[0])
    ])
    
    return { phase1, phase2 }
    
  } else {
    // 複雜需求：混合策略
    
    // 先進行深度分析
    const deepAnalysis = await invokeSubAgent({
      name: "general-task-execution",
      prompt: `
深度分析複雜需求：${userRequirement}

請使用 mcp_sequential_thinking_sequentialthinking 進行多層次分析：
1. 需求分解
2. 技術挑戰識別  
3. 風險評估
4. 約束條件分析
5. 成功標準定義

輸出詳細的分析報告。
      `,
      explanation: "複雜需求深度分析"
    })
    
    // 基於深度分析結果並行執行專業化代理
    const specializedAgents = await Promise.all([
      specializedAgent('技術架構師', userRequirement, deepAnalysis),
      specializedAgent('風險評估師', userRequirement, deepAnalysis),
      specializedAgent('實施規劃師', userRequirement, deepAnalysis),
      specializedAgent('品質保證師', userRequirement, deepAnalysis)
    ])
    
    return { deepAnalysis, specializedAgents }
  }
}

function analyzeRequirementComplexity(requirement) {
  const complexityIndicators = {
    length: requirement.length,
    technicalTerms: (requirement.match(/架構|整合|遷移|效能|安全|擴展/g) || []).length,
    constraints: (requirement.match(/預算|時程|團隊|技術棧|相容性/g) || []).length,
    stakeholders: (requirement.match(/用戶|客戶|管理層|開發團隊/g) || []).length
  }
  
  const score = 
    (complexityIndicators.length > 300 ? 2 : 1) +
    complexityIndicators.technicalTerms +
    complexityIndicators.constraints +
    (complexityIndicators.stakeholders > 2 ? 2 : 1)
  
  return {
    level: score <= 4 ? 'simple' : score <= 7 ? 'medium' : 'complex',
    score,
    indicators: complexityIndicators
  }
}
```

### 4. 結果品質保證

```javascript
// 🔍 品質檢查和自動改進
async function qualityAssuredExecution(userRequirement) {
  
  // 第一輪：並行執行
  const round1Results = await robustParallelExecution(userRequirement)
  
  // 品質評估
  const qualityCheck = await Promise.all([
    
    // 多樣性檢查
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
分析方案多樣性：

Agent A: ${round1Results.agentA}
Agent B: ${round1Results.agentB}  
Agent C: ${round1Results.agentC}

請評估：
1. 方案之間的差異程度（0-10分）
2. 是否涵蓋了不同的解決思路
3. 是否存在明顯的遺漏角度
4. 建議補充的視角

輸出多樣性評估報告。
      `,
      explanation: "方案多樣性檢查"
    }),
    
    // 完整性檢查
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
檢查方案完整性：

需求：${userRequirement}
方案：${JSON.stringify(round1Results)}

請檢查每個方案是否包含：
1. 技術實施細節
2. 風險評估和緩解
3. 成本和時程估算
4. 實施步驟
5. 成功標準

輸出完整性評估報告。
      `,
      explanation: "方案完整性檢查"
    }),
    
    // 可行性檢查
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
評估方案可行性：

使用 mcp_context7_* 工具查詢相關技術資料
使用 mcp_serena_* 工具分析技術可行性（如適用）

對每個方案評估：
1. 技術可行性
2. 資源需求合理性
3. 時程安排現實性
4. 風險可控性

輸出可行性評估報告。
      `,
      explanation: "方案可行性檢查"
    })
  ])
  
  // 根據品質檢查結果決定是否需要補強
  const [diversityReport, completenessReport, feasibilityReport] = qualityCheck
  
  const needsImprovement = 
    diversityReport.score < 7 || 
    completenessReport.score < 8 || 
    feasibilityReport.averageScore < 7
  
  if (needsImprovement) {
    console.log("🔄 品質不足，啟動改進機制...")
    
    // 並行執行改進措施
    const improvements = await Promise.all([
      
      // 補充遺漏角度
      diversityReport.score < 7 ? invokeSubAgent({
        name: "general-task-execution",
        prompt: `
基於多樣性分析，補充遺漏的解決角度：

現有方案摘要：${round1Results}
遺漏角度：${diversityReport.missingPerspectives}

請提供創新的解決方案。
        `,
        explanation: "補充創新角度"
      }) : null,
      
      // 完善方案細節
      completenessReport.score < 8 ? invokeSubAgent({
        name: "general-task-execution", 
        prompt: `
完善方案實施細節：

不完整的方案：${completenessReport.incompleteAspects}

請補充詳細的實施計劃。
        `,
        explanation: "完善實施細節"
      }) : null,
      
      // 可行性優化
      feasibilityReport.averageScore < 7 ? invokeSubAgent({
        name: "general-task-execution",
        prompt: `
優化方案可行性：

可行性問題：${feasibilityReport.issues}

請提供更可行的替代方案。
        `,
        explanation: "優化可行性"
      }) : null
      
    ].filter(Boolean)) // 過濾掉 null 值
    
    return {
      originalResults: round1Results,
      qualityReports: qualityCheck,
      improvements,
      finalQuality: 'improved'
    }
  }
  
  return {
    results: round1Results,
    qualityReports: qualityCheck,
    finalQuality: 'satisfactory'
  }
}
```

## 🚀 快速啟動模板

### 基本並行執行模板

```javascript
// 🎯 複製即用的並行執行模板
async function executeMultiAgentDebate(userRequirement) {
  
  console.log("🚀 啟動多代理辯證系統...")
  
  try {
    // 🔥 並行執行所有代理
    const [orchestrator, agentA, agentB, agentC] = await Promise.all([
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Orchestrator，請分析需求：${userRequirement}
使用 mcp_sequential_thinking_sequentialthinking 進行結構化分析。
        `,
        explanation: "Orchestrator 分析"
      }),
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Agent A（實用性優先），針對需求：${userRequirement}
請提出快速可行的解決方案。
        `,
        explanation: "Agent A 實用方案"
      }),
      
      invokeSubAgent({
        name: "general-task-execution", 
        prompt: `
你是 Agent B（品質優先），針對需求：${userRequirement}
請提出高品質的長期方案。
        `,
        explanation: "Agent B 品質方案"
      }),
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Agent C（平衡性優先），針對需求：${userRequirement}
請提出平衡各方考量的方案。
        `,
        explanation: "Agent C 平衡方案"
      })
    ])
    
    console.log("✅ 第一輪並行執行完成")
    
    // 🔥 並行批判和回應
    const [critic, ...responses] = await Promise.all([
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Critic，請審查方案：
Orchestrator: ${orchestrator}
Agent A: ${agentA}
Agent B: ${agentB}
Agent C: ${agentC}

使用 mcp_sequential_thinking_sequentialthinking 進行客觀評估。
        `,
        explanation: "Critic 審查"
      }),
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `Agent A 準備回應策略...`,
        explanation: "Agent A 準備"
      }),
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `Agent B 準備回應策略...`,
        explanation: "Agent B 準備"
      }),
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `Agent C 準備回應策略...`,
        explanation: "Agent C 準備"
      })
    ])
    
    console.log("✅ 批判審查完成")
    
    // 🎯 生成最終報告
    const finalReport = await invokeSubAgent({
      name: "general-task-execution",
      prompt: `
整合所有結果生成最終辯證報告：

需求：${userRequirement}
Orchestrator：${orchestrator}
方案 A：${agentA}
方案 B：${agentB}  
方案 C：${agentC}
Critic 評估：${critic}

請生成結構化的決策分析報告。
      `,
      explanation: "生成最終報告"
    })
    
    return {
      requirement: userRequirement,
      orchestrator,
      agents: { A: agentA, B: agentB, C: agentC },
      critic,
      finalReport,
      executionMode: "並行執行",
      timestamp: new Date().toISOString()
    }
    
  } catch (error) {
    console.error("執行失敗:", error.message)
    throw error
  }
}

// 🚀 使用範例
const result = await executeMultiAgentDebate(
  "設計一個支援百萬用戶的即時聊天系統，需要考慮效能、擴展性和成本控制。"
)

console.log("🎯 辯證結果:", result)
```

---

透過這些並行執行策略和最佳實踐，您的多代理辯證系統將能夠：

- **大幅提升執行效率**（60-70% 時間節省）
- **增強系統穩定性**（容錯和重試機制）
- **保證結果品質**（自動品質檢查和改進）
- **提供更好的使用者體驗**（即時回饋和互動）

現在您可以享受真正高效的並行多代理辯證體驗！🚀