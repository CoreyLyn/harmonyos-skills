# HarmonyOS Skills

用于 HarmonyOS 应用开发与代码审查。基于华为官方开发指南，支持 ArkTS 语言和 ArkUI 框架。

## 概述

本项目提供两个专用技能，覆盖 HarmonyOS 应用开发全生命周期：

| 技能 | 用途 | 适用场景 |
|------|------|----------|
| `harmonyos-dev` | 应用开发助手 | 从零开发、项目迭代、架构设计 |
| `harmonyos-review` | 代码审查 | 代码审计、安全检查、质量评估 |

## 技能说明

### harmonyos-dev

HarmonyOS 应用开发助手，基于 ArkTS 和 ArkUI 框架。

**核心能力：**
- UI 组件开发（声明式 UI、布局系统、动画）
- 数据管理（状态管理、数据持久化）
- 网络通信（HTTP 请求、WebSocket、文件上传）
- 系统能力集成（权限管理、设备能力）

**使用方式：**
```
/harmonyos-dev
```

**技术栈：**
- ArkTS 语言（基于 TypeScript 扩展）
- ArkUI 组件系统（声明式 UI）
- API 20+ 版本（向下兼容需说明）

### harmonyos-review

HarmonyOS 代码审查技能，基于华为官方开发指南和安全最佳实践。

**审查维度：**
- 安全合规（硬编码凭证、加密、输入验证）
- ArkTS 语言规范（hilog 使用、类型安全）
- 组件生命周期（资源清理、事件订阅）
- 状态管理（装饰器版本一致性）
- 数据库操作（ResultSet 处理、事务管理）
- 权限管理（官方权限模式）
- 性能问题（异步模式、资源泄漏）

**使用方式：**
```
/harmonyos-review
```

**输出：** 生成 Markdown 格式的审查报告，包含优先级修复建议。

## 项目结构

```
harmonyos-skills/
├── skills/
│   ├── harmonyos-dev/           # 开发技能
│   │   ├── SKILL.md             # 技能主文件
│   │   └── references/          # 参考文档
│   │       ├── state-management.md
│   │       ├── ui-components.md
│   │       ├── data-persistence.md
│   │       ├── network.md
│   │       ├── permissions.md
│   │       └── performance.md
│   │
│   └── harmonyos-review/         # 审查技能
│       ├── SKILL.md             # 技能主文件
│       └── references/          # 参考文档
│           ├── checklist.md
│           ├── official-docs.md
│           └── report-template.md
└── README.md
```

## 快速开始

### 1. 安装

```bash
npx skills add coreylyn/harmonyos-skills
```

### 2. 使用开发技能

```
/harmonyos-dev
```

确认以下信息：
- API 版本（默认 API 20+）
- 目标设备（PC 端/手机/平板/穿戴设备）
- 开发场景（新建/迭代/架构设计）

### 3. 使用审查技能

```
/harmonyos-review
```

审查流程：
1. 快速扫描（检测关键问题）
2. 深度分析（逐项检查）
3. 生成报告（Markdown 格式）

## 官方文档

- [版本说明](https://developer.huawei.com/consumer/cn/doc/harmonyos-releases/overview-allversion)
- [开发指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-dev-guide)
- [API 参考](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/development-intro-api)
- [最佳实践](https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-best-practices-overview)
- [FAQ](https://developer.huawei.com/consumer/cn/doc/harmonyos-faqs/faqs-multi-device-scenario)

## 许可证

MIT License
