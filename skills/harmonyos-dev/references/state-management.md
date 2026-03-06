# 状态管理参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - 状态管理指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-management-overview
> - 状态变量装饰器: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-decorators
> - @ObservedV2/@Trace: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-observedV2-trace

## 响应式状态

### @ObservedV2 + @Trace（推荐）

来源: [状态变量装饰器](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-decorators)

```typescript
@ObservedV2
export class User {
  @Trace name: string = '';
  @Trace age: number = 0;
}

@ComponentV2
struct UserProfile {
  @Local user: User = new User();
}
```

### 组件内状态

来源: [状态管理概述](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-management-overview)

- `@Local` - 仅组件内部使用（官方文档规定）
- `@Param` - 从父组件传入（官方文档规定）
- `@Provider` - 向子组件提供（官方文档规定）

### 生命周期

来源: [组件生命周期](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-page-life-cycle)

- aboutToAppear() - 组件即将出现
- aboutToDisappear() - 组件即将销毁

## 状态传递

### 父子组件通信

来源: [状态同步](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-synchronization)

```typescript
// 父组件
@ComponentV2
struct Parent {
  @Provider data: string = 'value';

  build() {
    Child()
  }
}

// 子组件
@ComponentV2
struct Child {
  @Consumer data: string = '';
}
```

### 事件通信

来源: [Emitter使用指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-common-emitter)

```typescript
// 使用 emitter
import { emitter } from '@kit.BasicServicesKit';

// 发送
emitter.emit('eventName', { data: {} });

// 订阅
emitter.on('eventName', (data: emitter.EventData) => {
  // 处理事件
});
```

## 性能优化

来源: [状态管理最佳实践](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-management-best-practices)

- 使用 @Trace 精确追踪变化字段（推荐实践）
- 避免深层嵌套对象（推荐实践）
- 按需更新：仅标记必要的响应式字段（推荐实践）

## 常见陷阱与调试

### 陷阱1: Getter 不是响应式的

**问题**: UI 不会因 getter 返回值变化而重新渲染

```typescript
// ❌ 错误：getter 返回值变化不会触发 UI 更新
get isLargeScreen(): boolean {
  return AppStorage.get('currentBreakpoint') === 'lg';
}

// ✅ 正确：使用 @Trace 装饰属性，通过方法主动更新
@Trace isLargeScreen: boolean = false;

updateLargeScreen(breakpoint: string): void {
  this.isLargeScreen = breakpoint === BreakpointTypeEnum.LG;
}
```

**原因**: ArkUI 响应式系统基于属性变化检测，而非计算属性依赖追踪。必须用装饰器标记需要响应式的数据，并显式更新它们。

### 陷阱2: @ComponentV2 不支持 @StorageLink

**问题**: `@StorageLink` 只能用于 `@Component`，不能用于 `@ComponentV2`

```typescript
// ❌ 编译错误
@ComponentV2
struct MyView {
  @StorageLink('key') value: string = '';  // 报错
}

// ✅ 替代方案：手动获取或通过 onAreaChange 等事件触发更新
@ComponentV2
struct MyView {
  @Local value: string = '';

  aboutToAppear() {
    this.value = AppStorage.get<string>('key') ?? '';
  }
}
```

**原因**: V2 装饰器体系与 V1 不完全兼容，查阅文档确认每个装饰器的适用范围。

### 调试检查清单

当发现"数据变了但 UI 没变"时，按顺序检查：

| 检查项 | 说明 |
|-------|------|
| 装饰器 | 属性是否有 `@Trace`/`@Local`？ |
| 更新方式 | 是直接赋值还是 getter 计算？ |
| 初始化 | 监听器是否已注册？ |
| 生命周期 | 是否在正确时机获取初始值？ |

### 架构分层建议

将"监听变化"与"响应变化"分离，通过 AppStorage 作为中间层解耦：

```
监听层 (MediaQuery / BreakpointSystem)
      ↓ AppStorage.setOrCreate()
状态层 (ViewModel) ← updateXxx() 方法
      ↓ @Trace 属性
UI层 (View) ← onAreaChange() 等事件触发检查
```

**经验**: 当数据源与 UI 之间没有直接的响应式绑定时，需要通过事件回调主动拉取最新状态。