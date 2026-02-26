# 网络通信参考

> **来源要求**: 本文档内容需对照官方文档验证。使用时请注明出处。
>
> 官方文档：
> - 网络管理指南: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/network-management
> - HTTP请求: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-http-request
> - WebSocket: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-websocket
> - Socket: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-socket

## HTTP 请求

来源: [HTTP网络请求](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-http-request)

### 基础请求

```typescript
import { http } from '@kit.NetworkKit';

let request = http.createHttp();

let response = await request.request('https://api.example.com/data', {
  method: http.RequestMethod.GET,
  header: {
    'Content-Type': 'application/json'
  },
  connectTimeout: 60000,
  readTimeout: 60000
});

if (response.responseType === http.ResponseType.STRING) {
  let data = JSON.parse(response.result as string);
}
```

### POST 请求

来源: [HTTP POST请求](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-http-request#发起post请求)

```typescript
let response = await request.request('https://api.example.com/data', {
  method: http.RequestMethod.POST,
  header: {
    'Content-Type': 'application/json'
  },
  extraData: JSON.stringify({
    key1: 'value1',
    key2: 'value2'
  })
});
```

### 文件上传

来源: [上传下载](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-upload-download)

```typescript
import { request } from '@kit.BasicServicesKit';

let uploadTask = await request.uploadFile(getContext(), {
  url: 'https://example.com/upload',
  method: 'POST',
  files: [
    {
      filename: 'test.txt',
      name: 'file',
      uri: 'file://path/to/file.txt',
      type: 'txt'
    }
  ],
  data: []
});

uploadTask.on('progress', (uploadedSize, totalSize) => {
  console.log(`Progress: ${uploadedSize}/${totalSize}`);
});
```

### 文件下载

```typescript
let downloadTask = await request.downloadFile(getContext(), {
  url: 'https://example.com/file.zip',
  filePath: '/data/storage/el2/files/download.zip'
});

downloadTask.on('progress', (downloadedSize, totalSize) => {
  console.log(`Progress: ${downloadedSize}/${totalSize}`);
});
```

## WebSocket

来源: [WebSocket连接](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-websocket)

### 建立连接

```typescript
import { webSocket } from '@kit.NetworkKit';

let ws = webSocket.createWebSocket();

ws.connect('ws://example.com/socket', (err, value) => {
  if (!err) {
    console.log('Connected');
  }
});
```

### 发送消息

```typescript
ws.send('Hello Server', (err) => {
  if (err) {
    console.error('Send error:', err);
  }
});
```

### 接收消息

```typescript
ws.on('message', (err, data) => {
  if (!err) {
    console.log('Received:', data);
  }
});
```

### 关闭连接

```typescript
ws.close((err) => {
  if (!err) {
    console.log('Closed');
  }
});
```

## Socket 连接

来源: [Socket连接](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-socket)

### TCP Socket

来源: [TCP Socket](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-socket#tcp-socket开发)

```typescript
import { socket } from '@kit.NetworkKit';

let tcp = socket.constructTCPSocketInstance();

await tcp.bind({ address: '0.0.0.0', port: 8888, family: 1 });

await tcp.connect({
  address: '192.168.1.1',
  port: 9999,
  family: 1
});

tcp.on('message', (data) => {
  console.log('Received:', data);
});

await tcp.send({ data: 'Hello' });
```

### UDP Socket

来源: [UDP Socket](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-socket#udp-socket开发)

```typescript
let udp = socket.constructUDPSocketInstance();

await udp.bind({ address: '0.0.0.0', port: 8888, family: 1 });

udp.on('message', (data) => {
  console.log('Received:', data);
});

await udp.send({
  data: 'Hello',
  address: { address: '192.168.1.1', port: 9999, family: 1 }
});
```

## 网络状态

来源: [网络连接管理](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkts-net-connection)

### 监听网络变化

```typescript
import { connection } from '@kit.NetworkKit';

let netCon = connection.createNetConnection();

netCon.register((error, data) => {
  if (!error) {
    console.log('Network type:', data.type);
    console.log('Is available:', data.isAvailable);
  }
});

// 取消注册
netCon.unregister();
```

### 获取网络类型

```typescript
let netType = connection.getConnectionType();
console.log('Network type:', netType);
```