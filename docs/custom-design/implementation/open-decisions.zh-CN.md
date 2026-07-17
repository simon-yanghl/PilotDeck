# 实施阶段待定项

以下事项不阻塞 V1 设计，由 Claude Code 在源码分析后提出选项，用户确认后实施。

1. 每个模型的实际 `maxContextTokens` 运行预算。
2. 每个模型的实际 `maxOutputTokens`。
3. Vision JSON 的最终字段、长度限制与不同图片类型扩展方式。
4. 人工覆盖采用 `/model`、`@pro/@vision`、UI，或多种方式并存。
5. Workspace metadata 和 Archive 的最终路径及与现有数据库的复用程度。
6. Guardian Skill 使用 PilotDeck 原生 Skill、Workflow 定义还是内部扩展的具体组合。
7. general/complex 的通用审查流程细节。
8. 路由理由是否直接展示完整文本，还是只展示标准化 reason code。
9. 日志查询、人工归档标签和导出方式。
