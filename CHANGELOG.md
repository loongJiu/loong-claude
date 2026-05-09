## [1.3.1] - 2026-05-09

### 修改
- `brainstorming` 技能：工作流开始时增加 skill 声明语句，明确标识当前使用的技能
- README：移除已迁移至全局的 MCP 配置描述（doraemon-docs、sketch-design）

## [1.3.0] - 2026-05-09

### 修改
- 重构 `brainstorming` 和 `writing-plans` 技能文档，精简冗余描述、提升结构与清晰度
  - `brainstorming`：description 改为结构化 YAML 格式，明确触发/不触发条件；正文精炼为 8 步标准工作流 + 架构规范 + dot 流程图
  - `writing-plans`：将 7 个独立章节合并为 5 个紧凑模块（范围定义、文档规范、禁飞区、自审、交接），消除重复表述

### 删除
- 移除 `.mcp.json` 中 doraemon-docs 和 sketch-design 的本地 MCP 配置（已迁移至全局）

### 文档
- `RELEASING.md` 补充 tag 推送命令示例

## [1.2.0] - 2026-05-08

### 新增
- 添加 `commit` 技能：代码实现完成后按需触发提交，支持单次/拆分提交策略
- `brainstorming` 技能新增 yapi 项目检测：自动识别 `ytt.config.ts` 并标记需遵循 `yapi-guide` 规则

### 修改
- `brainstorming` 技能：重写 description 触发词，提高自动触发命中率
- `brainstorming` 技能：更新 Checklist 第 8 步，串联完整工作流（brainstorming → writing-plans → 编码 → commit）
- `writing-plans` 技能：执行交接环节补充 commit skill 引用，明确实施过程中不自行 commit

## [1.1.0] - 2026-05-08

### 新增
- 添加 brainstorming 技能：在实施前通过协作对话将想法完善为设计方案
- 添加 writing-plans 技能：根据技术规范或需求编写详尽的实施计划

## [1.0.1] - 2026-05-08

### 文档
- 更新插件安装方式为 marketplace 两步安装
- 添加发布指南文档 (RELEASING.md)

### 删除
- 删除 hello skill

## [1.0.0] - 2026-05-08


### 新增
- 初始化个人 Claude 配置仓库
- 添加 Claude Code 插件配置和技能文档
