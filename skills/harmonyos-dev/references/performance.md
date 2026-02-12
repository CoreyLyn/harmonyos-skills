# 性能优化参考

## 渲染优化

### 减少重渲染
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
```typescript
// 使用 LazyForEach 替代 ForEach
LazyForEach(this.dataSource, (item) => {
  ListItem() {
    ItemView({ item: item })
  }
}, (item: Item) => item.id)
```

### 条件渲染优化
```typescript
// 使用 if 替代 visibility
if (this.showContent) {
  Content()
}

// 避免使用
Content().visibility(this.showContent ? Visibility.Visible : Visibility.None)
```

## 内存优化

### 及时释放资源
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
```typescript
// 使用合适的图片格式
Image($r('app.media.icon'))
  .width(24)
  .height(24)
  .renderMode(ImageRenderMode.Template)  // 模板模式，支持着色
  .interpolation(ImageInterpolation.High)  // 高质量插值
```

### 数据懒加载
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

### 延迟初始化
```typescript
@ComponentV2
struct MyComponent {
  private service?: MyService;

  aboutToAppear() {
    // 延迟初始化非关键服务
    setTimeout(() => {
      this.service = new MyService();
    }, 100);
  }
}
```

### 首屏优化
```typescript
// 分批加载数据
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

### 使用属性动画
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
```typescript
// 避免同时动画多个属性
// 推荐：只动画必要属性
Text('Animate')
  .opacity(this.opacity)  // 仅动画透明度
```

## 性能分析

### DevEco Studio Profiler
- CPU Profiler：分析 CPU 使用情况
- Memory Profiler：分析内存分配和泄漏
- Network Profiler：分析网络请求

### 性能指标
- 首屏渲染时间 < 1s
- 页面切换延迟 < 300ms
- 滚动帧率 > 55fps
- 内存增长 < 50MB/hour
