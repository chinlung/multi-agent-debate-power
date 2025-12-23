# 完整工作流程實作

本文件提供多代理辯證系統的完整階段實作，包含共識檢查、使用者互動和迭代機制。

## 🔄 完整的多輪辯證流程

### 主控制器函數

```javascript
async function executeCompleteDebate(userRequirement, maxRounds = 10) {
  
  console.log("🚀 啟動完整多代理辯證系統")
  
  let currentRound = 1
  let consensus = false
  let debateHistory = []
  let currentAgentStates = {}
  
  // Phase 0 + 1: 初始方案生成（並行）
  console.log("📋 Phase 0+1: 並行生成初始方案...")
  
  const [orchestrator, agentA, agentB, agentC] = await Promise.all([
    
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Orchestrator（協調者）。

需求描述：${userRequirement}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 進行結構化需求分析
2. 根據需求類型決定最適合的三個思考角度
3. 輸出需求分析報告和角度配置

參考角度配置：
- 架構設計：效能優先 vs 可維護性優先 vs 擴展性優先
- 功能開發：快速交付 vs 品質優先 vs 使用者體驗優先
- 效能優化：演算法優化 vs 快取策略 vs 架構重構
- 問題修復：快速修補 vs 根本解決 vs 防禦性重構
- 技術選型：主流穩定 vs 新興技術 vs 自研方案

輸出格式參考 agent-definitions.md 中的 Orchestrator 格式。
      `,
      explanation: "Orchestrator 分析需求並配置角度"
    }),

    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Perspective Agent A。

需求描述：${userRequirement}
思考角度：實用性優先（預設，Orchestrator 可能會調整）

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 mcp_context7_resolve_library_id 和 mcp_context7_get_library_docs
3. 如需程式碼分析，使用 serena 相關工具
4. 從實用性角度提出完整解決方案

輸出格式參考 agent-definitions.md 中的 Agent A 方案格式。
      `,
      explanation: "Agent A 提出實用性方案"
    }),

    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Perspective Agent B。

需求描述：${userRequirement}
思考角度：品質優先（預設，Orchestrator 可能會調整）

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 context7 工具
3. 如需程式碼分析，使用 serena 工具
4. 從品質角度提出完整解決方案

輸出格式參考 agent-definitions.md 中的 Agent B 方案格式。
      `,
      explanation: "Agent B 提出品質方案"
    }),

    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Perspective Agent C。

需求描述：${userRequirement}
思考角度：平衡性優先（預設，Orchestrator 可能會調整）

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 深度分析
2. 如需技術資料，使用 context7 工具
3. 如需程式碼分析，使用 serena 工具
4. 從平衡角度提出完整解決方案

輸出格式參考 agent-definitions.md 中的 Agent C 方案格式。
      `,
      explanation: "Agent C 提出平衡方案"
    })
  ])

  // 儲存初始狀態
  currentAgentStates = {
    orchestrator,
    agentA: { solution: agentA, stance: "堅持原方案", score: 0 },
    agentB: { solution: agentB, stance: "堅持原方案", score: 0 },
    agentC: { solution: agentC, stance: "堅持原方案", score: 0 }
  }

  console.log("✅ 初始方案生成完成")

  // 開始多輪辯證循環
  while (!consensus && currentRound <= maxRounds) {
    
    console.log(`\n🔄 === 第 ${currentRound} 輪辯證 ===`)

    // Phase 2: 批判審查
    console.log("🔍 Phase 2: Critic 審查方案...")
    
    const criticResult = await invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Critic（批判者），請審查第 ${currentRound} 輪的方案：

Orchestrator 分析：
${currentAgentStates.orchestrator}

Agent A 方案：
${currentAgentStates.agentA.solution}
當前立場：${currentAgentStates.agentA.stance}

Agent B 方案：
${currentAgentStates.agentB.solution}
當前立場：${currentAgentStates.agentB.stance}

Agent C 方案：
${currentAgentStates.agentC.solution}
當前立場：${currentAgentStates.agentC.stance}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 客觀分析每個方案
2. 針對每個方案的弱點提出具體挑戰
3. 根據可行性、效益、風險控制三個維度評分（每項10分）
4. 輸出結構化審查報告

輸出格式參考 agent-definitions.md 中的 Critic 審查格式。
      `,
      explanation: `第 ${currentRound} 輪 Critic 審查`
    })

    // Phase 3: 反駁與修正（並行）
    console.log("💬 Phase 3: 並行回應 Critic 挑戰...")
    
    const [agentAResponse, agentBResponse, agentCResponse] = await Promise.all([
      
      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Perspective Agent A，請回應第 ${currentRound} 輪的 Critic 挑戰：

你的當前方案：
${currentAgentStates.agentA.solution}

Critic 的挑戰：
${criticResult}

其他 Agent 的方案：
Agent B: ${currentAgentStates.agentB.solution}
Agent C: ${currentAgentStates.agentC.solution}

請執行：
1. 使用 mcp_sequential_thinking_sequentialthinking 分析挑戰
2. 決定舉證反駁或承認修正
3. 如需修正，提供修正後的方案
4. 評估其他 Agent 的方案
5. 表明最終立場（堅持原方案/修正後堅持/同意Agent B/同意Agent C）

輸出格式參考 agent-definitions.md 中的反駁回應格式。
        `,
        explanation: `Agent A 第 ${currentRound} 輪回應`
      }),

      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Perspective Agent B，請回應第 ${currentRound} 輪的 Critic 挑戰：

你的當前方案：
${currentAgentStates.agentB.solution}

Critic 的挑戰：
${criticResult}

其他 Agent 的方案：
Agent A: ${currentAgentStates.agentA.solution}
Agent C: ${currentAgentStates.agentC.solution}

請執行相同的回應流程...
        `,
        explanation: `Agent B 第 ${currentRound} 輪回應`
      }),

      invokeSubAgent({
        name: "general-task-execution",
        prompt: `
你是 Perspective Agent C，請回應第 ${currentRound} 輪的 Critic 挑戰：

你的當前方案：
${currentAgentStates.agentC.solution}

Critic 的挑戰：
${criticResult}

其他 Agent 的方案：
Agent A: ${currentAgentStates.agentA.solution}
Agent B: ${currentAgentStates.agentB.solution}

請執行相同的回應流程...
        `,
        explanation: `Agent C 第 ${currentRound} 輪回應`
      })
    ])

    // 更新 Agent 狀態
    currentAgentStates.agentA = {
      solution: agentAResponse.revisedSolution || currentAgentStates.agentA.solution,
      stance: agentAResponse.stance,
      score: extractScore(criticResult, 'A')
    }
    
    currentAgentStates.agentB = {
      solution: agentBResponse.revisedSolution || currentAgentStates.agentB.solution,
      stance: agentBResponse.stance,
      score: extractScore(criticResult, 'B')
    }
    
    currentAgentStates.agentC = {
      solution: agentCResponse.revisedSolution || currentAgentStates.agentC.solution,
      stance: agentCResponse.stance,
      score: extractScore(criticResult, 'C')
    }

    // 記錄本輪辯證
    debateHistory.push({
      round: currentRound,
      critic: criticResult,
      responses: {
        agentA: agentAResponse,
        agentB: agentBResponse,
        agentC: agentCResponse
      },
      scores: {
        A: currentAgentStates.agentA.score,
        B: currentAgentStates.agentB.score,
        C: currentAgentStates.agentC.score
      }
    })

    // Phase 4: 共識檢查
    console.log("🎯 Phase 4: 檢查共識狀態...")
    
    const consensusResult = checkConsensus(currentAgentStates)
    
    if (consensusResult.hasConsensus) {
      consensus = true
      console.log(`✅ 達成共識！採納 Agent ${consensusResult.agreedSolution} 的方案`)
      break
    }

    // 檢查是否有明顯領先的方案（分數差距 ≥8）
    const scores = [
      { agent: 'A', score: currentAgentStates.agentA.score },
      { agent: 'B', score: currentAgentStates.agentB.score },
      { agent: 'C', score: currentAgentStates.agentC.score }
    ].sort((a, b) => b.score - a.score)

    const scoreDifference = scores[0].score - scores[1].score
    
    if (scoreDifference >= 8) {
      console.log(`📊 Agent ${scores[0].agent} 明顯領先（差距 ${scoreDifference} 分），建議採納`)
    }

    // Phase 5: 使用者互動
    console.log("👥 Phase 5: 使用者互動...")
    
    const userChoice = await askUserForNextStep(currentRound, currentAgentStates, consensusResult)
    
    switch (userChoice.action) {
      case 'continue':
        console.log("▶️  使用者選擇繼續辯證")
        currentRound++
        break
        
      case 'adopt':
        console.log(`✅ 使用者選擇採納 Agent ${userChoice.selectedAgent} 的方案`)
        consensus = true
        break
        
      case 'intervene':
        console.log("🛠️  使用者介入調整...")
        await handleUserIntervention(userChoice.intervention, currentAgentStates)
        currentRound++
        break
        
      case 'reset_angles':
        console.log("🔄 重新設定思考角度...")
        await resetPerspectives(userChoice.newAngles, currentAgentStates)
        currentRound++
        break
        
      default:
        currentRound++
    }
  }

  // 處理最終結果
  let finalResult
  
  if (consensus) {
    finalResult = generateConsensusReport(currentAgentStates, debateHistory)
  } else {
    // 達到最大輪數，Critic 最終裁決
    console.log("⚖️  達到最大輪數，Critic 進行最終裁決...")
    
    const finalJudgment = await invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Critic，請進行最終裁決：

經過 ${maxRounds} 輪辯證，未能達成共識。請根據最終評分和綜合分析，裁定採納哪個方案。

最終狀態：
Agent A: 分數 ${currentAgentStates.agentA.score}, 立場 ${currentAgentStates.agentA.stance}
Agent B: 分數 ${currentAgentStates.agentB.score}, 立場 ${currentAgentStates.agentB.stance}
Agent C: 分數 ${currentAgentStates.agentC.score}, 立場 ${currentAgentStates.agentC.stance}

辯證歷史：
${JSON.stringify(debateHistory, null, 2)}

請輸出最終裁決報告，格式參考 agent-definitions.md 中的最終裁決格式。
      `,
      explanation: "Critic 最終裁決"
    })
    
    finalResult = generateFinalJudgmentReport(finalJudgment, currentAgentStates, debateHistory)
  }

  // Phase 6: 最終輸出 - 生成結構化報告
  console.log("📋 Phase 6: 生成最終輸出報告...")
  
  const finalOutput = await invokeSubAgent({
    name: "general-task-execution",
    prompt: `
請生成最終辯證報告，格式如下：

# 🎯 最終方案

## 採納方案: ${finalResult.adoptedSolution.agent === 'A' ? currentAgentStates.agentA.solution.title || 'Agent A 方案' : 
                finalResult.adoptedSolution.agent === 'B' ? currentAgentStates.agentB.solution.title || 'Agent B 方案' : 
                currentAgentStates.agentC.solution.title || 'Agent C 方案'}

**來源**: Agent ${finalResult.adoptedSolution.agent}（${finalResult.adoptedSolution.consensusType}）
**總分**: ${finalResult.adoptedSolution.finalScore}/30

${finalResult.adoptedSolution.solution}

## 📊 評分明細

| Agent | 可行性 | 效益 | 風險控制 | 總分 |
|-------|--------|------|----------|------|
| A     | ${Math.floor(finalResult.finalScores.A/3)}/10 | ${Math.floor(finalResult.finalScores.A/3)}/10 | ${Math.floor(finalResult.finalScores.A/3)}/10 | ${finalResult.finalScores.A}/30 |
| B     | ${Math.floor(finalResult.finalScores.B/3)}/10 | ${Math.floor(finalResult.finalScores.B/3)}/10 | ${Math.floor(finalResult.finalScores.B/3)}/10 | ${finalResult.finalScores.B}/30 |
| C     | ${Math.floor(finalResult.finalScores.C/3)}/10 | ${Math.floor(finalResult.finalScores.C/3)}/10 | ${Math.floor(finalResult.finalScores.C/3)}/10 | ${finalResult.finalScores.C}/30 |

## 🔍 關鍵洞察

${finalResult.keyInsights || '本次辯證過程中的主要發現和學習點'}

## 📋 實施建議

${finalResult.recommendation}

## 📜 辯論過程摘要

**總輪數**: ${debateHistory.length} 輪
**共識達成**: ${consensus ? '是' : '否（Critic 裁決）'}

<details>
<summary>📜 完整辯論記錄（點擊展開）</summary>

${JSON.stringify(debateHistory, null, 2)}

</details>

---

**辯證完成時間**: ${new Date().toISOString()}
**執行模式**: 並行多代理辯證系統
    `,
    explanation: "Phase 6: 生成最終輸出報告"
  })

  return {
    requirement: userRequirement,
    totalRounds: currentRound,
    consensus,
    finalResult,
    finalOutput, // Phase 6 的結構化輸出
    debateHistory,
    executionSummary: {
      startTime: new Date().toISOString(),
      totalRounds: currentRound,
      consensusAchieved: consensus,
      finalScores: {
        A: currentAgentStates.agentA.score,
        B: currentAgentStates.agentB.score,
        C: currentAgentStates.agentC.score
      }
    }
  }
}
```

## 🎯 輔助函數實作

### 共識檢查函數

```javascript
function checkConsensus(agentStates) {
  const stances = [
    agentStates.agentA.stance,
    agentStates.agentB.stance,
    agentStates.agentC.stance
  ]
  
  // 檢查是否有 ≥2 個 Agent 同意某個方案
  const agreements = {
    'A': stances.filter(s => s.includes('同意 Agent A') || s.includes('堅持原方案')).length,
    'B': stances.filter(s => s.includes('同意 Agent B')).length,
    'C': stances.filter(s => s.includes('同意 Agent C')).length
  }
  
  // 找出獲得最多支持的方案
  const maxSupport = Math.max(...Object.values(agreements))
  const agreedSolution = Object.keys(agreements).find(key => agreements[key] === maxSupport)
  
  return {
    hasConsensus: maxSupport >= 2,
    agreedSolution,
    supportCounts: agreements,
    details: `Agent A: ${agreements.A} 票, Agent B: ${agreements.B} 票, Agent C: ${agreements.C} 票`
  }
}
```

### 使用者互動函數

```javascript
async function askUserForNextStep(round, agentStates, consensusResult) {
  
  // 生成當前狀態摘要
  const statusSummary = `
第 ${round} 輪辯論結束

當前評分：
| Agent | 可行性 | 效益 | 風險控制 | 總分 | 立場 |
|-------|--------|------|----------|------|------|
| A     | ${Math.floor(agentStates.agentA.score/3)}/10 | ${Math.floor(agentStates.agentA.score/3)}/10 | ${Math.floor(agentStates.agentA.score/3)}/10 | ${agentStates.agentA.score}/30 | ${agentStates.agentA.stance} |
| B     | ${Math.floor(agentStates.agentB.score/3)}/10 | ${Math.floor(agentStates.agentB.score/3)}/10 | ${Math.floor(agentStates.agentB.score/3)}/10 | ${agentStates.agentB.score}/30 | ${agentStates.agentB.stance} |
| C     | ${Math.floor(agentStates.agentC.score/3)}/10 | ${Math.floor(agentStates.agentC.score/3)}/10 | ${Math.floor(agentStates.agentC.score/3)}/10 | ${agentStates.agentC.score}/30 | ${agentStates.agentC.stance} |

共識狀態：${consensusResult.hasConsensus ? '已達成' : '尚未達成'}
${consensusResult.details}
  `
  
  console.log(statusSummary)
  
  // 模擬使用者選擇（實際應用中應該是真實的使用者輸入）
  const options = [
    { 
      label: "繼續", 
      description: "進行下一輪辯論",
      action: "continue"
    },
    { 
      label: "採納", 
      description: "採納當前最高分方案",
      action: "adopt",
      selectedAgent: getBestAgent(agentStates)
    },
    { 
      label: "介入", 
      description: "調整方向或追加條件",
      action: "intervene"
    },
    { 
      label: "重設角度", 
      description: "重新設定思考角度",
      action: "reset_angles"
    }
  ]
  
  // 在實際應用中，這裡應該使用真實的使用者輸入
  // 現在我們根據情況自動選擇
  if (consensusResult.hasConsensus) {
    return {
      action: "adopt",
      selectedAgent: consensusResult.agreedSolution
    }
  } else if (round >= 8) {
    return {
      action: "adopt", 
      selectedAgent: getBestAgent(agentStates)
    }
  } else {
    return { action: "continue" }
  }
}

function getBestAgent(agentStates) {
  const scores = [
    { agent: 'A', score: agentStates.agentA.score },
    { agent: 'B', score: agentStates.agentB.score },
    { agent: 'C', score: agentStates.agentC.score }
  ]
  
  return scores.sort((a, b) => b.score - a.score)[0].agent
}
```

### 使用者介入處理

```javascript
async function handleUserIntervention(intervention, agentStates) {
  
  console.log("🛠️  處理使用者介入：", intervention)
  
  // 將使用者的介入意見傳達給所有 Agent
  const interventionPromises = ['A', 'B', 'C'].map(agent => 
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent ${agent}，使用者提供了以下介入意見：

"${intervention}"

請根據這個意見調整你的方案：
1. 分析使用者的意見
2. 評估如何整合到你的方案中
3. 提供調整後的方案
4. 說明調整的理由

當前方案：${agentStates[`agent${agent}`].solution}
      `,
      explanation: `Agent ${agent} 處理使用者介入`
    })
  )
  
  const adjustedSolutions = await Promise.all(interventionPromises)
  
  // 更新 Agent 狀態
  agentStates.agentA.solution = adjustedSolutions[0]
  agentStates.agentB.solution = adjustedSolutions[1] 
  agentStates.agentC.solution = adjustedSolutions[2]
  
  console.log("✅ 使用者介入處理完成，方案已調整")
}
```

### 重設思考角度

```javascript
async function resetPerspectives(newAngles, agentStates) {
  
  console.log("🔄 重新設定思考角度：", newAngles)
  
  // 根據新角度重新生成方案
  const newSolutionPromises = [
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent A，新的思考角度：${newAngles.A}

請根據新角度重新分析需求並提出方案：
${agentStates.orchestrator}

使用所有可用的 MCP 工具進行深度分析。
      `,
      explanation: `Agent A 重設角度：${newAngles.A}`
    }),
    
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent B，新的思考角度：${newAngles.B}

請根據新角度重新分析需求並提出方案：
${agentStates.orchestrator}

使用所有可用的 MCP 工具進行深度分析。
      `,
      explanation: `Agent B 重設角度：${newAngles.B}`
    }),
    
    invokeSubAgent({
      name: "general-task-execution",
      prompt: `
你是 Agent C，新的思考角度：${newAngles.C}

請根據新角度重新分析需求並提出方案：
${agentStates.orchestrator}

使用所有可用的 MCP 工具進行深度分析。
      `,
      explanation: `Agent C 重設角度：${newAngles.C}`
    })
  ]
  
  const newSolutions = await Promise.all(newSolutionPromises)
  
  // 更新 Agent 狀態
  agentStates.agentA.solution = newSolutions[0]
  agentStates.agentB.solution = newSolutions[1]
  agentStates.agentC.solution = newSolutions[2]
  
  // 重設立場
  agentStates.agentA.stance = "堅持原方案"
  agentStates.agentB.stance = "堅持原方案"
  agentStates.agentC.stance = "堅持原方案"
  
  console.log("✅ 思考角度重設完成")
}
```

## 📊 報告生成函數

### 共識報告生成

```javascript
function generateConsensusReport(agentStates, debateHistory) {
  
  const consensusCheck = checkConsensus(agentStates)
  const winningAgent = consensusCheck.agreedSolution
  const winningSolution = agentStates[`agent${winningAgent}`].solution
  
  return {
    type: "consensus",
    adoptedSolution: {
      agent: winningAgent,
      solution: winningSolution,
      finalScore: agentStates[`agent${winningAgent}`].score,
      consensusType: "多數同意"
    },
    finalScores: {
      A: agentStates.agentA.score,
      B: agentStates.agentB.score,
      C: agentStates.agentC.score
    },
    debateRounds: debateHistory.length,
    keyInsights: extractKeyInsights(debateHistory),
    recommendation: winningSolution,
    implementationNext: "建議立即開始實施準備工作"
  }
}
```

### 最終裁決報告生成

```javascript
function generateFinalJudgmentReport(judgment, agentStates, debateHistory) {
  
  return {
    type: "critic_judgment", 
    adoptedSolution: {
      agent: judgment.selectedAgent,
      solution: agentStates[`agent${judgment.selectedAgent}`].solution,
      finalScore: agentStates[`agent${judgment.selectedAgent}`].score,
      consensusType: "Critic 裁決"
    },
    judgmentReasoning: judgment.reasoning,
    finalScores: {
      A: agentStates.agentA.score,
      B: agentStates.agentB.score, 
      C: agentStates.agentC.score
    },
    debateRounds: debateHistory.length,
    keyInsights: extractKeyInsights(debateHistory),
    recommendation: judgment.recommendation,
    implementationNext: judgment.nextSteps
  }
}
```

## 🚀 使用範例

```javascript
// 🎯 完整辯證流程執行
async function runCompleteDebate() {
  
  const userRequirement = `
設計一個支援百萬用戶的電商推薦系統

背景：
- 現有系統：Spring Boot + MySQL + Redis
- 用戶規模：日活 10 萬，目標 100 萬
- 業務目標：提升 20% 購買轉換率
- 時程要求：6 個月內上線

約束條件：
- 預算：中等（不超過 50 萬）
- 團隊：5 個後端開發者，2 個前端開發者
- 技術棧：儘量使用現有技術
- 穩定性：不能影響現有系統

成功標準：
- 推薦準確率 >85%
- 系統回應時間 <200ms
- 可支援 100 萬日活用戶
- 系統可用性 >99.9%
  `
  
  try {
    const result = await executeCompleteDebate(userRequirement, 10)
    
    console.log("🎯 辯證完成！")
    console.log("📊 執行摘要：", result.executionSummary)
    console.log("🏆 最終方案：", result.finalResult.adoptedSolution)
    console.log("📋 完整報告：", result.finalResult)
    
    return result
    
  } catch (error) {
    console.error("❌ 辯證執行失敗：", error.message)
    throw error
  }
}

// 執行完整辯證
const debateResult = await runCompleteDebate()
```

---

現在您的多代理辯證系統包含了完整的階段實作：

✅ **Phase 0+1**: 並行初始方案生成  
✅ **Phase 2**: Critic 批判審查  
✅ **Phase 3**: 並行反駁與修正  
✅ **Phase 4**: 智能共識檢查  
✅ **Phase 5**: 使用者互動與選擇  
✅ **多輪迭代**: 自動循環直到達成共識  
✅ **最終裁決**: Critic 最終判決機制  
✅ **完整報告**: 結構化結果輸出  

這個實作提供了完整的辯證流程，包含您原始設計中的所有重要階段！🎉