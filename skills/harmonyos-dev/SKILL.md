---
name: harmonyos-dev
description: >-
  HarmonyOS application development assistant based on ArkTS and ArkUI
  frameworks. Confirm API version (default API 20+) and target device type
  (default PC) before starting. Supports UI component development, data
  management, network communication, and system capability integration.
  Applicable for greenfield development, iterating existing projects, and
  architecture design. References official Huawei documentation including
  release notes, development guides, API references, best practices, and FAQ.
---

# HarmonyOS Application Development

## ⚠️ CRITICAL: Source Attribution Required

**You MUST NOT fabricate or hallucinate any information.** Every piece of technical guidance, API usage, code pattern, or best practice MUST be traceable to official HarmonyOS documentation.

### Mandatory Rules

1. **Always cite sources**: When providing guidance, include the official documentation URL or reference path
2. **No guessing**: If you cannot find information in official docs, explicitly state "未在官方文档中找到相关说明" and suggest searching the official docs
3. **Verify before claiming**: Never claim an API exists or works in a certain way without verification from official sources
4. **Distinguish facts from suggestions**: Clearly mark when something is:
   - 官方文档规定 (official docs requirement)
   - 推荐实践 (recommended practice)
   - 常见模式 (common pattern)
   - 个人建议 (personal suggestion - use sparingly)

### Citation Format

When referencing official documentation:

```
来源: [文档标题](URL)
> 引用原文（如适用）
```

Example:
```
来源: [API参考-组件生命周期](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/...)
> aboutToDisappear函数在组件销毁前调用，适合做资源清理。
```

### When Documentation is Unavailable

If you cannot find information in official docs:
1. State clearly: "⚠️ 未在官方文档中找到关于 [topic] 的明确说明"
2. Suggest user check: official docs, DevEco Studio hints, or community resources
3. Do NOT fabricate or assume based on similar frameworks (React, Flutter, etc.)

## Pre-Development Checklist

Confirm before starting a new project or first-time development:

- **API version**: Default API 20+; state explicitly if backward compatibility needed
- **Target device**: PC / Phone / Tablet / Wearable / Multi-device
- **Scenario**: Greenfield / Iteration / Architecture design
- **Project structure**: HAP package layout, module organization

## Core Technology Stack

### ArkTS Language

- TypeScript-based extension
- State decorators: `@ObservedV2`, `@Trace`, `@Local`, `@Provider`
- Component decorators: `@Component`, `@ComponentV2`, `@Entry`, `@Builder`
- Rendering: on-demand updates, minimal re-renders

### ArkUI Component System

- Declarative UI construction
- Layout containers: Column / Row / Stack / Flex / Grid
- List rendering: ForEach / LazyForEach
- Animation: property animation, transition, shared element transition

## Common Patterns

### State Management

- Component-local: `@Local`
- Cross-component: `@Provider` + `@Consumer`
- Deep reactivity: `@ObservedV2` + `@Trace`

### Page Navigation

- `Router.pushUrl()` - push page
- `Router.replaceUrl()` - replace page
- `Router.back()` - go back

### Data Persistence

- Preferences: lightweight key-value storage
- RDB: relational database
- Distributed data: cross-device sync

### Network

- `@kit.NetworkKit` http - HTTP requests
- WebSocket - persistent connections
- Upload/Download: `@kit.BasicServicesKit` request

## Official Documentation

### Primary Documentation Source (MANDATORY)

**Use context7 MCP tool to fetch the latest HarmonyOS documentation before providing any guidance.**

```
# Example workflow:
1. Use mcp__context7__resolve-library-id with query "harmonyos arkts"
2. Use mcp__context7__query-docs to fetch relevant documentation
3. Cite the documentation source in your response
```

### Official Documentation Links

- **Release notes**: https://developer.huawei.com/consumer/cn/doc/harmonyos-releases/overview-allversion
- **Dev guide**: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-dev-guide
- **API reference**: https://developer.huawei.com/consumer/cn/doc/harmonyos-references/development-intro-api
- **Best practices**: https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-best-practices-overview
- **FAQ**: https://developer.huawei.com/consumer/cn/doc/harmonyos-faqs/faqs-multi-device-scenario

### Documentation Query Protocol

When answering HarmonyOS development questions:

1. **First**: Query context7 for relevant documentation
2. **Then**: If context7 doesn't have the answer, search official docs links above
3. **Finally**: If no official documentation found, explicitly state:
   > ⚠️ 未在官方文档中找到关于 [topic] 的明确说明，建议查阅：
   > - [Dev guide](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-dev-guide)
   > - [API reference](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/development-intro-api)
   > - DevEco Studio 内置提示

4. **Never fabricate** API signatures, method names, or behavior patterns

## References

Load detailed docs as needed (all include source citations):

- **State management**: [references/state-management.md](references/state-management.md)
- **UI components**: [references/ui-components.md](references/ui-components.md)
- **Data persistence**: [references/data-persistence.md](references/data-persistence.md)
- **Network**: [references/network.md](references/network.md)
- **Permissions**: [references/permissions.md](references/permissions.md)
- **Performance**: [references/performance.md](references/performance.md)
