---
paths:
  - "**/views/**/use*.ts"
  - "**/views/**/use*.tsx"
---

## 1. 文件命名与位置

与组件同目录放置 hook 文件，以 `use{Feature}.ts` 命名，Feature 用 PascalCase。

```
components/
  RagManagement/
    index.tsx
    useRagManagement.ts      ← 同目录
  RagSelectDrawer/
    index.tsx
    useRagSelect.ts           ← 同目录
```

- 正例：`RagManagement/useRagManagement.ts`、`RagSelectDrawer/useRagSelect.ts`、`MemberManagement/useMemberManagement.ts`

## 2. 命名约定

### useRequest 解构命名

使用固定的解构命名模式：

```typescript
// 数据 — 语义化变量名 + 空数组默认值
const { data: ragList = [] } = useRequest(...)

// 执行函数 — run + 动作描述
const { runAsync: runQueryRagList } = useRequest(...)
const { runAsync: runDeleteRag } = useRequest(...)

// 刷新函数 — refresh + 动作描述
const { refreshAsync: refreshQueryRagList } = useRequest(...)

// 加载状态 — 动作描述 + Loading 后缀
const { loading: queryRagListLoading } = useRequest(...)
const { loading: addMemberLoading } = useRequest(...)
```

- 正例：`useRagManagement.ts:43-48`（`data: ragList = []`、`runAsync: runQueryRagList`、`loading: queryRagListLoading`）
- 正例：`useMemberManagement.ts:84-88`（`runAsync: queryMemberList`、`refreshAsync: refreshMemberList`）

### 暴露给组件的 handler

handler 用 `on` 前缀命名，用于包装 useRequest 的 run/runAsync 并附加副作用（message、刷新等）：

```typescript
// onXxx 包装 runXxx，加入 message.success + 刷新逻辑
const onDeleteRag = (_id?: number) => {
  return runDeleteRag({ id: _id }).then((res) => {
    if (res?.success) {
      refreshQueryRagList();
      message.success('移除成功');
    }
  });
};
```

- 正例：`useRagManagement.ts:70-82`（`onDeleteRag`、`onAddRag`）
- 正例：`useRagManagement.ts:53-55`（`onSearchRagList`）

## 3. Loading 聚合

多个 loading 状态用 `||` 组合，按语义分类命名：

```typescript
return {
  loading: queryRagListLoading || judgeRagUseLoading,         // 页面级
  submitLoading: updateMemberLoading || addMemberLoading,     // 提交类
  optsLoading: queryRoleOptsLoading || queryMemberOptsLoading, // 选项加载
};
```

- 正例：`useRagManagement.ts:120`（`loading: queryRagListLoading || judgeRagUseLoading`）
- 正例：`useMemberManagement.ts:140-142`（`loading`、`optsLoading`、`submitLoading` 三类聚合）

## 4. 返回值结构

返回扁平对象，不嵌套。分三类字段：

1. **数据**：`xxxList`（带 `= []` 默认值）
2. **Handler**：`onXxx`（包装后的操作方法）或直接暴露 `runAsync`（无副作用的查询）
3. **Loading**：聚合后的加载状态

```typescript
return {
  // 数据
  ragList,
  memberList,
  roleOptList,
  // Handler
  onAddRag,
  onDeleteRag,
  onSearchRagList,
  onJudgeRagUse: runJudgeRagUse,  // 无副作用直接透传 runAsync
  // Loading
  loading: queryRagListLoading || judgeRagUseLoading,
};
```

- 正例：`useRagManagement.ts:118-126`
- 正例：`useRagSelect.ts:44`（精简版：数据 + handler + loading 三字段）
- 正例：`useMemberManagement.ts:136-152`（完整版：多数据 + 多 handler + 多聚合 loading）
