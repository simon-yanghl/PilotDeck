# 07. Fork、升级与部署

## 1. Git 结构

```text
upstream/main  官方主线
origin/main    用户 Fork 的同步主线
origin/custom  实际部署分支
feature/*      独立功能分支
```

推荐启用：

```bash
git config --global rerere.enabled true
```

## 2. 开发原则

- 每个功能先在独立 feature 分支实施和测试。
- 尽量通过现有 extension、adapter、hook、middleware 或小型模块实现。
- 避免大面积修改路由核心；必须修改时记录原因和升级风险。
- Guardian 和测试通过后合并到 custom。

## 3. 官方更新

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

git checkout custom
git merge main
# 解决冲突、重新构建、完整测试
git push origin custom
```

生产部署不直接跟随 upstream/main，而使用经过验证的 custom。

## 4. 从当前官方部署切换

当前配置位于 `~/.pilotdeck/`，正常情况下与源码目录分离。切换前必须：

1. 备份 `~/.pilotdeck/` 与 systemd service。
2. 为当前 VM 创建 PVE 快照。
3. 拉取 Fork 的 custom 分支。
4. 安装依赖、构建、运行测试。
5. 在临时端口或测试实例验证配置兼容性。
6. 停止旧服务、切换目录或分支、启动新服务。
7. 检查日志、路由、视觉、Workspace、Memory 和 UI 状态。
8. 保留快速回退方式。

## 5. 兼容性

自定义配置必须保持旧字段可用。新增字段应有默认值，未启用时行为尽量与官方一致。配置迁移必须可检测、可报告，不能静默丢弃未知字段。
