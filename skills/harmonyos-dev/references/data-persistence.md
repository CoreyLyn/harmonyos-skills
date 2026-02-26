# 数据持久化参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - 数据管理指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/data-management
> - Preferences: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-preferences
> - 关系型数据库: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store
> - 分布式数据: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-distributed-data-object

## Preferences（轻量级存储）

来源: [Preferences用户首选项](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-preferences)

### 基础用法

```typescript
import { preferences } from '@kit.ArkData';

// 获取 Preferences 实例
let dataPreferences = await preferences.getPreferences(getContext(), 'mystore');

// 存储数据
await dataPreferences.put('key', 'value');
await dataPreferences.flush();

// 读取数据
let value = await dataPreferences.get('key', 'defaultValue');

// 删除数据
await dataPreferences.delete('key');
await dataPreferences.flush();
```

### 数据监听

来源: [Preferences数据监听](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-preferences#监听数据变化)

```typescript
dataPreferences.on('change', (key) => {
  console.log(`Key ${key} changed`);
});
```

## 关系型数据库（RDB）

来源: [关系型数据库RDB](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store)

### 初始化数据库

```typescript
import { relationalStore } from '@kit.ArkData';

const STORE_CONFIG: relationalStore.StoreConfig = {
  name: 'RdbStore.db',
  securityLevel: relationalStore.SecurityLevel.S1
};

let store = await relationalStore.getRdbStore(getContext(), STORE_CONFIG);
```

### 创建表

来源: [RDB建表](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store#创建数据库表)

```typescript
await store.executeSql(`
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    age INTEGER
  )
`);
```

### 插入数据

来源: [RDB插入](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store#插入数据)

```typescript
let valueBucket: relationalStore.ValuesBucket = {
  'name': 'John',
  'age': 30
};
await store.insert('users', valueBucket);
```

### 查询数据

来源: [RDB查询](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store#查询数据)

```typescript
let predicates = new relationalStore.RdbPredicates('users');
predicates.equalTo('name', 'John');

let resultSet = await store.query(predicates);
while (resultSet.goToNextRow()) {
  let id = resultSet.getLong(resultSet.getColumnIndex('id'));
  let name = resultSet.getString(resultSet.getColumnIndex('name'));
}
resultSet.close();
```

### 更新数据

来源: [RDB更新](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store#更新数据)

```typescript
let valueBucket: relationalStore.ValuesBucket = {
  'age': 31
};
let predicates = new relationalStore.RdbPredicates('users');
predicates.equalTo('name', 'John');
await store.update(valueBucket, predicates);
```

### 删除数据

来源: [RDB删除](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-relational-store#删除数据)

```typescript
let predicates = new relationalStore.RdbPredicates('users');
predicates.equalTo('name', 'John');
await store.delete(predicates);
```

## 分布式数据

来源: [分布式数据对象](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-distributed-data-object)

### 分布式数据对象

```typescript
import { distributedDataObject } from '@kit.ArkData';

let obj: distributedDataObject.DataObject = {
  name: 'John',
  age: 30
};

let localObject = await distributedDataObject.create(getContext(), obj);

// 监听数据变化
localObject.on('change', (sessionId, fields) => {
  console.log('Data changed:', fields);
});

// 加入分布式网络
let sessionId = await localObject.setSessionId();
```

## 文件存储

来源: [文件管理](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-file-management)

### 文件操作

来源: [文件基础操作](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-file-io)

```typescript
import { fileIo } from '@kit.CoreFileKit';

// 写入文件
let file = await fileIo.open(filePath, fileIo.OpenMode.CREATE | fileIo.OpenMode.WRITE_ONLY);
await fileIo.write(file.fd, content);
await fileIo.close(file.fd);

// 读取文件
file = await fileIo.open(filePath, fileIo.OpenMode.READ_ONLY);
let arrayBuffer = new ArrayBuffer(1024);
await fileIo.read(file.fd, arrayBuffer);
await fileIo.close(file.fd);
```

### 目录操作

```typescript
import { fileIo } from '@kit.CoreFileKit';

// 创建目录
await fileIo.mkdir(dirPath);

// 列出目录
let files = await fileIo.listFile(dirPath);

// 删除文件
await fileIo.unlink(filePath);
```