# 网络通信参考

## HTTP 请求

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

### TCP Socket
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
