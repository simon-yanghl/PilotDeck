# 03. 工程治理层

## 1. 目的

解决 AI 容易“只求完成、不顾长期维护”的问题。治理层不负责替代执行 Agent，而是为每个重要阶段设置进入下一阶段的质量门槛。

## 2. 全流程

```text
Requirement
→ Planning
→ Plan Guardian
→ Implementation
→ Implementation Guardian
→ Functional Review
→ Architecture + Simplification Review
→ Test Planning and Execution
→ Test Guardian
→ Final Engineering Audit
→ Delivery
```

## 3. 角色

### plan-reviewer

- 模型：DeepSeek V4 Pro。
- 只审查，不修改代码。
- 检查需求理解、遗漏约束、影响范围、方案简单性、复用、风险、测试和回退。
- 输出 `approved` 或 `revision_required`。

### implementation-guardian

- 模型：DeepSeek V4 Flash。
- 阶段性读取计划与 diff，不直接改代码。
- 检查偏离设计、重复造轮子、无必要依赖、接口破坏、可读性和修改范围失控。

### architecture-reviewer

- 模型：DeepSeek V4 Pro。
- 检查模块边界、职责、扩展方式、兼容性、技术债务和长期维护成本。

### simplification-reviewer

- 模型：DeepSeek V4 Flash；重大变更可升级 Pro。
- 专门寻找可删除代码、重复逻辑、过度抽象、无必要配置和更简单的实现。

### test-guardian

- Flash：检查是否存在有效测试、主要路径和失败路径是否覆盖。
- Pro：为高风险模块设计测试策略、边界与集成验证。
- 不接受“运行了少量表面测试”作为完成证据。

### final-engineering-audit

- 模型：DeepSeek V4 Pro。
- 只在 complex、大型修改或发布前执行。
- 从需求、计划、实现、测试、风险与文档全链路判断是否可交付。

## 4. 不同等级的治理强度

### engineering/standard

```text
Flash execution → focused check → relevant test
```

不启动完整流水线，但仍遵守全局工程策略和危险操作规则。

### engineering/deep

```text
Pro plan/execute → selected guardians → review → test
```

按风险选择 Plan Review、Architecture Review 或 Test Guardian。

### engineering/complex

启用完整治理流程。主 Agent 负责组织，Guardian 只审批和提出问题，实际修订由 Planner/Worker 完成。

### general

不使用代码、架构和测试专用治理。general/deep 可以做轻量事实、结构和质量复核；general/complex 可用通用多 Agent 编排。

## 5. 结构化审查输出

推荐所有 Guardian 使用统一格式：

```json
{
  "status": "approved | revision_required | blocked",
  "severity": "info | warning | critical",
  "findings": [],
  "required_actions": [],
  "optional_suggestions": [],
  "evidence": []
}
```

`required_actions` 未清零时，不进入下一阶段。
