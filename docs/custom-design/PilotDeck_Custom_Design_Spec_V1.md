# PilotDeck Custom Design Specification V1

> 本文件是整套文档的合并摘要。详细内容以各分册为准。

## 决策摘要

- PilotDeck 保持通用智能体定位。
- 输入先做图片检测；图片由 Qwen 预处理成结构化 JSON，DeepSeek 不接收原始图片。
- Judge 使用 DeepSeek V4 Flash，每轮只判断当前请求，一次返回：

```json
{"domain":"engineering|general","tier":"standard|deep|complex","reason":"..."}
```

- engineering 和 general 均采用三级能力：
  - standard：DeepSeek V4 Flash
  - deep：DeepSeek V4 Pro
  - complex：DeepSeek V4 Pro + Agent 编排
- engineering/complex 启用完整工程治理；general 保持通用、轻量。
- 初始 Judge 可直接选择 complex；运行中自动升级只允许 standard→deep、deep→complex。
- 只允许一层编排，子 Agent 不继承完整上下文，也不得继续创建 Agent。
- complex 由 DeepSeek Pro 规划、汇总和最终验收。
- 模型级 context/output 优先，Agent 全局值只作兜底；具体限制由用户填写。
- Workspace 使用目录自动识别 + 手动覆盖。
- Memory 分 Global 与 Workspace；Tool 原始结果归档，摘要与索引进入 Memory。
- 工程任务应用全局工程策略：简单、复用、最小修改、可读、可维护、充分测试。
- 工程治理包括 Plan Review、Implementation Guardian、Architecture Review、Simplification Review、Test Guardian 和 Final Audit。
- 正常 Worker 拥有完成任务所需权限；高危险操作必须人工明确确认或人工执行。
- UI 显著显示实际模型、domain、tier、reason、source 和 fallback。
- 使用 Fork + custom 分支长期跟随 upstream；先设计、后由 Claude Code 实施。

## 文档入口

- 总体架构：`docs/01-architecture.zh-CN.md`
- 路由与视觉：`docs/02-router-and-vision.zh-CN.md`
- 工程治理：`docs/03-engineering-governance.zh-CN.md`
- Workspace/Memory/Archive：`docs/04-workspace-memory-archive.zh-CN.md`
- 安全权限：`docs/05-security-and-permissions.zh-CN.md`
- UI 与日志：`docs/06-observability-and-ui.zh-CN.md`
- Fork 与部署：`docs/07-fork-upgrade-deployment.zh-CN.md`
- Claude Code 指导：`implementation/CLAUDE_IMPLEMENTATION_GUIDE.md`
