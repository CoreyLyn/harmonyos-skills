# UI 组件参考

## 基础组件

### Text
```typescript
Text('Hello')
  .fontSize(16)
  .fontColor('#333333')
  .fontWeight(FontWeight.Medium)
```

### Image
```typescript
Image($r('app.media.icon'))
  .width(24)
  .height(24)
  .fillColor($r('sys.color.brand'))
```

### Button
```typescript
Button('Click')
  .onClick(() => {
    // 处理点击
  })
```

## 布局容器

### Column（垂直布局）
```typescript
Column() {
  Text('Title')
  Text('Content')
}
.alignItems(HorizontalAlign.Center)
.justifyContent(FlexAlign.Center)
```

### Row（水平布局）
```typescript
Row() {
  Text('Left')
  Blank()
  Text('Right')
}
.width('100%')
```

### Stack（堆叠布局）
```typescript
Stack({ alignContent: Alignment.BottomEnd }) {
  // 背景内容
  Column() {}

  // 前景内容
  Text('Overlay')
}
```

### Grid（网格布局）
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

### ForEach
```typescript
ForEach(this.items, (item: Item, index: number) => {
  ListItem() {
    Text(item.name)
  }
}, (item: Item) => item.id)
```

### LazyForEach（懒加载）
```typescript
LazyForEach(this.dataSource, (item) => {
  ListItem() {
    Text(item.name)
  }
})
```

## Tabs 组件

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

### CustomDialog
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

### 属性动画
```typescript
Text('Animate')
  .width(this.width)
  .animation({
    duration: 300,
    curve: Curve.EaseInOut
  })
```

### 转场动画
```typescript
Column()
  .transition(
    TransitionEffect.OPACITY
      .animation({ duration: 300 })
  )
```

### 共享元素转场
```typescript
// 源页面
this.uiContext.pushPath({ name: 'Detail' }, null);
```

## 样式系统

### 系统资源
```typescript
$r('sys.color.brand')           // 品牌色
$r('sys.color.ohos_id_color_text_primary')  // 主文本色
$r('sys.media.ohos_ic_public_ok') // 系统图标
```

### 主题模式
```typescript
.backgroundBlurStyle(BlurStyle.COMPONENT_THIN)
```

### 响应式布局
```typescript
.breakpoints({
  sm: { value: 600 },
  md: { value: 840 },
  lg: { value: 1280 }
})
```
