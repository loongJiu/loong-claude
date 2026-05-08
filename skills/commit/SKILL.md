---
name: commit
description: |
  当用户要求提交代码、commit、提交改动时触发。
  典型触发表达：'提交代码'、'commit'、'帮我提交'、'提交一下'、'可以commit了'、'准备好了，提交吧'。
  前提：用户已自行审阅过 diff，此 skill 只负责高效执行提交。
---

# 代码提交

用户已确认变更内容，直接执行提交。

## 流程

### 1. 收集变更信息

```bash
git status
git diff --stat
git diff --cached --stat
git log --oneline -5
```

### 2. 拆分提交策略

根据变更内容决定单次或多次提交：

**单次提交**：单一功能、文件少于 5 个、变更逻辑紧密关联。

**多次提交**：涉及多个独立功能、重构与新功能混合、配置与业务代码混合。

### 3. 编写 commit message

格式：`<type>: <简短描述>`

type 规范：
- `feat`: 新功能
- `fix`: 修复 bug
- `refactor`: 重构
- `style`: 样式调整
- `docs`: 文档
- `chore`: 构建/工具/配置
- `test`: 测试

要求：
- 用中文编写
- 描述"做了什么"而非"怎么做的"
- 不要引用 skill 名称或计划文件路径

### 4. 执行提交

按分组依次 `git add <具体文件>` + `git commit`。

**禁止**：
- `git add -A` / `git add .`
- `--no-verify` 跳过 hooks
- `--amend` 修改已有提交
- 提交 `.env`、`credentials` 等敏感文件
- 未经用户同意执行 `git push`
- 使用 force push
