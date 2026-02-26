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