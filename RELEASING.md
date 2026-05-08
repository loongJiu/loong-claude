# 发布指南

本文档说明如何发布 loong-claude 的新版本，包括版本号管理、git tag、GitHub Release 和用户更新。

---

## 版本号规范

遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)：`MAJOR.MINOR.PATCH`

| 类型 | 场景 | 示例 |
|---|---|---|
| `PATCH` | 修复 bug、调整 skill 描述措辞、小优化 | `1.0.0` → `1.0.1` |
| `MINOR` | 新增 skill、新增 hook、新增 MCP 配置 | `1.0.1` → `1.1.0` |
| `MAJOR` | 删除或重命名已有 skill、破坏性变更 | `1.1.0` → `2.0.0` |

> **重要**：只有修改 `plugin.json` 里的 `version` 字段，用户才会收到更新提示。
> 如果不改版本号，每次 commit 都会被视为新版本（基于 commit SHA），用户无法感知到"这是一次正式更新"。

---

## 发布流程

### 第一步：更新内容

完成代码修改后，确认改动：

```bash
git diff
git status
```

### 第二步：更新 CHANGELOG.md

在 `CHANGELOG.md` 顶部新增本次变更记录：

```markdown
## [1.1.0] - 2026-05-10

### 新增
- 新增 `react-components` skill，自动触发 React 组件编写规范

### 修改
- `hooks-reusable` skill：补充 useEffect 清理函数要求

### 修复
- 修复 hook 在 Windows 路径下格式化失败的问题
```

### 第三步：更新 plugin.json 版本号

```json
// .claude-plugin/plugin.json
{
  "name": "loong-claude",
  "version": "1.1.0",   // ← 修改这里
  ...
}
```

### 第四步：提交改动

```bash
git add .
git commit -m "chore: release v1.1.0"
```

### 第五步：打 git tag

**推荐方式：使用 `claude plugin tag`（会自动做版本校验）**

```bash
claude plugin tag
```

该命令会读取 `plugin.json` 里的版本号，验证格式后自动创建符合 Claude Code 规范的 tag。

**手动方式：**

```bash
# tag 格式：plugin名称--v版本号（双横线）
git tag loong-claude--v1.1.0
```

> tag 格式为 `{plugin-name}--v{semver}`，双横线是 Claude Code 解析 tag 的约定格式。

### 第六步：推送到 GitHub

```bash
# 推送 commit 和 tag
git push origin master

# 或一次推送所有 tag
git push origin master --tags
```

### 第七步：创建 GitHub Release

```bash
# 使用 GitHub CLI（推荐）
gh release create loong-claude--v1.1.0 \
  --title "v1.1.0" \
  --notes "$(sed -n '/## \[1.1.0\]/,/## \[/p' CHANGELOG.md | head -n -1)"
```

或者在 GitHub 网页操作：
1. 进入仓库 → Releases → Draft a new release
2. Tag 选择刚推送的 `loong-claude--v1.1.0`
3. Title 填写 `v1.1.0`
4. Description 粘贴 CHANGELOG 对应段落
5. 点击 Publish release

---

## 用户侧更新

发布新版本后，用户在 Claude Code 中执行：

```bash
/plugin update loong-claude
```

或更新所有已安装的 plugin：

```bash
/plugin update
```

---

## 快速发布 Checklist

```
□ 改动已测试（claude --plugin-dir . 本地验证）
□ CHANGELOG.md 已更新
□ plugin.json version 字段已更新
□ git commit 已提交
□ git tag 已创建（格式：loong-claude--vX.Y.Z）
□ tag 已 push 到 GitHub
□ GitHub Release 已创建
```

---

## 版本策略说明

**设置 `version` 字段（当前方式）**

用户只在你显式 bump 版本号时收到更新。适合稳定发布，用户不会因为你的小改动而被强制更新。

**不设置 `version` 字段**

每次 commit 都视为新版本（用 commit SHA 标识）。适合频繁迭代阶段，但用户体验较差——他们无法判断每次更新的重要性。

---

## 紧急回滚

如果新版本有问题，让用户回退到上一个版本：

```bash
# 查看历史 tag
git tag --list "loong-claude--v*" | sort -V

# 用户侧：安装指定版本
/plugin install loong-claude--v1.0.0@your-marketplace
```

或直接把 `plugin.json` 里的版本号改回旧版本，重新走发布流程。
