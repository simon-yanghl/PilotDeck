# 01. 总体架构

## 1. 定位

PilotDeck Custom 仍是通用智能体，不被限定为编程工具。它通过顶层领域识别，在工程任务中启用更严格的工程治理，在通用任务中保持轻量和自然。

## 2. 核心组件

```text
Input Gateway
├─ Media Detector
│  └─ Vision Preprocessor (Qwen)
├─ Unified Judge (DeepSeek Flash)
│  └─ domain + tier
├─ Model Router
├─ Agent Orchestrator
├─ Engineering Governance Layer
├─ Workspace Resolver
├─ Memory + Archive
└─ Observability UI / Logs
```

## 3. 模型分工

- DeepSeek V4 Flash：Judge、standard、普通 Worker、实现监管、轻量测试监管。
- DeepSeek V4 Pro：deep、complex 主控、规划审查、架构审查、最终整合与审计。
- Qwen 3.7 Plus：图片预处理与结构化视觉分析。
- 豆包：保留 Provider，但 V1 不参与自动路由或 fallback。

## 4. 路由顺序

1. 检测当前请求是否含图片。
2. 含图片时先由 Qwen 生成结构化 JSON；原始图片不发送给 DeepSeek。
3. 将用户当前请求与视觉 JSON 交给 Judge。
4. Judge 一次返回 `domain + tier + reason`。
5. Router 选择模型或 Workflow。
6. 执行、监管、审查和测试。
7. UI 显示实际模型、领域、等级、来源和原因。

## 5. 关键约束

- Judge 不接收完整对话历史，只接收当前用户请求、必要元数据、可选上一轮 tier 提示和视觉 JSON。
- 主 Agent 与子 Agent 不共享完整上下文；主 Agent必须生成自包含任务说明。
- 只允许一层编排；Worker 不得继续创建 Agent。
- complex 最终由 DeepSeek Pro 汇总和验收。
- 模型级上下文与输出限制优先；Agent 全局配置仅作兜底。
- 工程安全规则具有最高优先级，低级规则不能覆盖。
