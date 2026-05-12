<div align="center">

# loong-claude

面向个人工作流的 Claude Code 插件配置集合

<br />

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](.claude-plugin/plugin.json)

</div>

<br />

`loong-claude` 用于沉淀常用编码流程、统一项目规则，并通过 Skills、Hooks 与 MCP 提升日常研发效率。

## 亮点

| 能力 | 说明 |
| --- | --- |
| 技能（Skills） | 封装需求澄清、实施计划、调试、提交和 YApi 接口使用规范。 |
| 钩子（Hooks） | 在文件编辑后自动执行格式化，减少重复的手动操作。 |
| 规则（Rules） | 沉淀 React Hook 编写规范，可按需加载到全局或项目级上下文。 |
| MCP | 预置文档检索与设计稿相关服务配置。 |
| 插件市场安装 | 便于复用、更新和分发。 |

## 安装

在 Claude Code 中添加插件市场并安装插件：

```bash
/plugin marketplace add loongJiu/loong-claude
/plugin install loong-claude@loong-marketplace
```

安装完成后重启 Claude Code 使配置生效。

## 包含内容

### 技能

| 技能 | 用途 |
| --- | --- |
| `/loong-claude:brainstorming` | 将新需求转化为可确认的设计方案，避免直接进入实现导致返工。 |
| `/loong-claude:writing-plans` | 将明确需求拆解为可执行、可验证的实施计划。 |
| `/loong-claude:debugging` | 通过观察、推理、验证的 ReAct 流程系统性定位问题。 |
| `/loong-claude:commit` | 根据变更内容生成规范提交信息并执行提交流程。 |
| `/loong-claude:yapi-guide` | 指导使用 `@zcy/yapi-to-typescript` 生成的接口、类型和请求函数。 |

### 钩子

| 钩子 | 触发时机 | 行为 |
| --- | --- | --- |
| `PostToolUse` | `Write` / `Edit` 后 | 当目标文件为 `.ts` 或 `.tsx` 时，自动执行 `npx prettier --write`。 |

### MCP

| 服务 | 说明 |
| --- | --- |
| `doraemon-docs` | 文档检索相关 MCP 服务。 |
| `sketch-design` | 设计稿相关 MCP 服务。 |

### 规则

| 规则 | 说明 |
| --- | --- |
| `rules/react-hook.md` | React Hook 文件组织、命名、loading 聚合、返回值结构等编码规范。 |

> 注意：受 Claude Code 插件机制限制，`rules/` 目录不会随插件自动加载。需要手动安装或在项目中显式引用。

<details>
<summary><strong>规则安装方式</strong></summary>

### 全局安装

将插件中的规则复制到 Claude Code 全局规则目录：

```bash
cp -r ~/.claude/plugins/cache/loong-claude/*/rules/* ~/.claude/rules/
```

如果只需要某个规则，也可以单独复制：

```bash
cp ~/.claude/plugins/cache/loong-claude/*/rules/react-hook.md ~/.claude/rules/
```

验证方式：在 Claude Code 中运行 `/memory`，确认目标规则文件已出现在加载列表中。

### 项目级引用

如果只希望规则在特定项目生效，可以在项目的 `CLAUDE.md` 中按需引用：

```markdown
## 代码风格

@~/.claude/rules/react-hook.md
```

### 更新规则

插件更新后，如需同步最新规则，重新执行复制命令：

```bash
cp -r ~/.claude/plugins/cache/loong-claude/*/rules/* ~/.claude/rules/
```

</details>

<details>
<summary><strong>仓库结构</strong></summary>

```text
.
├── .claude-plugin/       # Claude Code 插件元数据和市场配置
├── hook/                 # Claude Code 钩子
├── rules/                # 可复用的编码规则
├── skills/               # Claude Code 技能
├── .mcp.json             # MCP 服务配置
├── CHANGELOG.md
├── LICENSE
└── README.md
```

</details>

## 许可协议

[MIT](LICENSE)
