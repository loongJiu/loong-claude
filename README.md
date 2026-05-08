# loong-claude

个人 Claude Code 配置库，包含代码风格规范、自动化 hooks 和 MCP 工具集成。

## 安装

```bash
/plugin install https://github.com/loongJiu/loong-claude
```

重启 Claude Code 后生效。

## 包含内容

### Skills

技能会在相关任务时自动触发，也可手动调用：

| 技能 | 触发场景 |
|---|---|
| `/loong-claude:yapi-guide` | 指导使用yapi接口 |

### Hooks

- **PostToolUse**：编辑 `.ts` / `.tsx` 文件后自动运行 Prettier 格式化

### MCP

- **doraemon-docs MCP**：提供 Doraemon 文档查询能力
- **sketch-design MCP**：提供 Sketch 设计稿识别能力

### Rules

代码风格规则文件存放在 `rules/` 目录，**不会随 plugin 自动加载**（Claude Code 的 plugin 机制限制）。

如需全局生效，参考下方手动安装步骤。

---

## Rules 手动安装（可选）

Plugin 安装完成后，执行以下命令将规则复制到全局目录：

```bash
# 复制全部规则
cp -r ~/.claude/plugins/cache/loong-claude/*/rules/* ~/.claude/rules/

# 或只复制你需要的
cp ~/.claude/plugins/cache/loong-claude/*/rules/hooks-reusable.md ~/.claude/rules/
cp ~/.claude/plugins/cache/loong-claude/*/rules/hooks-data.md ~/.claude/rules/
```

验证是否生效：在 Claude Code 中运行 `/memory`，确认对应文件出现在加载列表中。

### 项目级引用（推荐）

如果只想在特定项目生效，在项目 `CLAUDE.md` 里按需引用：

```markdown
## 代码风格
@~/.claude/rules/hooks-reusable.md
@~/.claude/rules/hooks-data.md
```

### 更新 rules

Plugin 更新后规则文件也会更新，需要重新执行复制命令同步最新内容：

```bash
cp -r ~/.claude/plugins/cache/loong-claude/*/rules/* ~/.claude/rules/
```

## 本地开发

```bash
git clone https://github.com/loongJiu/loong-claude
cd loong-claude

# 本地测试，不需要安装
claude --plugin-dir .

# 修改文件后热更新
/reload-plugins
```

## License

MIT