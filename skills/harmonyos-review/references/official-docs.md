# HarmonyOS Official Documentation References

## Official Documentation Links

### Version Releases
**URL**: https://developer.huawei.com/consumer/cn/doc/harmonyos-releases/overview-allversion

Current Recommended Version:
- HarmonyOS 6.0.0 (API 20) - Recommended for new and existing applications
- Released: 2025/09/25
- DevEco Studio 6.0.0 Release

Release Types:
- **Canary**: Early experience, features may be unstable
- **Beta**: Public beta, features stabilizing
- **Release**: Official release with quality commitment

### Application Development Guide
**URL**: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-dev-guide

Key Development Areas (Kit-based):
- **Application Framework**: Ability Kit, ArkUI
- **System**: Universal Keystore Kit, Network Kit
- **Media**: Audio Kit, Media Library Kit
- **Graphics**: ArkGraphics 2D, Graphics Accelerate Kit
- **Application Services**: Game Service Kit, Location Kit
- **AI**: Intents Kit, CANN Kit

### API Reference
**URL**: https://developer.huawei.com/consumer/cn/doc/harmonyos-references/development-intro-api

Key Concepts:
- **Version Support**: API version标注 (e.g., "8+" means API 8+)
- **SystemCapability (SysCap)**: Check device capabilities with `canIUse()`
- **Mini Service API**: Marked as "__元服务API__" for applicable interfaces
- **Widget/Card Capability**: Marked as "__卡片能力__" for ArkTS card UI
- **Permissions**: Marked as "__需要权限__: ohos.permission.xxxx"
- **Error Codes**: Use try-catch for sync, try-catch + catch()/onRejected for async
- **Deprecation**: Marked with "deprecated", 5 API levels compatibility
- **Application Models**: FA model vs Stage model

### Best Practices
**URL**: https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-best-practices-overview

### FAQ - Multi-Device Scenarios
**URL**: https://developer.huawei.com/consumer/cn/doc/harmonyos-faqs/faqs-multi-device-scenario

## Review Guidelines Based on Official Docs

### Version Compatibility
- [ ] Target API version is current/recommended (API 20+)
- [ ] DevEco Studio version compatible with target API
- [ ] No use of deprecated APIs without migration plan
- [ ] Proper API version checks using `canIUse()` for feature detection

### Kit Usage Best Practices
- [ ] Appropriate Kit imports (no unused dependencies)
- [ ] Correct API usage per Kit documentation
- [ ] Mini service APIs properly marked if applicable
- [ ] Widget/card capability checked when used in cards

### Error Handling
- [ ] Sync APIs: try-catch for exceptions
- [ ] Async APIs: try-catch + Promise.catch() or onRejected
- [ ] BusinessError objects properly handled
- [ ] Error codes checked and handled appropriately

### Permissions
- [ ] Required permissions declared in module.json5
- [ ] Permission status checked before requesting
- [ ] Graceful handling when permissions denied
- [ ] No unnecessary permission requests

### System Capabilities
- [ ] SysCap checks for optional features using `canIUse()`
- [ ] Fallback behavior when capability unavailable
- [ ] Custom syscap configured if needed

### Application Model
- [ ] Stage model preferred over FA model (current standard)
- [ ] Proper component lifecycle (Page, Ability, UIAbility)
- [ ] Correct context usage patterns
