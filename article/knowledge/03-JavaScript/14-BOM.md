# BOM

## window 对象

window 对象是所有浏览器对象的全局对象，表示浏览器窗口。所有全局 JavaScript 对象、函数和变量都是 window 对象的属性。

常用属性和方法

- window.alert(message): 显示一个警告框。
- window.confirm(message): 显示一个确认框，返回 true 或 false。
- window.prompt(message, default): 显示一个提示框，返回用户输入的值。
- window.setTimeout(function, milliseconds): 在指定的毫秒数后调用函数。
- window.setInterval(function, milliseconds): 每隔指定的毫秒数调用函数。
- window.open(url, name, specs): 打开一个新窗口。

```js
// 显示警告框
window.alert('Hello, World!');

// 显示确认框
const result = window.confirm('Are you sure?');
console.log('Confirm result:', result);

// 显示提示框
const userInput = window.prompt('Please enter your name:', 'John Doe');
console.log('User input:', userInput);

// 设置定时器
const timeoutId = window.setTimeout(() => {
  console.log('Timeout executed');
}, 2000);

// 设置间隔器
const intervalId = window.setInterval(() => {
  console.log('Interval executed');
}, 1000);

// 打开新窗口
const newWindow = window.open('https://www.example.com', '_blank', 'width=600,height=400');
```

## navigator 对象

navigator 对象包含有关浏览器的信息，如用户代理、平台、语言等。

常用属性和方法

- navigator.userAgent: 返回浏览器的用户代理字符串。
- navigator.platform: 返回浏览器运行所在的操作系统平台。
- navigator.language: 返回浏览器的语言。
- navigator.onLine: 返回浏览器是否联网。

```js
// 获取用户代理字符串
console.log('User Agent:', navigator.userAgent);

// 获取平台信息
console.log('Platform:', navigator.platform);

// 获取浏览器语言
console.log('Language:', navigator.language);

// 检查是否联网
console.log('Online:', navigator.onLine);
```

## location 对象

location 对象包含当前 URL 的信息，并提供了操作 URL 的方法。

常用属性和方法

- location.href: 返回或设置当前页面的 URL。
- location.protocol: 返回当前 URL 的协议部分。
- location.host: 返回当前 URL 的主机部分。
- location.pathname: 返回当前 URL 的路径部分。
- location.search: 返回当前 URL 的查询字符串部分。
- location.hash: 返回当前 URL 的哈希部分。
- location.reload(): 重新加载当前页面。
- location.assign(url): 加载新的页面。
- location.replace(url): 用新的页面替换当前页面。

```js
// 获取当前 URL
console.log('URL:', location.href);

// 获取协议部分
console.log('Protocol:', location.protocol);

// 获取主机部分
console.log('Host:', location.host);

// 获取路径部分
console.log('Pathname:', location.pathname);

// 获取查询字符串部分
console.log('Search:', location.search);

// 获取哈希部分
console.log('Hash:', location.hash);

// 重新加载当前页面
location.reload();

// 加载新的页面
location.assign('https://www.example.com');

// 用新的页面替换当前页面
location.replace('https://www.example.com');
```

## history 对象

history 对象包含用户的浏览历史记录，并提供了在历史记录中导航的方法。

常用属性和方法

- history.length: 返回浏览历史的长度。
- history.back(): 加载历史列表中的前一个 URL。
- history.forward(): 加载历史列表中的下一个 URL。
- history.go(number): 加载历史列表中的指定 URL。

```js
// 获取浏览历史的长度
console.log('History length:', history.length);

// 加载前一个 URL
history.back();

// 加载下一个 URL
history.forward();

// 加载指定的 URL
history.go(-1); // 相当于 history.back()
history.go(1);  // 相当于 history.forward()
```

### history如何不刷新页面就能替换页面链接？

#### 使用 pushState 和 replaceState

- history.pushState(state, title, url): 将新的状态添加到历史记录的顶部。
- history.replaceState(state, title, url): 替换当前的历史记录条目。

**参数说明**

- state: 一个与指定网址关联的状态对象。这个对象可以在 popstate 事件中被访问。
- title: 新页面的标题（目前大多数浏览器会忽略这个参数）。
- url: 新的 URL。必须是相同源（same-origin）的 URL。

```js
// 当前 URL: https://www.example.com/page1

// 将新的状态添加到历史记录的顶部
history.pushState({ page: 2 }, 'Title 2', '/page2');

// 当前 URL: https://www.example.com/page2

```

在这个示例中，URL 从 /page1 变为 /page2，而不会触发页面刷新。

```js
// 当前 URL: https://www.example.com/page1

// 替换当前的历史记录条目
history.replaceState({ page: 2 }, 'Title 2', '/page2');

// 当前 URL: https://www.example.com/page2
```

在这个示例中，URL 从 /page1 变为 /page2，而不会触发页面刷新，并且当前的历史记录条目被替换。


#### 监听 popstate 事件

当用户点击浏览器的前进或后退按钮时，会触发 popstate 事件。你可以监听这个事件来处理状态变化。

```js
window.addEventListener('popstate', (event) => {
  console.log('Location: ' + document.location + ', State: ' + JSON.stringify(event.state));
});
```


## screen 对象

screen 对象包含有关用户屏幕的信息，如屏幕的宽度、高度、颜色深度等。

常用属性

- screen.width: 返回屏幕的宽度。
- screen.height: 返回屏幕的高度。
- screen.availWidth: 返回屏幕的可用宽度。
- screen.availHeight: 返回屏幕的可用高度。
- screen.colorDepth: 返回屏幕的颜色深度。
- screen.pixelDepth: 返回屏幕的像素深度。

```js
// 获取屏幕的宽度和高度
console.log('Screen width:', screen.width);
console.log('Screen height:', screen.height);

// 获取屏幕的可用宽度和高度
console.log('Available width:', screen.availWidth);
console.log('Available height:', screen.availHeight);

// 获取屏幕的颜色深度和像素深度
console.log('Color depth:', screen.colorDepth);
console.log('Pixel depth:', screen.pixelDepth);
```

## URL 对象

JavaScript 的 URL 对象是一个内置的全局对象，用于解析、构建和操作 URL。这个对象提供了一个便捷的接口，可以轻松地访问和修改 URL 的各个部分。

### 创建 URL 对象

你可以使用 URL 构造函数来创建一个新的 URL 对象。构造函数接受两个参数：一个是相对或绝对 URL 字符串，另一个是可选的基准 URL。

```js
// 创建一个绝对 URL 对象
const url = new URL('https://www.example.com/path?query=hello#hash');
console.log(url);

// 创建一个相对 URL 对象，指定基准 URL
const relativeUrl = new URL('/path?query=hello#hash', 'https://www.example.com');
console.log(relativeUrl);
```

### URL 对象的属性

URL 对象提供了多个属性，可以方便地访问 URL 的各个部分。

- href: 完整的 URL。
- protocol: URL 的协议部分（如 http: 或 https:）。
- host: URL 的主机部分，包括端口号（如 www.example.com:80）。
- hostname: URL 的主机名部分，不包括端口号（如 www.example.com）。
- port: URL 的端口号（如 80）。
- pathname: URL 的路径部分（如 /path）。
- search: URL 的查询字符串部分，包括问号（如 ?query=hello）。
- searchParams: 一个 URLSearchParams 对象，表示查询参数。
- hash: URL 的哈希部分，包括井号（如 #hash）。

```js
const url = new URL('https://www.example.com:8080/path?query=hello#hash');

console.log('href:', url.href);
console.log('protocol:', url.protocol);
console.log('host:', url.host);
console.log('hostname:', url.hostname);
console.log('port:', url.port);
console.log('pathname:', url.pathname);
console.log('search:', url.search);
console.log('hash:', url.hash);
```

```bash
href: https://www.example.com:8080/path?query=hello#hash
protocol: https:
host: www.example.com:8080
hostname: www.example.com
port: 8080
pathname: /path
search: ?query=hello
hash: #hash
```

### 修改 URL 对象的属性

你可以直接修改 URL 对象的属性，然后获取更新后的 URL。

```js
const url = new URL('https://www.example.com:8080/path?query=hello#hash');

// 修改各个部分
url.protocol = 'http:';
url.hostname = 'example.org';
url.port = '9090';
url.pathname = '/newpath';
url.search = '?newquery=world';
url.hash = '#newhash';

console.log('Updated URL:', url.href);
```

```bash
Updated URL: http://example.org:9090/newpath?newquery=world#newhash
```

### 使用 URLSearchParams 对象

URL 对象的 searchParams 属性是一个 URLSearchParams 对象，可以方便地操作查询参数。

**添加、获取和删除查询参数**

```js
const url = new URL('https://www.example.com/path?query=hello');

// 添加查询参数
url.searchParams.append('newParam', 'newValue');
console.log('URL with new param:', url.href);

// 获取查询参数
const queryValue = url.searchParams.get('query');
console.log('Query value:', queryValue);

// 删除查询参数
url.searchParams.delete('query');
console.log('URL after deleting query param:', url.href);

// 修改查询参数
url.searchParams.set('newParam', 'updatedValue');
console.log('URL after updating param:', url.href);
```

```bash
URL with new param: https://www.example.com/path?query=hello&newParam=newValue
Query value: hello
URL after deleting query param: https://www.example.com/path?newParam=newValue
URL after updating param: https://www.example.com/path?newParam=updatedValue
```

### 迭代查询参数

你可以使用 URLSearchParams 对象的迭代方法来遍历查询参数。

```js
const url = new URL('https://www.example.com/path?param1=value1&param2=value2');

// 使用 for...of 迭代查询参数
for (const [key, value] of url.searchParams) {
  console.log(`${key}: ${value}`);
}
```

```bash
param1: value1
param2: value2
```


## URL 编码和解码

### encodeURI 和 decodeURI

- encodeURI(url): 对整个 URL 进行编码，但不会编码保留字符（如 :、/、?、# 等）。
- decodeURI(encodedURL): 解码通过 encodeURI 编码的 URL。

```js
const url = 'https://www.example.com/search?query=hello world&name=John Doe';

// 对整个 URL 进行编码
const encodedURL = encodeURI(url);
console.log('Encoded URL:', encodedURL);

// 解码 URL
const decodedURL = decodeURI(encodedURL);
console.log('Decoded URL:', decodedURL);
```

```bash
Encoded URL: https://www.example.com/search?query=hello%20world&name=John%20Doe
Decoded URL: https://www.example.com/search?query=hello world&name=John Doe
```

### encodeURIComponent 和 decodeURIComponent

- encodeURIComponent(component): 对 URL 的组件（如查询参数）进行编码，会编码所有非字母数字字符。
- decodeURIComponent(encodedComponent): 解码通过 encodeURIComponent 编码的组件。

```js
const queryParam = 'hello world&name=John Doe';

// 对 URL 组件进行编码
const encodedParam = encodeURIComponent(queryParam);
console.log('Encoded Query Param:', encodedParam);

// 解码 URL 组件
const decodedParam = decodeURIComponent(encodedParam);
console.log('Decoded Query Param:', decodedParam);
```

```bash
Encoded Query Param: hello%20world%26name%3DJohn%20Doe
Decoded Query Param: hello world&name=John Doe
```

### 结合使用

在实际应用中，通常需要结合使用 encodeURIComponent 和 decodeURIComponent 来处理 URL 的查询参数，而使用 encodeURI 和 decodeURI 来处理整个 URL。

```js
const baseURL = 'https://www.example.com/search';
const queryParams = {
  query: 'hello world',
  name: 'John Doe'
};

// 构建查询字符串
const queryString = Object.keys(queryParams)
  .map(key => `${encodeURIComponent(key)}=${encodeURIComponent(queryParams[key])}`)
  .join('&');

// 构建完整的 URL
const fullURL = `${baseURL}?${queryString}`;
console.log('Full URL:', fullURL);

// 解码 URL
const url = new URL(fullURL);
const decodedQuery = decodeURIComponent(url.searchParams.get('query'));
const decodedName = decodeURIComponent(url.searchParams.get('name'));
console.log('Decoded Query:', decodedQuery);
console.log('Decoded Name:', decodedName);
```

```bash
Full URL: https://www.example.com/search?query=hello%20world&name=John%20Doe
Decoded Query: hello world
Decoded Name: John Doe
```

### 总结

- encodeURI: 对整个 URL 进行编码，但不会编码保留字符。
- decodeURI: 解码通过 encodeURI 编码的 URL。
- encodeURIComponent: 对 URL 的组件（如查询参数）进行编码，会编码所有非字母数字字符。
- decodeURIComponent: 解码通过 encodeURIComponent 编码的组件。
