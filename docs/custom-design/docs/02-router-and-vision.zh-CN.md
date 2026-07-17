# 02. 路由与视觉预处理设计

## 1. 顶层领域

### engineering

用户要求实际创建、修改、调试、测试、部署、配置或维护技术系统。

### general

日常对话、解释、总结、翻译、改写、文档整理、分析、研究及不直接修改技术系统的工作。

判断依据是任务结果，而不是是否出现技术名词。

## 2. 三级能力

### standard

- 模型：DeepSeek V4 Flash。
- 用途：普通问答、简单代码、边界清楚的小修改、常规文件操作、短文本处理。
- 原则：Flash 能可靠完成时不升级。

### deep

- 模型：DeepSeek V4 Pro。
- 用途：复杂调试、架构决策、多步骤分析、重要修改、高可靠任务。
- 原则：难度并不等于需要编排；能由单个专家完成就保持 deep。

### complex

- 模型：DeepSeek V4 Pro + Agent 编排。
- 用途：能明显拆成独立并行工作流、多模块实施、多来源研究或专业角色协作的任务。
- 原则：complex 表示“需要编排”，不是“很难”。

## 3. Judge 调用成本

保留 PilotDeck 现有的轻量思路：只抽取当前最后一条用户请求，不拼接完整历史。可传入上一轮 tier 作为短续写提示，但不能发送大段上下文。

Judge 应使用：

- temperature = 0
- thinking disabled
- 小输出限制
- 最多有限次数重试
- 严格 JSON 校验

## 4. 初始分类与运行中升级

- 初始 Judge 可以直接选择 standard、deep 或 complex，否则复杂任务无法正常进入编排。
- 已执行过程中，模型主动升级只允许逐级：`standard → deep`、`deep → complex`。
- 禁止运行中从 standard 直接升级到 complex。

## 5. 非法输出兜底

合法值：

```text
domain: engineering | general
tier: standard | deep | complex
```

规则：

1. 非法 JSON、空响应、超时或未知值先按有限次数重试。
2. domain 合法、tier 非法：engineering 退到 deep；general 退到 standard。
3. tier 合法、domain 非法：优先按 Workspace 类型推断。
4. 两者均失败：工程 Workspace 使用 engineering/deep；普通 Workspace 使用 general/standard。
5. 任何异常都不得兜底到 complex。
6. 日志保存 Judge 原始输出、错误、最终选择和 fallback 原因。

## 6. 图片处理

### 处理位置

图片必须在路由入口被检测和解析，而不是先交给 DeepSeek 再决定是否调用视觉能力。

### 目标流程

```text
User request + image
→ detect image
→ Qwen vision preprocessing
→ structured JSON
→ archive raw vision result
→ Judge receives user request + concise vision JSON
→ DeepSeek continues the task
```

### 推荐接口

它在实现上可以暴露为内部 Tool 接口，但调用者是路由预处理层，不是 DeepSeek 主 Agent。不要使用 MCP；MCP 更适合外部系统能力。

### 视觉 JSON V1 草案

```json
{
  "summary": "short overall description",
  "key_elements": ["important objects or regions"],
  "text_ocr": "recognized text when relevant",
  "useful_details": ["details relevant to the request"],
  "uncertainties": ["ambiguous or low-confidence observations"],
  "archive_id": "reference to raw result"
}
```

字段和长度在实现阶段根据实际图片任务微调。
