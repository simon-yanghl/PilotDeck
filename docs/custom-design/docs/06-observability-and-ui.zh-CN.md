# 06. 可观测性与 UI

## 1. 用户可见模型状态

在当前任务显著位置显示：

- 实际使用模型
- domain
- tier
- resolvedFrom：judge、manual、fallback、vision-preprocess 等
- 简短选择原因
- 是否正在编排或审计

示例：

```text
Model: DeepSeek V4 Pro
Route: engineering/deep
Source: judge
Reason: architecture decision with high correctness requirements
```

## 2. Fallback 警示

Judge 输出非法、超时或模型错误时，应明显显示 fallback，而不是静默切换：

```text
Route fallback: engineering/deep
Cause: invalid judge output
```

## 3. 路由日志

每轮记录：

```json
{
  "timestamp": "...",
  "workspace": "...",
  "input_type": "text | image",
  "domain": "engineering",
  "tier": "deep",
  "model": "deepseek/deepseek-v4-pro",
  "resolved_from": "judge",
  "reason": "...",
  "judge_raw": "...",
  "usage": {
    "input_tokens": 0,
    "output_tokens": 0,
    "cache_read_tokens": 0
  }
}
```

## 4. 长期保留

Router、模型调用、Tool、Guardian、测试、错误和危险操作日志长期保留，由人工整理。需要提供归档状态或标签，而不是自动清理。

## 5. 人工覆盖

V1 需支持人工指定模型或路线，具体语法在实现阶段确定。候选方式：

- `/model ...`
- `@pro`、`@vision`
- UI 下拉或当前会话锁定

覆盖结果必须在 UI 和日志中明确标记为 `manual`。
