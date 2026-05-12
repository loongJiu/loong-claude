## [1.4.1] - 2026-05-12

### 修改
- `brainstorming` 技能：步骤 1 退出条件从单一判断升级为 5 项清单（代码位置、模块职责、隐含契约、使用路径等）
- `brainstorming` 技能：新增步骤 6「设计反证验证」，包含现状契约表、职责迁移表、关键场景矩阵、反例审查和受影响范围清单，确保设计在提交审核前经自检推翻
- `brainstorming` 技能：流程图新增「设计反证验证」节点，反例发现时回流至「沉淀设计文档」
- `brainstorming` 技能：文档自检步骤补充确认设计反证验证已写入设计文档的要求

## [1.4.0] - 2026-05-12

### 新增
- `debugging` 技能：系统性调试工作流，采用 ReAct 循环（观察→推理→行动）定位根因，包含四个阶段：收集症状、ReAct 调试循环、最小侵入修复、防复发小结

### 修改
- `brainstorming` 技能：步骤 1（探索项目上下文）改写为 ReAct 探索循环，避免一次性读完所有文件，每轮探索后更新"当前认知"直至能回答需求落地点；步骤 2（明确需求）增加自适应提问原则，根据上一个回答动态决定下一个问题
- `writing-plans` 技能：步骤 4（自审清单）升级为两轮 Reflexion 自审——第 1 轮「全新开发者」视角逐任务检查路径/代码/验证/上下文/占位符，第 2 轮「执行者」视角检查规范覆盖率/任务顺序/类型一致性/测试可运行性/边界情况
- `commit` 技能：新增步骤 4（Commit message 自检），写完 message 后以"3 个月后的我能立刻知道做了什么、为什么吗？"为标准自审，不通过则重写

## [1.3.3] - 2026-05-12

### 新增
- 恢复 `.mcp.json` 中 `sketch-design` 本地 MCP 配置

## [1.3.2] - 2026-05-11

### 新增
- 添加 `.gitignore`，排除 `.claude/` 目录
- 恢复 `.mcp.json` 中 doraemon-docs 本地 MCP 配置

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
