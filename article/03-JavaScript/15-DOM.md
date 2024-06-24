# DOM

## 查询和修改 DOM 元素

### 查询 DOM 元素

使用以下方法可以查询 DOM 元素：

- document.getElementById(id): 通过元素的 ID 查询元素。
- document.getElementsByClassName(className): 通过类名查询元素集合。
- document.getElementsByTagName(tagName): 通过标签名查询元素集合。
- document.querySelector(selector): 通过 CSS 选择器查询第一个匹配的元素。
- document.querySelectorAll(selector): 通过 CSS 选择器查询所有匹配的元素集合。

```js
// 通过 ID 查询元素
const elementById = document.getElementById('myElement');

// 通过类名查询元素集合
const elementsByClassName = document.getElementsByClassName('myClass');

// 通过标签名查询元素集合
const elementsByTagName = document.getElementsByTagName('div');

// 通过 CSS 选择器查询第一个匹配的元素
const elementBySelector = document.querySelector('.myClass');

// 通过 CSS 选择器查询所有匹配的元素集合
const elementsBySelectorAll = document.querySelectorAll('.myClass');
```

### 修改 DOM 元素

可以通过修改元素的属性、样式和内容来修改 DOM 元素。

```js
// 修改元素的属性
elementById.setAttribute('data-custom', 'value');
elementById.id = 'newId';

// 修改元素的样式
elementById.style.color = 'red';
elementById.style.fontSize = '20px';

// 修改元素的内容
elementById.textContent = 'Hello, World!';
elementById.innerHTML = '<strong>Hello, World!</strong>';
```

### 获取 HTML 元素的属性

你可以使用 getAttribute 方法来获取元素的属性值，也可以直接通过元素对象的属性来获取。

```js
// 获取元素
const element = document.getElementById('myElement');

// 使用 getAttribute 方法获取属性值
const classValue = element.getAttribute('class');
console.log('Class:', classValue);

// 直接通过元素对象的属性获取属性值
const idValue = element.id;
console.log('ID:', idValue);

const dataCustomValue = element.dataset.custom; // 获取 data-* 属性
console.log('Data-Custom:', dataCustomValue);
```

### 修改 HTML 元素的属性

你可以使用 setAttribute 方法来修改元素的属性值，也可以直接通过元素对象的属性来进行修改。

```js
// 获取元素
const element = document.getElementById('myElement');

// 使用 setAttribute 方法修改属性值
element.setAttribute('class', 'newClass');
console.log('New Class:', element.getAttribute('class'));

// 直接通过元素对象的属性修改属性值
element.id = 'newId';
console.log('New ID:', element.id);

element.dataset.custom = 'newValue'; // 修改 data-* 属性
console.log('New Data-Custom:', element.dataset.custom);
```

### 移除 HTML 元素的属性

你可以使用 removeAttribute 方法来移除元素的属性。

```js
// 获取元素
const element = document.getElementById('myElement');

// 移除属性
element.removeAttribute('class');
console.log('Class after removal:', element.getAttribute('class')); // null

element.removeAttribute('data-custom');
console.log('Data-Custom after removal:', element.getAttribute('data-custom')); // null
```

## 事件处理

事件处理是指为 DOM 元素绑定事件监听器，以便在事件发生时执行特定的代码。

### 添加事件监听器

可以使用 addEventListener 方法为元素添加事件监听器。

```js
// 获取元素
const button = document.getElementById('myButton');

// 添加点击事件监听器
button.addEventListener('click', (event) => {
  console.log('Button clicked!', event);
});

// 添加鼠标移入事件监听器
button.addEventListener('mouseover', (event) => {
  console.log('Mouse over button!', event);
});
```

### 移除事件监听器

可以使用 removeEventListener 方法移除事件监听器。

```js
// 定义事件处理函数
const handleClick = (event) => {
  console.log('Button clicked!', event);
};

// 添加事件监听器
button.addEventListener('click', handleClick);

// 移除事件监听器
button.removeEventListener('click', handleClick);
```

## 创建和删除节点

### 创建节点

可以使用 document.createElement 方法创建新的 DOM 元素，并使用 appendChild 或 insertBefore 方法将其插入到 DOM 树中。

```js
// 创建新的 div 元素
const newDiv = document.createElement('div');

// 设置元素的属性和内容
newDiv.id = 'newDiv';
newDiv.textContent = 'This is a new div';

// 将新元素添加到 body 中
document.body.appendChild(newDiv);

// 创建新的 p 元素
const newParagraph = document.createElement('p');
newParagraph.textContent = 'This is a new paragraph';

// 将新元素插入到新 div 元素之前
document.body.insertBefore(newParagraph, newDiv);
```

### 删除节点

可以使用 removeChild 方法删除 DOM 元素。

```js
// 获取要删除的元素
const elementToRemove = document.getElementById('myElement');

// 获取父元素
const parentElement = elementToRemove.parentNode;

// 删除元素
parentElement.removeChild(elementToRemove);
```

### 创建 DOM 片段

你可以使用 document.createDocumentFragment() 方法来创建一个新的 DOM 片段。然后，你可以向这个片段中添加子节点。

```js
// 创建一个新的 DOM 片段
const fragment = document.createDocumentFragment();

// 创建一些新的元素并添加到片段中
const newDiv = document.createElement('div');
newDiv.textContent = 'This is a new div';
fragment.appendChild(newDiv);

const newParagraph = document.createElement('p');
newParagraph.textContent = 'This is a new paragraph';
fragment.appendChild(newParagraph);

// 将片段插入到文档中
document.body.appendChild(fragment);
```

### 删除 DOM 片段

由于 DOM 片段本身是一个临时的容器，它在被插入到文档中后，其子节点会被实际的 DOM 树接管，因此你不需要显式地删除 DOM 片段。但是，如果你需要删除片段中的某些节点，可以在将片段插入到文档之前进行操作。

```js
// 创建一个新的 DOM 片段
const fragment = document.createDocumentFragment();

// 创建一些新的元素并添加到片段中
const newDiv = document.createElement('div');
newDiv.textContent = 'This is a new div';
fragment.appendChild(newDiv);

const newParagraph = document.createElement('p');
newParagraph.textContent = 'This is a new paragraph';
fragment.appendChild(newParagraph);

// 删除片段中的某个节点
fragment.removeChild(newDiv);

// 将片段插入到文档中
document.body.appendChild(fragment);
```
