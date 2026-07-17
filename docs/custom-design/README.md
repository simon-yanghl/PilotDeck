# PilotDeck Custom Design V1

版本日期：2026-07-17

本文件包是 PilotDeck 定制版的**设计规格与实施指导**。当前阶段只做规划，不直接修改 PilotDeck 源码；后续由 Claude Code 按本规格分析源码、提出最小修改方案、实施、测试并汇报。

## 设计目标

1. 保留 PilotDeck 的通用智能体定位，同时强化编程、系统设计与长期项目维护能力。
2. DeepSeek 作为主要文字模型；Qwen 作为图片预处理模型；豆包暂不参与自动路由。
3. 路由一次判断任务领域与复杂度，避免重复 Judge 调用。
4. 工程任务引入规划、实现、审查、测试与最终审计的治理流程。
5. 全局强调简单、复用、可读、可测试、可维护，不以“能运行”为唯一目标。
6. 尽量复用 PilotDeck 现有抽象，保持与官方主线持续同步的能力。

## 目标路由

```text
输入
├─ 含图片
│  └─ 路由入口预处理 → Qwen Vision → 结构化 JSON → Judge
└─ 纯文字
   └─ Judge 一次返回 domain + tier

Domain
├─ engineering
│  ├─ standard → DeepSeek V4 Flash
│  ├─ deep     → DeepSeek V4 Pro
│  └─ complex  → DeepSeek V4 Pro + 多 Agent 编排 + 工程治理
└─ general
   ├─ standard → DeepSeek V4 Flash
   ├─ deep     → DeepSeek V4 Pro
   └─ complex  → DeepSeek V4 Pro + 通用编排
```

## 目录

- `config/`：当前配置快照、可立即采用的三级路由配置、目标自定义配置草案。
- `prompts/`：英文运行提示词与中文维护版。
- `docs/`：架构、路由、视觉、治理、Workspace、Memory、安全、可观测性与升级设计。
- `implementation/`：Claude Code 实施指引、阶段计划、验收标准和待定项。

## 先读顺序

1. `docs/01-architecture.zh-CN.md`
2. `docs/02-router-and-vision.zh-CN.md`
3. `docs/03-engineering-governance.zh-CN.md`
4. `prompts/global-engineering-policy.en.md`
5. `implementation/CLAUDE_IMPLEMENTATION_GUIDE.md`
6. `implementation/acceptance-criteria.zh-CN.md`

## 重要约束

- 本包中的 `config/pilotdeck.custom.target.yaml` 是目标 Schema 草案，官方原版未必能直接加载。
- 每个模型的上下文预算和最大输出均保留占位，部署前由用户按实际模型限制与成本策略填写。
- API Key 不进入 Git 仓库；一律使用环境变量或本地私密配置。
- 所有高危险操作必须由人工明确确认或人工执行。
