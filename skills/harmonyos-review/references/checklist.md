# HarmonyOS Code Review Checklist

## 0. Version & Compatibility (Official Docs)

### API Version
- [ ] Target API version is current/recommended (API 20+ for HarmonyOS 6.0.0)
- [ ] DevEco Studio version compatible with target API
- [ ] No use of deprecated APIs without migration plan
- [ ] API version checks using `canIUse()` for conditional features

### System Capabilities
- [ ] `canIUse()` checks for optional system features
- [ ] Fallback behavior when capability unavailable
- [ ] No assumption of device capabilities

### Application Model
- [ ] Stage model preferred over FA model
- [ ] Proper component lifecycle for chosen model
- [ ] Correct context usage patterns

---

## 1. Security

### Hardcoded Credentials
- [ ] Check `build-profile.json5` for hardcoded signing passwords
- [ ] Search for hardcoded API keys, tokens, secrets
- [ ] Verify no sensitive data in version control

### Database Encryption
- [ ] Check `relationalStore.StoreConfig` for `encrypt: true`
- [ ] Verify appropriate `securityLevel` setting

### Input Validation
- [ ] All user inputs validated
- [ ] Error messages don't leak sensitive data

---

## 2. ArkTS Language

### Error Handling (Official API Patterns)
- [ ] Sync APIs: try-catch for exceptions
- [ ] Async APIs with await: try-catch for exceptions and rejections
- [ ] Async APIs with Promise: catch() or onRejected for rejections
- [ ] BusinessError objects properly handled
- [ ] Error codes checked and acted upon

### Logging
- [ ] Uses `hilog` instead of `console`
- [ ] Logging level appropriate for production

### Type Safety
- [ ] Minimal use of `as` type assertions
- [ ] Proper use of optional chaining vs assertions

### Code Quality
- [ ] No magic numbers (define constants with comments)
- [ ] Functions under 50 lines when possible
- [ ] Files under 800 lines

---

## 3. Component Lifecycle

### Resource Cleanup
- [ ] `WebviewController.release()` in `aboutToDisappear`
- [ ] `setInterval` cleared in `onDestroy` or `aboutToDisappear`
- [ ] Event listeners properly unsubscribed

### Event Subscriptions
- [ ] `eventHub.on()` matched with `eventHub.off()`
- [ ] Callback references consistent (use bound functions)

---

## 4. State Management

### Decorator Versions
- [ ] Consistent use of V2 decorators (`@Local`, `@Param`, `@Provider`)
- [ ] Avoid V1/V2 mixing (`@State`, `@Prop` mixed with V2)

### Async State
- [ ] No race conditions in state updates
- [ ] Proper locking for concurrent operations

---

## 5. Database

### ResultSet Handling
- [ ] All `ResultSet` objects closed in all code paths
- [ ] Use try-finally to ensure cleanup

### Transactions
- [ ] Errors in transactions are propagated
- [ ] Proper `commit()` / `rollBack()` handling

### Versioning
- [ ] `onUpgrade` / `onDowngrade` callbacks implemented

---

## 6. Permissions

### Request Timing
- [ ] Check permission status before requesting
- [ ] Use `checkAccessToken` before `requestPermissionsFromUser`

### Declarations
- [ ] All required permissions in `module.json5`
- [ ] `ohos.permission.INTERNET` if using network

---

## 7. Performance

### Async Patterns
- [ ] No `forEach` with `await` (use `for...of` or `Promise.all`)
- [ ] Large file operations use async APIs

### Error Handling
- [ ] File operation errors propagated to caller
- [ ] No silent failures in catch blocks

---

## 8. Kit Usage (Official Patterns)

### Kit Imports
- [ ] Appropriate Kit imports (no unused dependencies)
- [ ] Correct import patterns per Kit documentation
- [ ] Mini service APIs properly marked if applicable
- [ ] Widget/card capability checked when used in ArkTS cards

### Kit-Specific Patterns
- [ ] Ability Kit: Proper lifecycle management
- [ ] ArkUI: Correct component usage patterns
- [ ] Network Kit: Proper async/await patterns
- [ ] Universal Keystore Kit: Secure key handling

---

## 9. Code Quality

### Testing
- [ ] Unit tests for core logic
- [ ] 80%+ coverage target

### Duplication
- [ ] Extract repeated logic into shared functions

### Structure
- [ ] Component nesting under 4 levels
- [ ] Related code grouped in same module
