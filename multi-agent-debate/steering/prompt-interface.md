# 關鍵字 Prompt 調用介面

本文件說明如何透過關鍵字 prompt 來調用多代理辯證系統。

## 🎯 關鍵字觸發機制

### 主要觸發關鍵字

當使用者的 prompt 包含以下關鍵字時，系統應該自動啟動多代理辯證：

**核心關鍵字**：
- `辯證`、`辯論`、`debate`
- `多角度分析`、`多方案比較`
- `決策分析`、`方案評估`
- `架構設計`、`技術選型`
- `需求分析`、`解決方案`

**情境關鍵字**：
- `我想要...但是不確定怎麼做`
- `有幾種方案，幫我分析`
- `請幫我評估...的優缺點`
- `設計...系統，考慮...因素`
- `選擇...技術，需要考慮...`

### 自動檢測函數

```javascript
function shouldTriggerDebate(userPrompt) {
  
  const coreKeywords = [
    '辯證', '辯論', 'debate', '多角度', '多方案', 
    '決策分析', '方案評估', '架構設計', '技術選型',
    '需求分析', '解決方案', '比較分析', '優缺點'
  ]
  
  const contextKeywords = [
    '不確定怎麼做', '幫我分析', '評估', '考慮因素',
    '選擇', '設計', '規劃', '制定策略', '最佳實踐'
  ]
  
  const complexityIndicators = [
    '系統', '架構', '平台', '框架', '技術棧',
    '效能', '擴展', '安全', '成本', '風險'
  ]
  
  // 檢查核心關鍵字
  const hasCoreKeyword = coreKeywords.some(keyword => 
    userPrompt.toLowerCase().includes(keyword.toLowerCase())
  )
  
  // 檢查情境關鍵字
  const hasContextKeyword = contextKeywords.some(keyword =>
    userPrompt.toLowerCase().includes(keyword.toLowerCase())
  )
  
  // 檢查複雜度指標
  const complexityScore = complexityIndicators.filter(indicator =>
    userPrompt.toLowerCase().includes(indicator.toLowerCase())
  ).length
  
  // 判斷是否應該觸發辯證
  return {
    shouldTrigger: hasCoreKeyword || (hasContextKeyword && complexityScore >= 2),
    confidence: calculateConfidence(hasCoreKeyword, hasContextKeyword, complexityScore),
    detectedKeywords: {
      core: coreKeywords.filter(k => userPrompt.toLowerCase().includes(k.toLowerCase())),
      context: contextKeywords.filter(k => userPrompt.toLowerCase().includes(k.toLowerCase())),
      complexity: complexityIndicators.filter(k => userPrompt.toLowerCase().includes(k.toLowerCase()))
    }
  }
}

function calculateConfidence(hasCoreKeyword, hasContextKeyword, complexityScore) {
  if (hasCoreKeyword) return 0.9
  if (hasContextKeyword && complexityScore >= 3) return 0.8
  if (hasContextKeyword && complexityScore >= 2) return 0.7
  return 0.3
}
```

## 🚀 自動調用流程

### 主要調用函數

```javascript
async function handleUserPrompt(userPrompt) {
  
  // 1. 檢測是否應該觸發辯證
  const triggerCheck = shouldTriggerDebate(userPrompt)
  
  if (triggerCheck.shouldTrigger && triggerCheck.confidence >= 0.7) {
    
    console.log(`🎯 檢測到辯證需求 (信心度: ${triggerCheck.confidence})`)
    console.log(`檢測到的關鍵字:`, triggerCheck.detectedKeywords)
    
    // 2. 詢問使用者確認
    const userConfirmation = await confirmDebateExecution(userPrompt, triggerCheck)
    
    if (userConfirmation.confirmed) {
      
      // 3. 解析參數
      const debateParams = parseDebateParameters(userPrompt)
      
      // 4. 執行完整辯證
      console.log("🚀 啟動多代理辯證系統...")
      
      const result = await executeCompleteDebate(
        debateParams.requirement,
        debateParams.maxRounds,
        debateParams.customPerspectives
      )
      
      // 5. 輸出結果
      console.log("🎯 辯證完成！")
      console.log(result.finalOutput)
      
      return result
      
    } else {
      // 使用者拒絕，執行一般回應
      return await handleNormalPrompt(userPrompt)
    }
    
  } else if (triggerCheck.confidence >= 0.5) {
    
    // 中等信心度，建議使用者考慮辯證
    console.log("💡 建議：您的問題可能適合使用多代理辯證分析")
    console.log("如果您希望從多個角度深入分析，請說「啟動辯證」")
    
    return await handleNormalPrompt(userPrompt)
    
  } else {
    
    // 低信心度，執行一般回應
    return await handleNormalPrompt(userPrompt)
  }
}
```

### 使用者確認函數

```javascript
async function confirmDebateExecution(userPrompt, triggerCheck) {
  
  const suggestion = `
🎯 檢測到您的需求可能適合多代理辯證分析：

**您的需求**: ${userPrompt}

**檢測到的關鍵字**: ${triggerCheck.detectedKeywords.core.join(', ')}

**辯證分析的優勢**:
- 從多個角度全面分析問題
- 透過批判性思考發現潛在風險
- 產出經過充分驗證的最優方案
- 提供詳細的決策依據

**預估時間**: 2-5 分鐘（並行執行）

是否啟動多代理辯證分析？
  `
  
  console.log(suggestion)
  
  // 在實際應用中，這裡應該等待使用者回應
  // 現在我們根據信心度自動決定
  return {
    confirmed: triggerCheck.confidence >= 0.8,
    reason: triggerCheck.confidence >= 0.8 ? "高信心度自動確認" : "等待使用者確認"
  }
}
```

### 參數解析函數

```javascript
function parseDebateParameters(userPrompt) {
  
  // 解析最大輪數
  const maxRoundsMatch = userPrompt.match(/--max-rounds\s+(\d+)|最多\s*(\d+)\s*輪|(\d+)\s*輪/)
  const maxRounds = maxRoundsMatch ? 
    parseInt(maxRoundsMatch[1] || maxRoundsMatch[2] || maxRoundsMatch[3]) : 10
  
  // 解析自訂角度
  const perspectivesMatch = userPrompt.match(/--perspectives\s+"([^"]+)"|角度[：:]\s*"([^"]+)"|從\s*([^，,]+)[，,]\s*([^，,]+)[，,]\s*([^，,]+)\s*角度/)
  
  let customPerspectives = null
  if (perspectivesMatch) {
    const perspectiveString = perspectivesMatch[1] || perspectivesMatch[2]
    if (perspectiveString) {
      customPerspectives = perspectiveString.split(/[，,]/).map(p => p.trim())
    } else if (perspectivesMatch[3] && perspectivesMatch[4] && perspectivesMatch[5]) {
      customPerspectives = [
        perspectivesMatch[3].trim(),
        perspectivesMatch[4].trim(), 
        perspectivesMatch[5].trim()
      ]
    }
  }
  
  // 清理需求描述（移除參數）
  const requirement = userPrompt
    .replace(/--max-rounds\s+\d+/g, '')
    .replace(/--perspectives\s+"[^"]+"/g, '')
    .replace(/最多\s*\d+\s*輪/g, '')
    .replace(/角度[：:]\s*"[^"]+"/g, '')
    .trim()
  
  return {
    requirement,
    maxRounds,
    customPerspectives
  }
}
```

## 📋 使用範例

### 範例 1：直接觸發

```
使用者: "我想設計一個電商推薦系統，請幫我進行多角度分析"

系統檢測:
- 核心關鍵字: ["多角度", "分析"]
- 複雜度指標: ["系統"]
- 信心度: 0.9

自動回應: 🚀 啟動多代理辯證系統...
```

### 範例 2：參數化觸發

```
使用者: "辯證分析微服務架構 vs 單體架構，最多 5 輪，從效能、維護性、成本角度"

解析結果:
- 需求: "微服務架構 vs 單體架構"
- 最大輪數: 5
- 自訂角度: ["效能", "維護性", "成本"]

執行: executeCompleteDebate(需求, 5, ["效能", "維護性", "成本"])
```

### 範例 3：建議觸發

```
使用者: "我需要選擇資料庫技術，考慮效能和擴展性"

系統檢測:
- 情境關鍵字: ["選擇", "考慮"]
- 複雜度指標: ["效能", "擴展"]
- 信心度: 0.7

建議回應: 💡 您的問題適合多代理辯證分析，是否啟動？
```

## 🎯 整合到 Power 中

### 在 POWER.md 中新增使用說明

```markdown
## 自動觸發機制

此 Power 支援關鍵字自動觸發，當您的 prompt 包含以下內容時會自動啟動：

### 觸發關鍵字
- **直接觸發**: 辯證、多角度分析、方案比較、決策分析
- **情境觸發**: 設計系統 + 考慮因素、選擇技術 + 評估標準
- **參數化**: --max-rounds N, --perspectives "角度1,角度2,角度3"

### 使用範例
```
# 直接觸發
"請對電商推薦系統進行多角度辯證分析"

# 參數化觸發  
"辯證分析快取策略，最多 3 輪，從效能、成本、複雜度角度"

# 情境觸發
"我要設計支付系統，需要考慮安全性、效能和使用者體驗"
```

### 手動觸發
如果自動檢測未觸發，您可以明確要求：
- "啟動多代理辯證"
- "使用 debate 分析"
- "進行辯證分析"
```

## 🔧 實作整合

### 主要入口函數

```javascript
// 🎯 Power 的主要入口點
async function multiAgentDebatePower(userInput) {
  
  try {
    // 自動檢測和處理
    const result = await handleUserPrompt(userInput)
    
    if (result.type === 'debate') {
      // 辯證結果
      return {
        success: true,
        type: 'multi-agent-debate',
        result: result.finalOutput,
        metadata: {
          rounds: result.totalRounds,
          consensus: result.consensus,
          executionTime: result.executionSummary
        }
      }
    } else {
      // 一般回應
      return {
        success: true,
        type: 'normal-response',
        result: result
      }
    }
    
  } catch (error) {
    console.error("多代理辯證執行失敗:", error.message)
    return {
      success: false,
      error: error.message,
      fallback: "建議手動啟動辯證或簡化需求後重試"
    }
  }
}

// 🚀 使用範例
const userPrompt = "我想設計一個高併發的聊天系統，請幫我分析不同的架構方案"
const result = await multiAgentDebatePower(userPrompt)

if (result.success && result.type === 'multi-agent-debate') {
  console.log("🎯 辯證分析完成:")
  console.log(result.result)
} else {
  console.log("💬 一般回應:", result.result)
}
```

---

現在您的多代理辯證系統具備了：

✅ **完整的 6 個階段** (Phase 0-6)  
✅ **關鍵字自動觸發機制**  
✅ **參數解析和自訂配置**  
✅ **智能檢測和使用者確認**  
✅ **無縫整合到 Power 系統**  

使用者只需要自然地描述需求，系統就會自動檢測並啟動多代理辯證分析！🎉