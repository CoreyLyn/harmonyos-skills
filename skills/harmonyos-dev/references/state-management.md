# 状态管理参考

## 响应式状态

### @ObservedV2 + @Trace（推荐）
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
- `@Local` - 仅组件内部使用
- `@Param` - 从父组件传入
- `@Provider` - 向子组件提供

### 生命周期
- aboutToAppear() - 组件即将出现
- aboutToDisappear() - 组件即将销毁

## 状态传递

### 父子组件通信
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

- 使用 @Trace 精确追踪变化字段
- 避免深层嵌套对象
- 按需更新：仅标记必要的响应式字段
