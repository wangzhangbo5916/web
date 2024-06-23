# DOM 和浏览器 API

## XMLHttpRequest 对象

### 概述

XMLHttpRequest 是一种传统的方式，用于在不重新加载整个页面的情况下与服务器进行数据交换。它支持同步和异步请求，但一般推荐使用异步请求。

### 使用方法

**创建和发送请求**

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    const response = JSON.parse(xhr.responseText);
    console.log('Response:', response);
  }
};

xhr.send();
```

**处理 POST 请求**

```js
const xhr = new XMLHttpRequest();
xhr.open('POST', 'https://api.example.com/data', true);
xhr.setRequestHeader('Content-Type', 'application/json;charset=UTF-8');

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    const response = JSON.parse(xhr.responseText);
    console.log('Response:', response);
  }
};

const data = JSON.stringify({ key: 'value' });
xhr.send(data);
```

**处理错误**

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      const response = JSON.parse(xhr.responseText);
      console.log('Response:', response);
    } else {
      console.error('Error:', xhr.statusText);
    }
  }
};

xhr.onerror = function () {
  console.error('Request failed');
};

xhr.send();
```

### 为什么请求成功的判断是readyState === 4&&status === 200？

在使用 XMLHttpRequest 对象进行网络请求时，判断请求是否成功需要检查两个属性：readyState 和 status。这是因为 XMLHttpRequest 的生命周期包含多个状态，而 HTTP 响应状态码则指示了请求的结果。以下是详细的解释：

#### readyState 属性

readyState 属性表示 XMLHttpRequest 对象的当前状态。它有以下五个可能的值：

- 0 (UNSENT)：XMLHttpRequest 对象已被创建，但尚未调用 open() 方法。
- 1 (OPENED)：open() 方法已经被调用，但尚未调用 send() 方法。
- 2 (HEADERS_RECEIVED)：send() 方法已经被调用，并且头部和状态已经可获得。
- 3 (LOADING)：响应体正在接收中，数据部分可用了。
- 4 (DONE)：请求已完成，响应已完全接收。

只有当 readyState 为 4 时，表示请求已完成且响应已完全接收。

#### status 属性

status 属性表示 HTTP 响应状态码。常见的状态码包括：

- 200 (OK)：请求成功，服务器返回所请求的数据。
- 404 (Not Found)：请求的资源不存在。
- 500 (Internal Server Error)：服务器内部错误。

只有当 status 为 200 时，表示请求成功且服务器返回了所请求的数据。

### 判断请求成功的条件

为了确保请求成功并且响应已完全接收，需要同时满足以下两个条件：

readyState === 4：请求已完成，响应已完全接收。
status === 200：HTTP 状态码为 200，表示请求成功。

### AJAX如何取消正在请求中的请求？

XMLHttpRequest 对象提供了一个 abort() 方法，可以用来取消正在进行的请求。

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      const response = JSON.parse(xhr.responseText);
      console.log('Response:', response);
    } else {
      console.error('Error:', xhr.statusText);
    }
  }
};

// 发送请求
xhr.send();

// 取消请求
setTimeout(() => {
  xhr.abort();
  console.log('Request aborted');
}, 2000); // 2秒后取消请求
```

## Fetch API

### 概述

Fetch API 是一种更现代的方式，用于进行网络请求。它基于 Promises，提供了更简洁和灵活的接口，并且默认情况下是异步的。

### 使用方法

**发送 GET 请求**

```js
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log('Response:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```


**发送 POST 请求**

```js
fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ key: 'value' })
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log('Response:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

**处理错误**

```js
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log('Response:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

**使用 async/await**

```js
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    const data = await response.json();
    console.log('Response:', data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

fetchData();
```

#### 比较 XMLHttpRequest 和 Fetch API

##### 优点和缺点

**XMLHttpRequest**

**优点**：

- 兼容性好，支持所有现代浏览器。
- 支持同步请求（尽管不推荐）。

**缺点**：

- API 设计较为复杂和冗长。
- 需要手动处理状态变化和错误。

**Fetch API**

**优点**：

- API 设计简洁，基于 Promise，易于使用。
- 默认是异步的，支持更现代的异步编程方式（如 async/await）。
- 提供更好的错误处理机制。

**缺点**：

- 不能直接取消请求，需借助 AbortController。
- 兼容性较差，需在旧版浏览器中使用 polyfill。

#### 总结

- XMLHttpRequest：适用于需要兼容旧版浏览器或需要同步请求的场景，但 API 复杂且冗长。
- Fetch API：适用于现代 Web 开发，API 简洁且灵活，推荐使用。

### Fetch如何取消请求？

Fetch API 本身不提供直接取消请求的方法，但是可以通过 AbortController 来实现取消请求。

```js
const controller = new AbortController();
const signal = controller.signal;

fetch('https://api.example.com/data', { signal })
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok ' + response.statusText);
    }
    return response.json();
  })
  .then(data => {
    console.log('Response:', data);
  })
  .catch(error => {
    if (error.name === 'AbortError') {
      console.log('Fetch aborted');
    } else {
      console.error('Fetch error:', error);
    }
  });

// 取消请求
setTimeout(() => {
  controller.abort();
  console.log('Fetch request aborted');
}, 2000); // 2秒后取消请求
```

### 目前流行的请求库有哪些？

#### Axios

Axios 是一个基于 Promise 的 HTTP 客户端，用于浏览器和 Node.js。它支持拦截请求和响应、取消请求、自动转换 JSON 数据等功能。

##### 使用方法

**发送 GET 请求**

```js
import axios from 'axios';

axios.get('https://api.example.com/data')
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**发送 POST 请求**

```js
axios.post('https://api.example.com/data', {
  key: 'value'
})
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**取消请求**

```js
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('https://api.example.com/data', {
  cancelToken: source.token
})
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    if (axios.isCancel(error)) {
      console.log('Request canceled', error.message);
    } else {
      console.error('Error:', error);
    }
  });

// 取消请求
source.cancel('Operation canceled by the user.');
```

#### jQuery.ajax

jQuery 是一个流行的 JavaScript 库，提供了简洁的 AJAX 操作。尽管在现代开发中使用 jQuery 的情况减少，但在一些遗留项目中仍然很常见。

##### 使用方法

**发送 GET 请求**

```js
$.ajax({
  url: 'https://api.example.com/data',
  method: 'GET',
  success: function(data) {
    console.log('Response:', data);
  },
  error: function(error) {
    console.error('Error:', error);
  }
});
```

**发送 POST 请求**

```js
$.ajax({
  url: 'https://api.example.com/data',
  method: 'POST',
  contentType: 'application/json',
  data: JSON.stringify({ key: 'value' }),
  success: function(data) {
    console.log('Response:', data);
  },
  error: function(error) {
    console.error('Error:', error);
  }
});
```

**取消请求**

```js
const request = $.ajax({
  url: 'https://api.example.com/data',
  method: 'GET'
});

// 取消请求
request.abort();
```

#### SuperAgent

SuperAgent 是一个轻量级的 AJAX 请求库，支持链式调用和 Promise。

##### 使用方法

**发送 GET 请求**

```js
const superagent = require('superagent');

superagent.get('https://api.example.com/data')
  .then(response => {
    console.log('Response:', response.body);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**发送 POST 请求**

```js
superagent.post('https://api.example.com/data')
  .send({ key: 'value' })
  .then(response => {
    console.log('Response:', response.body);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**取消请求**

```js
const request = superagent.get('https://api.example.com/data');

// 取消请求
request.abort();
```

#### Got (用于 Node.js)

Got 是一个基于 Promise 的 HTTP 请求库，专为 Node.js 设计，支持许多高级功能如重试、流、取消请求等。

##### 使用方法

**发送 GET 请求**

```js
const got = require('got');

got('https://api.example.com/data')
  .then(response => {
    console.log('Response:', response.body);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**发送 POST 请求**

```js
got.post('https://api.example.com/data', {
  json: { key: 'value' },
  responseType: 'json'
})
  .then(response => {
    console.log('Response:', response.body);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**取消请求**

```js
const request = got('https://api.example.com/data');

// 取消请求
request.cancel();
```

#### 总结

- Axios：功能强大，支持拦截器、取消请求等，适用于浏览器和 Node.js。
- Fetch API：现代浏览器内置，基于 Promise，适用于简单的网络请求。
- jQuery.ajax：适用于使用 jQuery 的遗留项目。
- SuperAgent：轻量级，链式调用，适用于浏览器和 Node.js。
- Got：专为 Node.js 设计，功能丰富，适用于服务器端应用。


