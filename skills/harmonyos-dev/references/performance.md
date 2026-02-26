# 性能优化参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - 性能优化指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/performance
> - 渲染性能: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-performance
> - 最佳实践: https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-best-practices-overview

## 渲染优化

来源: [渲染性能优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-performance)

### 减少重渲染

来源: [状态管理优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-state-management-performance)

```typescript
// 使用 @Local 替代 @Trace 避免不必要的更新
@ComponentV2
struct MyComponent {
  @Local value: string = '';  // 仅组件内使用

  // 仅追踪需要响应的字段
  @Trace关键时刻: string = '';
}
```

### 列表懒加载

来源: [LazyForEach性能优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-control-lazyforeach)

```typescript
// 使用 LazyForEach 替代 ForEach
LazyForEach(this.dataSource, (item) => {
  ListItem() {
    ItemView({ item: item })
  }
}, (item: Item) => item.id)
```

### 条件渲染优化

来源: [条件渲染](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-control-if)

```typescript
// 使用 if 替代 visibility（推荐实践）
if (this.showContent) {
  Content()
}

// 避免使用（不推荐）
Content().visibility(this.showContent ? Visibility.Visible : Visibility.None)
```

## 内存优化

来源: [内存管理](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-memory-management)

### 及时释放资源

来源: [组件生命周期](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-page-life-cycle)

```typescript
aboutToDisappear() {
  // 取消事件订阅
  this.eventHub.off('event', this.handler);

  // 关闭数据库连接
  await this.rdbStore.close();

  // 释放控制器
  this.dialogController = null;
}
```

### 图片资源优化

来源: [图片处理](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-graphics-image)

```typescript
// 使用合适的图片格式
Image($r('app.media.icon'))
  .width(24)
  .height(24)
  .renderMode(ImageRenderMode.Template)  // 模板模式，支持着色
  .interpolation(ImageInterpolation.High)  // 高质量插值
```

### 数据懒加载

来源: [数据懒加载](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-data-lazy-loading)

```typescript
class LazyDataSource {
  private loaded: boolean = false;
  private data: any[] = [];

  async getData(): Promise<any[]> {
    if (!this.loaded) {
      this.data = await this.loadData();
      this.loaded = true;
    }
    return this.data;
  }
}
```

## 网络优化

来源: [网络性能优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/network-performance)

### 请求缓存

```typescript
class ApiCache {
  private cache: Map<string, any> = new Map();

  async get(url: string): Promise<any> {
    if (this.cache.has(url)) {
      return this.cache.get(url);
    }

    let response = await http.request(url);
    this.cache.set(url, response.data);
    return response.data;
  }
}
```

### 请求合并

```typescript
class RequestBatcher {
  private queue: Map<string, Promise<any>> = new Map();

  async request(key: string, fn: () => Promise<any>): Promise<any> {
    if (this.queue.has(key)) {
      return this.queue.get(key);
    }

    let promise = fn().finally(() => {
      this.queue.delete(key);
    });

    this.queue.set(key, promise);
    return promise;
  }
}
```

## 启动优化

来源: [应用启动优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/application-startup-optimization)

### 延迟初始化

```typescript
@ComponentV2
struct MyComponent {
  private service?: MyService;

  aboutToAppear() {
    // 延迟初始化非关键服务（推荐实践）
    setTimeout(() => {
      this.service = new MyService();
    }, 100);
  }
}
```

### 首屏优化

来源: [首屏渲染优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-first-screen-optimization)

```typescript
// 分批加载数据（推荐实践）
async loadInitialData() {
  // 先加载关键数据
  await this.loadCriticalData();

  // 再加载次要数据
  setTimeout(() => {
    this.loadSecondaryData();
  }, 500);
}
```

## 动画优化

来源: [动画性能优化](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-animation-performance)

### 使用属性动画

来源: [属性动画](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-property-animation)

```typescript
// 推荐使用属性动画而非转场动画
Text('Animate')
  .width(this.width)
  .height(this.height)
  .animation({
    duration: 300,
    curve: Curve.EaseInOut
  })
```

### 避免复杂动画

来源: [动画最佳实践](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-animation-best-practices)

```typescript
// 避免同时动画多个属性（不推荐）
// 推荐：只动画必要属性
Text('Animate')
  .opacity(this.opacity)  // 仅动画透明度
```

## 性能分析

来源: [性能分析工具](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-performance-profiling)

### DevEco Studio Profiler

来源: [Profiler使用指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/deveco-profiler)

- CPU Profiler：分析 CPU 使用情况
- Memory Profiler：分析内存分配和泄漏
- Network Profiler：分析网络请求

### 性能指标

来源: [性能指标标准](https://developer.huawei.com/consumer/cn/doc/best-practices/bpta-performance-metrics)

- 首屏渲染时间 < 1s（推荐指标）
- 页面切换延迟 < 300ms（推荐指标）
- 滚动帧率 > 55fps（推荐指标）
- 内存增长 < 50MB/hour（推荐指标）