---
name: yapi-guide
description: |
  项目使用 @zcy/yapi-to-typescript 管理 API 请求。当用户需要调用接口、查找接口定义、使用生成的请求函数、处理接口返回数据、添加新接口、理解 API 层架构、查看接口类型定义、或在任何涉及"接口"、"请求""API""yapi""ajax"的上下文中,都应该使用此 skill 提供指导。即使用户没有明确提到 yapi,只要涉及本项目中 src/api/ 下的接口调用,也应参考此 skill。
---

# Yapi 接口管理指南

本 skill 指导你如何在项目中正确使用由 yapi 自动生成的接口代码。

## 核心架构概览

```
ytt.config.ts (配置)
    ↓ 代码生成
src/api/{moduleName}.ts (自动生成的接口函数 + 类型)
    ↓ 运行时调用
src/ajax/index.ts (统一请求入口,错误处理)
    ↓ 底层
src/ajax/request.ts (axios 封装,拦截器)
```

## 1. Yapi 配置 (ytt.config.ts)

配置文件 `ytt.config.ts` 定义了代码生成规则：

- **Server**: `http://yapi.cai-inc.com/`
- **工具**: `@zcy/yapi-to-typescript`
- **Project 映射**:
  - `8889` → `src/api/rag.ts`
  - `8934` → `src/api/intelligence.ts`
- **函数命名规则**: `{camelCase(path)}_{method}` (例如 `apiBizTaggingTaggingSystemRagChunkListPost`)
- **输出文件**: 根据 project_id 自动路由到 `src/api/{projectName}.ts`
- **请求函数入口**: `src/ajax/index.ts`

新增项目时需要：
1. 在 `projectNameMap` 中添加 `{projectId}: 'moduleName'` 映射
2. 在 `projects` 数组中添加对应 token 的项目配置
3. 运行 ytt 代码生成命令

## 2. Ajax 请求层 (src/ajax/)

### request.ts — 底层 axios 封装
- GET 请求自动附加 `timestamp` 参数防缓存
- 响应拦截器自动解包 `response.data`（axios 拦截器中 `return response.data`）
- 401 自动跳转 SSO 登录（开发环境提示手动设置 cookie token）
- `validateStatus`: 200-399 视为成功
- `withCredentials: true`（携带 cookie）
- `maxRedirects: 0`（不自动跟随重定向）
- URL 前缀通过 `window.apiPreFix` 全局变量控制

### index.ts — 统一请求入口（不要直接修改）
这是 yapi 生成代码的请求入口文件，核心逻辑：

1. **GET 请求处理**: 将 `data` 移到 `params`，清空 `data`
2. **响应结构**: 统一为 `{ success, result, code, message }` 格式
3. **错误处理**:
   - `success === false` 时自动弹出 `message.error()` 提示
   - 网络异常时也自动弹出错误提示
   - 错误时 **仍然 resolve（而非 reject）**，即即使 `success: false` 也会走 `.then`

**重要**: 不要直接修改 `src/ajax/` 下的文件。如果需要自定义错误处理，在调用处处理。

## 3. 一个接口生成的东西

每个 yapi 接口会在 `src/api/` 中生成以下内容：

### 类型
```typescript
// 请求参数类型（interface，字段均为 optional）
export interface ApiBizTaggingTaggingSystemRagChunkListPostRequest {
  datasetId?: string;
  contentKeyword?: string;
}

// 返回值类型（interface，嵌套结构对应后端 JSON）
export interface ApiBizTaggingTaggingSystemRagChunkListPostResponse {
  success?: boolean;
  result?: {
    total?: number;
    data?: { chunkId?: string; content?: string }[];
  };
  code?: string;
  message?: string;
}
```
请求和返回值类型具体包含字段不确定，以上只是一个示例，具体请参考对应接口的文档。

### 请求函数
```typescript
// 导出的是 const 函数，直接调用即可
export const apiBizTaggingTaggingSystemRagChunkListPost = /*#__PURE__*/ (
  requestData: ApiBizTaggingTaggingSystemRagChunkListPostRequest,
  ...args: UserRequestRestArgs
) => {
  return request<ApiBizTaggingTaggingSystemRagChunkListPostResponse>(
    prepare(config, requestData),
    ...args,
  );
};
```

### 命名规律
函数名 = `api` + 路径的 PascalCase + `Get`/`Post`
- `/api/biz-tagging/tagging-system/rag/chunk/list` + POST → `apiBizTaggingTaggingSystemRagChunkListPost`
- `/api/biz-tagging/tagging-system/rag/dataset/detail` + GET → `apiBizTaggingTaggingSystemRagDatasetDetailGet`

## 4. 只使用类型（不调用接口时）

生成的 `Request` 和 `Response` 类型可以直接用 `type` 关键字复用：

```typescript
// 用 type 引用嵌套类型作为 Props 等
type ChunkItem = NonNullable<
  ApiBizTaggingTaggingSystemRagChunkListPostResponse['result']
>['data'][number];

// 或直接使用整个响应类型
type DatasetListResult = ApiBizTaggingTaggingSystemRagDatasetListPostResponse;
```

也可以用 `NonNullable<>` + 索引访问来提取嵌套类型中的必填字段。

## 5. 实际开发如何使用

### 基本调用模式
```typescript
import { apiBizTaggingTaggingSystemRagDatasetListPost } from '@/api/rag';

// 简单调用（注意：错误已被 ajax/index.ts 自动处理并弹提示）
const res = await apiBizTaggingTaggingSystemRagDatasetListPost({
  name: '测试',
  pageNum: 1,
  pageSize: 10,
});
// res 类型为 ApiBizTaggingTaggingSystemRagDatasetListPostResponse
// 数据在 res.result 中
const list = res?.result?.data || [];
```

### 配合 useRequest 使用（推荐模式）
```typescript
import { useRequest } from 'ahooks';
import { apiBizAiAiPlatformDashboardApiApplicationQueryToolListPost } from '@/api/intelligence';

const { data, loading, refresh } = useRequest(async () => {
  const res = await apiBizAiAiPlatformDashboardApiApplicationQueryToolListPost({
    spaceId,
    appId,
  });
  return res?.data || [];
});
```

### 配合 useMemo 派生数据
```typescript
const toolPackages = useMemo(() => {
  return (data || []).filter((item) => item.locator === 'COMPONENT_TOOL_GROUP');
}, [data]);
```

### 文件组织位置
- **接口函数 + 类型**: `src/api/{moduleName}.ts`（自动生成，不要手动修改）
- **业务 hooks**: `src/pages/{feature}/hooks/useXxx.ts` 或 `src/pages/{feature}/components/Xxx/useXxx.ts`
- **组件**: 对应的组件目录下

## 6. 实用技巧

### 快速定位接口
如果知道接口路径（如 `/api/xxx/yyy`），可以通过函数命名规则推算函数名：`api` + PascalCase(path) + Method。

如果不确定，直接在 `src/api/rag.ts` 或 `src/api/intelligence.ts` 中搜索关键字。

### 请求参数都是 optional
所有 Request 类型的字段都是 `?:`（可选），这是因为 yapi 生成策略。实际使用时根据业务需要传参。

### 返回值结构
`success: false` 时 ajax 层已自动弹了错误提示，但仍然会 resolve 而非 reject。

### GET vs POST
- GET 请求：参数通过 query string 发送（代码自动处理，开发者无需关心）
- POST 请求：参数通过 request body 发送（JSON）
- 部分 GET 请求的参数名会出现在 `queryNames` 配置中（自动处理）
