# CSS 内容概述

## CSS 选择器

- **元素选择器**: 直接通过 HTML 元素名称选择，如 div, p, h1 等。
- **类选择器**: 通过元素的 class 属性选择，使用.标识，如 .classname。
- **ID 选择器**: 通过元素的 ID 选择，使用#标识，如 #idname。
- **属性选择器**: 根据元素属性选择，如 [type="text"]。
- **伪类选择器**: 选择处于特定状态的元素，如 :hover, :focus。
- **伪元素选择器**: 选择元素的特定部分，如 ::before, ::after。
- **组合选择器**: 使用组合器组合多个选择器，如后代选择器（空格）、子选择器（>）、相邻兄弟选择器（+）和通用兄弟选择器（~）。

### CSS 伪类选择器

1. :active - 选择活动状态的元素，通常是被鼠标点击时。
2. :hover - 当鼠标悬停在元素上时，选择该元素。
3. :focus - 选择获得焦点的元素，如输入框。
4. :visited - 选择已访问的链接。
5. :link - 选择未被访问的链接。
6. :checked - 选择被选中的 input 元素（如单选按钮或复选框）。
7. :disabled - 选择被禁用的表单元素。
8. :enabled - 选择启用的表单元素。
9. :first-child - 选择父元素的第一个子元素。
10. :last-child - 选择父元素的最后一个子元素。
11. :nth-child(n) - 选择父元素的第 n 个子元素。
12. :nth-last-child(n) - 选择父元素的倒数第 n 个子元素。
13. :nth-of-type(n) - 选择指定类型的第 n 个同级兄弟元素。
14. :nth-last-of-type(n) - 选择指定类型的倒数第 n 个同级兄弟元素。
15. :first-of-type - 选择指定类型的第一个同级兄弟元素。
16. :last-of-type - 选择指定类型的最后一个同级兄弟元素。
17. :only-child - 选择父元素的唯一子元素。
18. :only-of-type - 选择指定类型的唯一同级兄弟元素。
19. :empty - 选择没有子元素（包括文本节点）的元素。
20. :root - 选择文档的根元素，通常是 html 元素。
21. :target - 选择当前活动的目标元素（点击锚链接跳转到的元素）。
22. :not(selector) - 选择不符合指定选择器的元素。
23. :lang(language) - 选择指定语言的元素。

### CSS 伪元素选择器

1. ::before - 在元素内容的前面插入内容。
2. ::after - 在元素内容的后面插入内容。
3. ::first-letter - 选择元素的第一个字母。
4. ::first-line - 选择元素的第一行文本。
5. ::selection - 选择用户选择的元素部分（如文本选择）。

### CSS 组合选择器

组合选择器（也称为复合选择器）用于根据元素之间的特定关系来选择元素。以下是 CSS 中常用的组合选择器：

1. **后代选择器（Descendant Selector）**
   - 语法: A B
   - 描述: 选择所有位于 A 元素内部的 B 元素，不论 B 元素嵌套了多少层。
2. **子选择器（Child Selector）**
   - 语法: A > B
   - 描述: 选择所有作为 A 元素直接子元素的 B 元素。
3. **相邻兄弟选择器（Adjacent Sibling Selector）**
   - 语法: A + B
   - 描述: 选择所有紧接在 A 元素之后的同级 B 元素。
4. **通用兄弟选择器（General Sibling Selector）**
   - 语法: A ~ B
   - 描述: 选择所有在 A 元素之后的同级 B 元素，不论它们之间隔了多少个兄弟元素。
5. **分组选择器（Grouping Selector）**
   - 语法: A, B
   - 描述: 同时选择所有的 A 元素和 B 元素。这不是一个组合选择器，但它允许你将多个选择器合并在一起，以相同的方式应用一组样式。

### CSS 选择器的权重

1. **内联样式**
   - 权重: 1,0,0,0
   - 描述: 直接在 HTML 元素上使用的 style 属性。
2. **ID 选择器**
   - 权重: 0,1,0,0
   - 描述: 以#符号开始，后面跟有元素的 ID。
3. **类选择器、伪类选择器和属性选择器**
   - 权重: 0,0,1,0
   - 描述: 类选择器以.开始，伪类选择器如:hover，属性选择器如[type="text"]。
4. **元素选择器和伪元素选择器**
   - 权重: 0,0,0,1
   - 描述: 直接指定的元素标签，如 div，伪元素选择器如::before。
5. **通配符、子选择器、相邻选择器**
   - 权重: 0,0,0,0
   - 描述: 通配符\*，子选择器>，相邻选择器+和兄弟选择器~不增加权重。
6. **权重的叠加**
   - 描述: 选择器的权重是根据每一类中的数量叠加的。例如，#nav .item a:hover 将有一个权重的叠加为 0,1,2,1。
7. **!important 规则**
   - 描述: 任何规则后面加上!important 会覆盖上述权重计算，但使用时应谨慎，因为它会破坏 CSS 的层叠规则。
8. **权重的比较**
   - 描述: 当两条规则冲突时，权重较高的规则将胜出。如果权重相同，则最后声明的规则将被应用。
9. **继承**
   - 描述: 某些 CSS 属性值是可以继承的，继承的规则不具有权重，但可以被任何直接指定的规则（权重大于 0）覆盖。
10. **特殊情况**
    - 描述: 浏览器的默认样式表权重为最低，用户定义的样式表通常优先级高于默认样式表。

## CSS 属性和值

- **文本样式**: font-family, font-size, text-align, color, line-height, text-decoration 等。
- **背景**: background-color, background-image, background-position, background-repeat, background-size 等。
- **盒模型**: width, height, padding, margin, border 等。
- **定位**: position, top, right, bottom, left, z-index 等。
- **显示**: display, visibility, opacity 等。
- **浮动和清除**: float, clear 等。
- **列表**: list-style-type, list-style-position, list-style-image 等。
- **表格**: border-collapse, border-spacing, table-layout 等。

### CSS 盒模型

CSS盒模型是CSS中的基础概念，用来描述HTML文档中元素的布局和设计。一个元素的盒模型由以下几个部分组成：

1. 内容（Content）
    - 描述: 盒模型的中心部分，包含元素的实际内容，如文本、图片等。
    - 属性: width 和 height 用来设置内容区域的宽和高。
2. 内边距（Padding）
    - 描述: 内容区域周围的空间，位于内容和边框之间。
    - 属性: padding-top, padding-right, padding-bottom, padding-left 或简写为 padding。
3. 边框（Border）
    - 描述: 内边距和外边距之间的线，围绕着内边距和内容。
    - 属性: border-width, border-style, border-color 或简写为 border。
4. 外边距（Margin）
    - 描述: 边框外部的空间，用于隔开元素与其他元素。
    - 属性: margin-top, margin-right, margin-bottom, margin-left 或简写为 margin。

#### 盒模型布局的重要性

盒模型对于页面布局至关重要，它决定了元素如何在页面中占据空间以及元素之间如何相互作用。了解和掌握盒模型是进行网页设计和布局的基础。

#### 盒模型的类型

##### 标准盒模型（Content-Box）

- 特点: width 和 height 属性只包括内容区域。
- 计算总宽高: 总宽度 = 左右 margin + 左右 border + 左右 padding + width
- 计算总宽高: 总高度 = 上下 margin + 上下 border + 上下 padding + height

##### IE盒模型（Border-Box）

- 特点: width 和 height 属性包括内容、内边距和边框。
- 计算总宽高: 总宽度 = 左右 margin + width（已包含 padding 和 border）
- 计算总宽高: 总高度 = 上下 margin + height（已包含 padding 和 border）

### CSS 定位方式

CSS定位是布局中的一个重要概念，它决定了一个元素在页面中的位置。以下是CSS中的定位方式：

1. 静态定位（Static）
    - 描述: 默认定位方式，元素按照正常的文档流进行排列。
    - 属性: position: static;
    - 特点: 不受 top, bottom, left, right 和 z-index 属性的影响。
2. 相对定位（Relative）
    - 描述: 元素相对于其原始位置进行偏移。
    - 属性: position: relative;
    - 特点: 元素偏移后，原位置仍然保留。
3. 绝对定位（Absolute）
    - 描述: 元素相对于最近的已定位（非 static）祖先元素进行定位。
    - 属性: position: absolute;
    - 特点: 元素从文档流中脱离，不占据空间，可通过 top, bottom, left, right 属性进行精确控制。
4. 固定定位（Fixed）
    - 描述: 元素相对于浏览器窗口进行定位。
    - 属性: position: fixed;
    - 特点: 即使页面滚动，元素也会停留在指定位置。
5. 粘性定位（Sticky）
    - 描述: 结合了相对定位和固定定位的特点，根据用户的滚动位置进行定位。
    - 属性: position: sticky;
    - 特点: 元素在容器内滚动时像相对定位，到达指定偏移位置后像固定定位。

## CSS 布局

- **Flexbox**: 使用 display: flex 创建灵活的布局，涉及 flex-direction, justify-content, align-items, flex-wrap 等属性。
- **Grid**: 使用 display: grid 实现网格布局，包括 grid-template-columns, grid-template-rows, grid-gap, grid-area 等属性。
- **多列布局**: 利用 column-count, column-gap, column-rule 等属性创建多列文本流。

## CSS 视觉效果

- **盒阴影**: box-shadow 为元素添加阴影效果。
- **文本阴影**: text-shadow 为文本添加阴影效果。
- **边框**: border-radius 创建圆角边框，border-style, border-width, border-color 设置边框样式。
- **过渡**: transition 属性实现元素状态变化的平滑过渡效果。
- **动画**: @keyframes 定义动画关键帧，animation 属性设置动画的执行细节。

## CSS 响应式和媒体查询

- **媒体查询**: 使用 @media 规则根据不同媒体类型和特征应用不同的样式。
- **视口单位**: vw, vh, vmin, vmax 随视口大小变化的长度单位。
- **百分比单位**: 使用百分比单位创建相对于父元素的大小或位置。

## CSS 预处理器和后处理器

- **Sass**: 提供变量、嵌套、混合等高级功能。
- **Less**: 类似 Sass，但有自己的语法和特性。
- **PostCSS**: 使用 JavaScript 插件转换 CSS 代码，实现自动前缀、未来 CSS 语法等。
