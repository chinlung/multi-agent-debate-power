# 實作執行指南

本文件說明如何使用 Kiro 的 subagent 系統和 MCP 工具來實際執行多代理辯證系統。

## ✅ 並行執行支援

**Kiro Subagent 並行執行**：
- Kiro **支援真正的並行 subagent 執行**
- 關鍵是在**同一個 function_calls block** 中同時發起多個 `invokeSubAgent` 調用
- 三個 Perspective Agent 可以同時啟動，大幅縮短執行時間

## 🚀 並行執行流程

### Phase 0：Orchestrator 需求分析（必須先完成）

Orchestrator 必須先完成分析，才能為三個 Agent 配置思考角度：

```javascript
// 🎯 Phase 0：Orchestrator 先行分析
const orchestrator = await invokeSubAgent({
  name: "general-task-execution",
  prompt: `你是 Orchestrator（協調者）。需求：${userRequirement}
請分析需求並決定三個 Agent 的思考角度。`,
  explanation: "Phase 0: Orchestrator 分析需求"
})

// 從結果提取角度配置
const angleConfig = extractAnglesFromOrchestrator(orchestrator)
```

### Phase 1：三個 Agent 並行生成方案 ⚡

**關鍵**：在同一個 `<function_calls>` block 中同時發起三個 `invokeSubAgent` 調用

```xml
<!-- 🚀 並行執行：三個 Agent 同時啟動 -->
<function_calls>
  <invoke name="invokeSubAgent">
    <parameter name="name">general-task-execution</parameter>
    <parameter name="prompt">你是 Agent A，角度：效能優先...</parameter>
    <parameter name="explanation">Agent A 提出方案</parameter>
  </invoke>
  <invoke name="invokeSubAgent">
    <parameter name="name">general-task-execution</parameter>
    <parameter name="prompt">你是 Agent B，角度：品質優先...</parameter>
    <parameter name="explanation">Agent B 提出方案</parameter>
  </invoke>
  <invoke name="invokeSubAgent">
    <parameter name="name">general-task-execution</parameter>
    <parameter name="prompt">你是 Agent C，角度：平衡性優先...</parameter>
    <parameter name="explanation">Agent C 提出方案</parameter>
  </invoke>
</function_calls>
```

### Phase 2：Critic 審查

```javascript
const critic = await invokeSubAgent({
  name: "general-task-execution",
  prompt: `你是 Critic，請審查方案：
Agent A: ${agentA}
Agent B: ${agentB}
Agent C: ${agentC}`,
  explanation: "Critic 審查所有方案"
})
```

### Phase 3：三個 Agent 並行回應挑戰 ⚡

同樣在一個 block 中並行執行：

```xml
<function_calls>
  <invoke name="invokeSubAgent">
    <parameter name="prompt">Agent A 回應 Critic 挑戰...</parameter>
  </invoke>
  <invoke name="invokeSubAgent">
    <parameter name="prompt">Agent B 回應 Critic 挑戰...</parameter>
  </invoke>
  <invoke name="invokeSubAgent">
    <parameter name="prompt">Agent C 回應 Critic 挑戰...</parameter>
  </invoke>
</function_calls>
```

## 📊 執行時間對比

| 執行模式 | Phase 1 | Phase 3 | 總時間 |
|----------|---------|---------|--------|
| **序列執行** | 3-6 分鐘 | 3-6 分鐘 | 9-18 分鐘 |
| **並行執行** | 1-2 分鐘 | 1-2 分鐘 | 3-6 分鐘 |

**效能提升：60-70%**

## 🔧 MCP 工具使用

### Sequential Thinking
```javascript
mcp_sequential_thinking_sequentialthinking({
  thought: "分析需求的核心問題",
  nextThoughtNeeded: true,
  thoughtNumber: 1,
  totalThoughts: 5
})
```

### Context7
```javascript
mcp_context7_resolve_library_id({ libraryName: "React" })
mcp_context7_get_library_docs({
  context7CompatibleLibraryID: "/facebook/react",
  topic: "hooks"
})
```

### Serena
```javascript
mcp_serena_get_symbols_overview({ relative_path: "src" })
mcp_serena_search_for_pattern({ substring_pattern: "useEffect" })
```

## 🎯 完整執行範例

```javascript
async function executeMultiAgentDebate(userRequirement) {
  
  // Phase 0: Orchestrator 分析
  const orchestrator = await invokeSubAgent({
    name: "general-task-execution",
    prompt: `Orchestrator 分析：${userRequirement}`,
    explanation: "Orchestrator 分析"
  })
  
  const angleConfig = extractAngles(orchestrator)
  
  // Phase 1: 三個 Agent 並行執行（在同一個 function_calls block）
  // Agent A, B, C 同時啟動...
  
  // Phase 2: Critic 審查
  const critic = await invokeSubAgent({...})
  
  // Phase 3: 三個 Agent 並行回應（在同一個 function_calls block）
  // Agent A, B, C 同時回應...
  
  // Phase 4-6: 共識檢查、使用者互動、最終報告
  return finalReport
}
```

---

透過並行執行，多代理辯證系統可以在 3-6 分鐘內完成完整的辯證流程！🚀
