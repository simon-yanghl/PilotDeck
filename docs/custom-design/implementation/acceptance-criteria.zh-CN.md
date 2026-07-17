# 验收标准

## 路由

- Judge 每轮只接收当前请求和必要元数据，不接收完整历史。
- 一次输出合法 `domain + tier + reason`。
- 中文、英文、短续写和技术词汇边界分类稳定。
- 非法输出、空响应、超时和未知分类不会中断任务。
- 任何失败都不会自动进入 complex。
- 初始任务可正确选择 complex；运行中自动升级不得 standard 直达 complex。

## 模型

- 模型级 context/output 配置优先于 Agent 全局兜底。
- 每个模型限制可独立配置。
- UI 显示实际生效模型，而不是仅显示预期模型。

## 图片

- 带图请求在入口被检测。
- Qwen 接收图片并返回符合 Schema 的 JSON。
- 原始图片不发送给 DeepSeek。
- 视觉原始结果可归档，Memory 只保存摘要与 archive_id。
- 后续纯文字轮次继续由 DeepSeek 主进程处理。

## Agent 编排

- engineering/complex 使用 Pro 规划和最终整合。
- 子 Agent 不继承完整对话，收到自包含任务说明。
- Worker 可以重新判断为 standard/deep，但不能继续创建 Agent。
- 一层编排限制有自动测试。

## 工程治理

- complex 完整执行 Plan Review、Implementation Guardian、Architecture Review、Simplification Review、Test Guardian 和 Final Audit。
- Guardian 默认只读，不修改源代码。
- 审查失败会阻止进入下一阶段，并要求执行 Agent 修订。
- 测试输出包含执行命令、结果和未覆盖风险。

## Workspace / Memory / Archive

- 手动 Workspace > 目录识别 > 默认 Workspace。
- Global 与 Workspace Memory 可验证隔离。
- Archive 索引与原始 payload 均可恢复和检索。
- 日志与存档默认长期保留。

## 安全

- 高危险操作需要明确人工确认或人工执行。
- 所有操作限制在当前 Workspace/仓库范围内。
- API Key 不出现在仓库、日志或测试快照中。

## 升级与维护

- 官方更新可合并到 custom，冲突范围可识别。
- 现有配置在未启用新功能时保持兼容。
- 有明确构建、升级、回滚和服务验证步骤。
