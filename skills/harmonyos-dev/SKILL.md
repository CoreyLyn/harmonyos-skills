---
name: harmonyos-dev
description: HarmonyOS 应用开发助手，基于 ArkTS 和 ArkUI 框架。使用时需先确认 API 版本（默认 API 20+）和目标设备类型（默认 PC 端）。支持 UI 组件开发、数据管理、网络通信、系统能力集成等场景。适用于从零开始开发、迭代现有项目和架构设计。参考华为官方文档进行开发：版本说明、开发指南、API 参考、最佳实践和 FAQ。
---

# HarmonyOS 应用开发

## 开发前确认

首次开发或新项目启动时，确认以下信息：

- **API 版本**：默认 API 20+，向下兼容需明确说明
- **目标设备**：PC 端/手机/平板/穿戴设备/全场景
- **开发场景**：新建/迭代/架构设计
- **项目结构**：HAP 包结构、模块划分方式

## 核心技术栈

### ArkTS 语言特性
- 基于 TypeScript 扩展
- 状态管理：`@ObservedV2`、`@Trace`、`@Local`、`@Provider`
- 组件装饰器：`@Component`、`@ComponentV2`、`@Entry`、`@Builder`
- 性能优化：按需更新、最小化重渲染

### ArkUI 组件系统
- 声明式 UI 构建
- 布局容器：Column/Row/Stack/Flex/Grid
- 列表渲染：ForEach/LazyForEach
- 动画系统：属性动画、转场动画、共享元素转场

### 项目结构模式
```
products/default/     # 主产品入口
commons/utils/        # 通用工具模块
commons/database/     # 数据库模块
commons/logic/        # 业务逻辑模块
features/             # 功能特性模块
  ├── history/        # 历史工程
  ├── make/           # 文件制作
  ├── viewer/         # 文件查看
  ├── sign/           # 签章
  └── generate/       # 生成
```

## 常用开发模式

### 状态管理
- 组件内状态：`@Local`
- 跨组件传递：`@Provider` + `@Consumer`
- 深度响应式：`@ObservedV2` + `@Trace`

### 页面导航
- Router.pushUrl() - 页面跳转
- Router.replaceUrl() - 替换页面
- Router.back() - 返回上一页

### 数据持久化
- Preferences：轻量级键值存储
- RDB：关系型数据库
- 分布式数据：跨设备同步

### 网络请求
- @ohos/axios.http - HTTP 请求
- WebSocket - 长连接
- 上传下载：@ohos.request

## 参考 API 文档

**版本说明**：https://developer.huawei.com/consumer/cn/doc/harmonyos-releases/overview-allversion

**开发指南**：https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-dev-guide

**API 参考**：https://developer.huawei.com/consumer/cn/doc/harmonyos-references/development-intro-api

**最佳实践**：https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-best-practices-overview

**FAQ**：https://developer.huawei.com/consumer/cn/doc/harmonyos-faqs/faqs-multi-device-scenario

## 深入参考

按需加载详细文档：

- **状态管理**：[references/state-management.md](references/state-management.md)
- **UI 组件**：[references/ui-components.md](references/ui-components.md)
- **数据存储**：[references/data-persistence.md](references/data-persistence.md)
- **网络通信**：[references/network.md](references/network.md)
- **权限管理**：[references/permissions.md](references/permissions.md)
- **性能优化**：[references/performance.md](references/performance.md)
