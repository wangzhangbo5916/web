# WebSocket

HTML5 引入了 WebSocket API，这是一种在客户端和服务器之间建立全双工通信通道的协议。与传统的 HTTP 请求-响应模式不同，WebSocket 允许在单个 TCP 连接上进行双向通信。这使得 WebSocket 非常适合需要实时数据更新的应用程序，如在线聊天、实时游戏和股票行情等。

## WebSocket 基础

### 创建 WebSocket 连接

要创建一个 WebSocket 连接，你需要实例化一个 WebSocket 对象，并传入要连接的 WebSocket 服务器的 URL。

```js
// 创建 WebSocket 连接
const socket = new WebSocket('ws://example.com/socket');

// 处理连接打开事件
socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
  // 发送消息到服务器
  socket.send('Hello Server!');
});

// 处理收到消息事件
socket.addEventListener('message', (event) => {
  console.log('Message from server:', event.data);
});

// 处理连接关闭事件
socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
});

// 处理连接错误事件
socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});
```

### WebSocket 方法

- send(data): 向服务器发送数据。data 可以是字符串、Blob 或 ArrayBuffer。
- close([code[, reason]]): 关闭 WebSocket 连接。可以选择传入关闭代码和原因。

```js
// 发送字符串消息
socket.send('Hello Server!');

// 发送 Blob 数据
const blob = new Blob(['Hello Server!'], { type: 'text/plain' });
socket.send(blob);

// 发送 ArrayBuffer 数据
const buffer = new ArrayBuffer(16);
const view = new Uint8Array(buffer);
view[0] = 42;
socket.send(buffer);

// 关闭 WebSocket 连接
socket.close(1000, 'Work complete');
```

### WebSocket 事件

- open: 当 WebSocket 连接成功建立时触发。
- message: 当收到服务器发送的消息时触发。
- close: 当 WebSocket 连接关闭时触发。
- error: 当 WebSocket 连接发生错误时触发。

```js
socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
});

socket.addEventListener('message', (event) => {
  console.log('Message from server:', event.data);
});

socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
});

socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});
```

### 处理二进制数据

WebSocket 支持发送和接收二进制数据，如 ArrayBuffer 和 Blob。

```js
// 设置 WebSocket 二进制类型
socket.binaryType = 'arraybuffer';

// 处理收到的二进制数据
socket.addEventListener('message', (event) => {
  if (event.data instanceof ArrayBuffer) {
    const view = new Uint8Array(event.data);
    console.log('Received ArrayBuffer:', view);
  } else if (event.data instanceof Blob) {
    const reader = new FileReader();
    reader.onload = () => {
      const arrayBuffer = reader.result;
      const view = new Uint8Array(arrayBuffer);
      console.log('Received Blob:', view);
    };
    reader.readAsArrayBuffer(event.data);
  }
});
```

### 综合示例

以下是一个综合示例，展示了如何使用 WebSocket 在客户端和服务器之间进行双向通信。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WebSocket Example</title>
</head>
<body>
  <h1>WebSocket Example</h1>
  <button id="sendMessageButton">Send Message</button>
  <div id="messages"></div>
  <script src="script.js"></script>
</body>
</html>
```

```js
// 创建 WebSocket 连接
const socket = new WebSocket('ws://example.com/socket');

// 处理连接打开事件
socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
  document.getElementById('sendMessageButton').addEventListener('click', () => {
    // 发送消息到服务器
    socket.send('Hello Server!');
  });
});

// 处理收到消息事件
socket.addEventListener('message', (event) => {
  const messagesDiv = document.getElementById('messages');
  const messageElement = document.createElement('div');
  messageElement.textContent = `Message from server: ${event.data}`;
  messagesDiv.appendChild(messageElement);
});

// 处理连接关闭事件
socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
});

// 处理连接错误事件
socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});
```

### 主动结束 WebSocket 通信

在 WebSocket 通信中，你可以通过调用 WebSocket 对象的 close 方法来主动结束 WebSocket 连接。close 方法可以接受两个可选参数：关闭代码（code）和关闭原因（reason）。关闭代码是一个表示连接关闭原因的数字，关闭原因是一个描述关闭原因的字符串。

- code: 一个表示连接关闭原因的数字（可选）。标准的关闭代码包括：
  - 1000: 正常关闭
  - 1001: 终端离开
  - 1002: 协议错误
  - 1003: 不可接受的数据类型
  - 1006: 异常关闭（仅用于报告，不能由应用程序指定）
  - 1007: 无效的数据帧
  - 1008: 策略违规
  - 1009: 消息过大
  - 1010: 客户端期望的扩展未能协商
  - 1011: 服务器遇到不可恢复的情况
  - 1012: 服务重启
  - 1013: 临时服务器过载
  - 1015: TLS 握手失败（仅用于报告，不能由应用程序指定）
- reason: 一个描述关闭原因的字符串（可选）。

```js
// 创建 WebSocket 连接
const socket = new WebSocket('ws://example.com/socket');

// 打开连接时的处理
socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
});

// 接收到消息时的处理
socket.addEventListener('message', (event) => {
  console.log('Message from server:', event.data);
});

// 关闭连接时的处理
socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
});

// 发生错误时的处理
socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});

// 主动关闭 WebSocket 连接
function closeWebSocket() {
  socket.close(1000, 'Work complete');
  console.log('WebSocket connection closed.');
}

// 示例：在 5 秒后主动关闭 WebSocket 连接
setTimeout(closeWebSocket, 5000);
```

#### 处理关闭事件

当 WebSocket 连接关闭时，close 事件会被触发。你可以在 close 事件的处理函数中获取关闭代码和原因。

```js
// 创建 WebSocket 连接
const socket = new WebSocket('ws://example.com/socket');

// 打开连接时的处理
socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
});

// 接收到消息时的处理
socket.addEventListener('message', (event) => {
  console.log('Message from server:', event.data);
});

// 关闭连接时的处理
socket.addEventListener('close', (event) => {
  console.log(`WebSocket is closed now. Code: ${event.code}, Reason: ${event.reason}`);
});

// 发生错误时的处理
socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});

// 主动关闭 WebSocket 连接
function closeWebSocket() {
  socket.close(1000, 'Normal closure');
  console.log('WebSocket connection closed.');
}

// 示例：在 5 秒后主动关闭 WebSocket 连接
setTimeout(closeWebSocket, 5000);
```

### 注意事项

#### 1. 连接管理

WebSocket 连接需要进行管理，包括连接的建立、保持和关闭。你需要处理连接断开和重新连接的情况。

```js
const socket = new WebSocket('ws://example.com/socket');

socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
});

socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
  // 尝试重新连接
  setTimeout(() => {
    socket = new WebSocket('ws://example.com/socket');
  }, 1000);
});

socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});
```

#### 2. 安全性

使用 wss:// 而不是 ws:// 来确保 WebSocket 连接的安全性。wss:// 使用 TLS 加密，类似于 HTTPS。

```js
const socket = new WebSocket('wss://example.com/socket');
```

#### 3. 心跳机制

为了保持连接的活跃性，可以实现心跳机制，定期发送心跳消息以检测连接状态。

```js
const socket = new WebSocket('ws://example.com/socket');

let heartbeatInterval;

socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
  // 启动心跳机制
  heartbeatInterval = setInterval(() => {
    socket.send(JSON.stringify({ type: 'heartbeat' }));
  }, 30000); // 每30秒发送一次心跳
});

socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
  clearInterval(heartbeatInterval);
});
```


#### 4. 处理大数据量

当需要传输大量数据时，确保数据的分块和重组，以避免单次传输过多数据导致的性能问题。

```js
const socket = new WebSocket('ws://example.com/socket');

const largeData = new Array(10000).fill('data').join(',');

socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
  // 分块发送数据
  const chunkSize = 1024;
  for (let i = 0; i < largeData.length; i += chunkSize) {
    socket.send(largeData.slice(i, i + chunkSize));
  }
});
```


#### 5. 处理并发连接

在高并发场景下，服务器需要能够处理大量 WebSocket 连接，确保服务器的性能和稳定性。

**示例**

- 使用负载均衡器分发连接
- 优化服务器资源管理
- 使用高性能的 WebSocket 服务器实现

#### 综合示例

以下是一个综合示例，展示了如何使用 WebSocket 实现一个简单的实时聊天应用，并处理连接管理、安全性和心跳机制等注意事项。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WebSocket Chat Example</title>
</head>
<body>
  <h1>WebSocket Chat Example</h1>
  <div id="chat">
    <div id="messages"></div>
    <input type="text" id="messageInput" placeholder="Type a message">
    <button id="sendMessageButton">Send</button>
  </div>
  <script src="script.js"></script>
</body>
</html>
```

```js
let socket = new WebSocket('wss://example.com/socket');
let heartbeatInterval;

socket.addEventListener('open', (event) => {
  console.log('WebSocket is open now.');
  // 启动心跳机制
  heartbeatInterval = setInterval(() => {
    socket.send(JSON.stringify({ type: 'heartbeat' }));
  }, 30000); // 每30秒发送一次心跳
});

socket.addEventListener('message', (event) => {
  const messagesDiv = document.getElementById('messages');
  const messageElement = document.createElement('div');
  messageElement.textContent = `Message from server: ${event.data}`;
  messagesDiv.appendChild(messageElement);
});

socket.addEventListener('close', (event) => {
  console.log('WebSocket is closed now.');
  clearInterval(heartbeatInterval);
  // 尝试重新连接
  setTimeout(() => {
    socket = new WebSocket('wss://example.com/socket');
  }, 1000);
});

socket.addEventListener('error', (event) => {
  console.error('WebSocket error observed:', event);
});

document.getElementById('sendMessageButton').addEventListener('click', () => {
  const messageInput = document.getElementById('messageInput');
  const message = messageInput.value;
  socket.send(message);
  messageInput.value = '';
});
```