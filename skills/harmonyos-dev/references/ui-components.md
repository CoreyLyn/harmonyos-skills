# UI 组件参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - ArkUI组件参考: https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-overview
> - 开发指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkui-ts-components
> - 布局容器: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-layout-development

## 基础组件

来源: [基础组件](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-basic)

### Text

来源: [Text组件](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-basic-text)

```typescript
Text('Hello')
  .fontSize(16)
  .fontColor('#333333')
  .fontWeight(FontWeight.Medium)
```

### Image

来源: [Image组件](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-basic-image)

```typescript
Image($r('app.media.icon'))
  .width(24)
  .height(24)
  .fillColor($r('sys.color.brand'))
```

### Button

来源: [Button组件](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-basic-button)

```typescript
Button('Click')
  .onClick(() => {
    // 处理点击
  })
```

## 布局容器

来源: [布局开发指导](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-layout-development)

### Column（垂直布局）

来源: [Column容器](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-container-column)

```typescript
Column() {
  Text('Title')
  Text('Content')
}
.alignItems(HorizontalAlign.Center)
.justifyContent(FlexAlign.Center)
```

### Row（水平布局）

来源: [Row容器](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-container-row)

```typescript
Row() {
  Text('Left')
  Blank()
  Text('Right')
}
.width('100%')
```

### Stack（堆叠布局）

来源: [Stack容器](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-container-stack)

```typescript
Stack({ alignContent: Alignment.BottomEnd }) {
  // 背景内容
  Column() {}

  // 前景内容
  Text('Overlay')
}
```

### Grid（网格布局）

来源: [Grid容器](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-container-grid)

```typescript
Grid() {
  ForEach(this.data, (item) => {
    GridItem() {
      Text(item)
    }
  })
}
.columnsTemplate('1fr 1fr')
.rowsTemplate('1fr 1fr')
```

## 列表渲染

来源: [列表开发指导](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-control)

### ForEach

来源: [ForEach使用](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-control-foreach)

```typescript
ForEach(this.items, (item: Item, index: number) => {
  ListItem() {
    Text(item.name)
  }
}, (item: Item) => item.id)
```

### LazyForEach（懒加载）

来源: [LazyForEach使用](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-rendering-control-lazyforeach)

```typescript
LazyForEach(this.dataSource, (item) => {
  ListItem() {
    Text(item.name)
  }
})
```

## Tabs 组件

来源: [Tabs组件](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-container-tabs)

```typescript
Tabs({ index: this.currentIndex, controller: this.tabsController }) {
  TabContent() {
    Content1()
  }
  .tabBar('Tab 1')

  TabContent() {
    Content2()
  }
  .tabBar('Tab 2')
}
.barPosition(BarPosition.Start)
.vertical(true)
```

## 对话框

来源: [弹窗开发指导](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-popup-and-menu)

### CustomDialog

来源: [CustomDialog](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-customdialog)

```typescript
private dialogController: CustomDialogController = new CustomDialogController({
  builder: CustomDialogExample({
    confirm: () => {
      // 确认回调
    }
  }),
  autoCancel: true,
  alignment: DialogAlignment.Center,
  customStyle: true
});

// 显示
this.dialogController?.open();

// 关闭
this.dialogController?.close();
```

### AlertDialog

来源: [AlertDialog](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/arkui-ts-components-alertdialog)

```typescript
AlertDialog.show({
  title: '提示',
  message: '确定要删除吗？',
  primaryButton: {
    value: '取消',
    action: () => {}
  },
  secondaryButton: {
    value: '确定',
    action: () => {}
  }
})
```

## 动画

来源: [动画开发指导](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-animation)

### 属性动画

来源: [属性动画](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-property-animation)

```typescript
Text('Animate')
  .width(this.width)
  .animation({
    duration: 300,
    curve: Curve.EaseInOut
  })
```

### 转场动画

来源: [转场动画](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-transition-animation)

```typescript
Column()
  .transition(
    TransitionEffect.OPACITY
      .animation({ duration: 300 })
  )
```

### 共享元素转场

来源: [共享元素转场](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-shared-element-transition)

```typescript
// 源页面
this.uiContext.pushPath({ name: 'Detail' }, null);
```

## 样式系统

来源: [资源使用指导](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-resource)

### 系统资源

来源: [系统资源](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-system-resource)

```typescript
$r('sys.color.brand')           // 品牌色
$r('sys.color.ohos_id_color_text_primary')  // 主文本色
$r('sys.media.ohos_ic_public_ok') // 系统图标
```

### 主题模式

来源: [主题设置](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-theme)

```typescript
.backgroundBlurStyle(BlurStyle.COMPONENT_THIN)
```

### 响应式布局

来源: [响应式布局](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-responsive-layout)

```typescript
.breakpoints({
  sm: { value: 600 },
  md: { value: 840 },
  lg: { value: 1280 }
})
```