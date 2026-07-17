# 实施阶段计划

## Phase 0：Fork 与基线

- Fork 官方仓库，配置 upstream/origin/custom。
- 记录当前 commit、构建方式和测试基线。
- 备份配置并创建 PVE 快照。
- 不修改功能。

## Phase 1：安全的配置与路由基础

- 模型级 context/output 优先级验证与测试。
- 将现有 tier 改为 standard/deep/complex。
- 固化英文 Judge Prompt 与严格输出解析。
- 增强非法输出、超时、未知值的 fallback 日志。
- 增加路由模型状态显示的最小版本。

## Phase 2：统一 domain + tier 分类

- Judge 一次返回 engineering/general + tier。
- 实现 Workspace 感知 fallback。
- 保持当前配置向后兼容。
- 增加分类测试集，包括中文、短续写和边界任务。

## Phase 3：视觉预处理

- 在路由入口检测图片。
- 复用现有 media modality 识别代码。
- 调用 Qwen，生成结构化 JSON。
- Archive 原始结果，向 Judge/DeepSeek 注入精简结构化上下文。
- 验证 DeepSeek 不接收原始图片。

## Phase 4：Workspace、Memory 与 Archive

- 目录自动识别、手动覆盖、首次创建提示。
- Global/Workspace Memory 隔离。
- SQLite 索引 + 文件 payload；优先复用现有数据层。
- Tool 原始结果与摘要生命周期。

## Phase 5：工程治理 Skills 与 Workflow

- plan-reviewer
- implementation-guardian
- architecture-reviewer
- simplification-reviewer
- test-guardian
- final-engineering-audit
- 分别实现 standard/deep/complex 的治理强度。

## Phase 6：完整 UI、日志与人工覆盖

- 当前模型、domain、tier、reason、source、fallback 可视化。
- 长期日志与查询。
- `/model`、别名或 UI 覆盖方式择一实现，并支持扩展。

## Phase 7：迁移、回归与部署

- 配置兼容与迁移测试。
- 官方原版与 custom 对照测试。
- 升级、回滚、服务重启和数据恢复演练。
- Final Engineering Audit。
