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
