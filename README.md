# Agent Release Governance Skill

一个用于 **AI Agent 双仓发布治理** 的 Codex / Agent Skill。

它适合管理这类项目：

```text
Full 私有仓库
  ↓ 通过 manifest / release script 导出
User 用户版仓库
```

也就是说，完整版本里可以保留内部治理、QA、eval、审计、交接、发布工具；用户版只保留真正需要交付给使用者的运行能力和最小配置说明。

## 这个 Skill 解决什么问题

当一个 Agent 从“自己能跑”变成“要交给别人稳定使用”时，常见问题会变成：

- 哪些文件能进入用户版？
- 哪些治理文档、测试脚本、demo 资产应该只留在 full 私仓？
- 发布前要跑哪些检查？
- 用户版是不是 full 的受控子集？
- GitHub issue 哪些可以关闭，哪些只是治理总线？
- 前端接后端有没有破坏真实 workflow？
- 是否有敏感信息泄露到公开仓库？

这个 Skill 提供一套固定流程，帮助 Agent 在发布前做边界检查、隐私扫描、QA 回归、issue 治理和双仓同步。

## 适用场景

- 你有一个完整 Agent 项目，但只想公开或交付其中一部分。
- 你希望 full 仓库是唯一开发源，user 仓库由脚本导出。
- 你需要在发布前检查隐私、测试、构建和用户版一致性。
- 你需要区分“用户版 bug”和“内部治理/维护事项”。
- 你希望把 Agent 产品化交付，而不是靠手工复制文件。

## 推荐仓库结构

```text
agent-full/
├── user-release-manifest.yaml
├── scripts/
│   ├── release_user.py
│   ├── privacy_scan.py
│   └── check_user_release.py
├── tests/
│   ├── run_tests.py
│   └── test_issue_regressions.py
└── ...

agent-user/
├── SKILL.md
├── config.example.yaml
├── scripts/
├── references/
└── frontend/  # 如果用户版需要前端
```

核心原则：

```text
full/main = 唯一开发源
user/main = 自动导出的交付物
```

不要把 `main` 当 full、某个分支当 user 来做权限边界。分支适合开发隔离，仓库更适合交付和可见性隔离。

## 使用方式

把本仓库作为 Codex / Agent Skill 安装到你的 skills 目录，例如：

```bash
~/.agents/skills/agent-release-governance
```

之后可以对 Agent 说：

```text
使用 Agent Release Governance，检查这个 Agent 的 full/user 发布边界。
```

或：

```text
帮我从 full 仓库导出用户版，跑发布前 QA，并告诉我哪些 issue 可以关闭。
```

## Skill 内容

```text
agent-release-governance/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── release-governance.md
```

`SKILL.md` 放核心流程，`references/release-governance.md` 放详细检查清单，包括：

- full / user 边界
- 发布门禁
- 五链路 QA
- issue 治理规则
- GitHub 双账号推送
- 已安装 skill 同步与锁定
- 隐私扫描重点
- release note 结构

## 发布前检查建议

一次标准发布至少应覆盖：

1. full / user 两边 `git status`
2. 隐私扫描
3. 业务回归测试
4. issue regression / QA tests
5. 前端 build 或 typecheck
6. manifest 驱动的用户版导出
7. 用户版一致性检查
8. open issue 收口和说明

构建通过不等于生产验证通过。构建只能证明包能编译，真实 workflow 仍需要业务验收。

## 许可证

当前仓库仅包含通用 Skill 文档和流程，不包含任何业务数据、密钥或私有配置。
