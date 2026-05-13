<div align="center">

# loong-claude

面向个人工作流的 Claude Code 插件配置集合

<br />

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](.claude-plugin/plugin.json)
[![Version](https://img.shields.io/badge/version-1.5.0-orange)](.claude-plugin/plugin.json)

</div>

<br />

`loong-claude` 沉淀编码全流程规范，通过 Skills、Hooks 与 MCP 将日常研发中的重复决策自动化——从需求澄清到代码提交，让每个环节都有章可循。

## 工作流全景

```
新需求 ──→ brainstorming ──→ writing-plans ──→ 编码 ──→ code-review ──→ commit
              需求澄清          实施计划                      质量审查       提交

遇到问题 ──→ debugging（ReAct 循环定位根因，最小侵入修复）

调用接口 ──→ yapi-guide（接口架构、类型、请求函数使用规范）
```

## 安装

```bash
/plugin marketplace add loongJiu/loong-claude
/plugin install loong-claude@loong-marketplace
```

安装后重启 Claude Code 生效。更新时执行 `/plugin update loong-claude`。

## 包含内容

### 技能（Skills）

| 技能 | 触发方式 | 核心能力 |
| --- | --- | --- |
| `brainstorming` | "帮我做"、"新增"、"实现" | 复杂度路由 + 协作对话，将想法转化为经反证验证的设计方案，批准前禁止编码 |
| `writing-plans` | 具备技术规范后进入 | 将规范拆解为微型任务（Bite-sized），含两轮 Reflexion 自审，假设执行者对代码库完全不了解 |
| `code-review` | "review代码"、"代码审查" | 结构化审查 + 严重等级分类（Critical / Major / Minor / Suggestion），只产出报告，不经授权不修改代码 |
| `debugging` | "报错了"、"有 bug" | ReAct 循环（观察→推理→行动）系统性定位根因，确认根因前绝不修改代码 |
| `commit` | "提交代码"、"commit" | 智能拆分提交策略 + scope 规范（从分支名推断）+ message 自检 |
| `yapi-guide` | 涉及"接口"、"请求"、"API" | `@zcy/yapi-to-typescript` 生成的接口函数、类型定义与请求调用规范 |

### 钩子（Hooks）

| 事件 | 匹配工具 | 行为 |
| --- | --- | --- |
| `PostToolUse` | `Write` / `Edit` | 目标文件为 `.ts` / `.tsx` 时，自动执行 `prettier --write` 格式化 |

### MCP 服务

| 服务 | 用途 |
| --- | --- |
| `doraemon-docs` | `@zcy/doraemon` 组件库文档检索（支持按名称查询、关键词搜索、组件列表） |
| `sketch-design` | Sketch 设计稿解析（场景图、蓝图、交互图、实现验证），配合 `implement-from-design` skill 使用 |

### 规则（Rules）

| 规则 | 范围 |
| --- | --- |
| `react-hook.md` | `**/views/**/use*.ts(x)` — Hook 文件组织、`useRequest` 解构命名、loading 聚合、返回值结构规范 |

> 受插件机制限制，`rules/` 不会随插件自动加载，需手动安装。

<details>
<summary><strong>规则安装方式</strong></summary>

**全局安装**（所有项目生效）：

```bash
cp -r ~/.claude/plugins/cache/loong-claude/*/rules/* ~/.claude/rules/
```

**项目级引用**（在 `CLAUDE.md` 中添加）：

```markdown
@~/.claude/rules/react-hook.md
```

插件更新后，重新执行复制命令同步最新规则。

</details>

<details>
<summary><strong>仓库结构</strong></summary>

```text
.
├── .claude-plugin/
│   ├── plugin.json            # 插件元数据（名称、版本、作者）
│   └── marketplace.json       # 插件市场配置
├── hook/
│   └── hooks.json             # PostToolUse 自动格式化
├── rules/
│   └── react-hook.md          # React Hook 编码规范
├── skills/
│   ├── brainstorming/SKILL.md # 需求澄清与设计
│   ├── code-review/SKILL.md   # 代码质量审查
│   ├── commit/SKILL.md        # 代码提交流程
│   ├── debugging/SKILL.md     # 系统性调试
│   ├── writing-plans/SKILL.md # 实施计划编写
│   └── yapi-guide/SKILL.md   # YApi 接口规范
├── .mcp.json                  # MCP 服务配置
├── CHANGELOG.md               # 版本变更记录
├── RELEASING.md               # 发布指南
├── LICENSE
└── README.md
```

</details>

## 许可协议

[MIT](LICENSE)
