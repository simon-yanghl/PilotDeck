# 04. Workspace、Memory 与 Archive

## 1. Workspace 识别

采用“目录自动识别 + 手动覆盖”。优先级：

```text
手动指定 > 当前目录识别 > 默认 Workspace
```

首次进入未登记项目时，PilotDeck 可以询问是否创建 Workspace。

## 2. Workspace 内容

每个项目维护：

- 项目路径和名称
- 项目规则
- 项目摘要
- 项目 Memory Namespace
- Tool Archive
- 模型偏好（可选）
- 最近重要决策与当前状态

## 3. 规则优先级

```text
系统安全规则
> 用户全局工程与安全规则
> Workspace 项目规则
> 当前任务临时要求
```

低级规则不得覆盖高级安全规则。

## 4. Memory 分层

### Global Memory

保存跨项目稳定偏好和环境事实，例如：先讨论后执行、危险命令必须人工确认、主要开发环境等。

### Workspace Memory

只保存当前项目事实、决策、约束、进度和重要路径，禁止不同项目无控制混用。

## 5. Tool 结果生命周期

```text
Tool invocation
→ raw output archived
→ concise summary generated
→ summary + archive_id stored in Memory
→ load raw payload only when needed
```

视觉图片、OCR、长 JSON、PDF 分析等不直接长期塞入主对话。

## 6. 存储方案

- SQLite：索引、摘要、关联、时间线、状态和 archive_id。
- 文件系统：图片、PDF、原始 JSON、长文本和二进制内容。
- 日志与存档长期保留，由人工整理，不自动删除。

建议目录：

```text
~/.pilotdeck/custom/
├── workspaces/
│   └── <workspace-id>/
│       ├── rules.md
│       ├── summary.md
│       └── archive/
├── archive-index.db
└── logs/
```

最终路径应尽量复用 PilotDeck 已有数据层，而不是无条件另起一套系统。
