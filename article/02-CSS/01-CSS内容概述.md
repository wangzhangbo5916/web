# CSS 内容概述

## CSS 选择器

- **元素选择器**: 直接通过 HTML 元素名称选择，如 div, p, h1 等。
- **类选择器**: 通过元素的 class 属性选择，使用.标识，如 .classname。
- **ID 选择器**: 通过元素的 ID 选择，使用#标识，如 #idname。
- **属性选择器**: 根据元素属性选择，如 [type="text"]。
- **伪类选择器**: 选择处于特定状态的元素，如 :hover, :focus。
- **伪元素选择器**: 选择元素的特定部分，如 ::before, ::after。
- **组合选择器**: 使用组合器组合多个选择器，如后代选择器（空格）、子选择器（>）、相邻兄弟选择器（+）和通用兄弟选择器（~）。

### CSS 属性选择器

CSS 属性选择器是一种特殊类型的选择器，它可以根据元素的属性及其值来选择元素。以下是常用的 CSS 属性选择器：

1. 存在和值属性选择器
   - [attr]：选择包含 attr 属性的所有元素，不论属性值为何。
2. 精确值属性选择器
   - [attr="value"]：选择 attr 属性的值完全等于 value 的所有元素。
3. 部分值属性选择器
   - [attr*="value"]：选择 attr 属性的值中包含子字符串 value 的所有元素。
   - [attr^="value"]：选择 attr 属性的值以 value 开头的所有元素。
   - [attr$="value"]：选择 attr 属性的值以 value 结尾的所有元素。
4. 语言子代码属性选择器
   - [attr|="value"]：选择 attr 属性的值是 value 或以 value-开头的所有元素（常用于语言代码选择，如[lang|="en"]）。
5. 包含在空格分隔的列表中的值属性选择器
   - [attr~="value"]：选择 attr 属性的值是一个空格分隔的列表，其中一个列表项正好等于 value 的所有元素。
6. 包含在连字符分隔的列表中的值属性选择器
   - [attr-="value"]：这是一个较少见的属性选择器，它会选择 attr 属性的值是一个连字符分隔的列表，其中一个列表项正好等于 value 的所有元素。
7. 大小写不敏感的属性选择器
   - [attr="value" i]：选择 attr 属性的值等于 value，不区分大小写的所有元素。
8. 大小写敏感的属性选择器
   - [attr="value" s]：选择 attr 属性的值等于 value，区分大小写的所有元素。

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

CSS 盒模型是 CSS 中的基础概念，用来描述 HTML 文档中元素的布局和设计。一个元素的盒模型由以下几个部分组成：

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

##### IE 盒模型（Border-Box）

- 特点: width 和 height 属性包括内容、内边距和边框。
- 计算总宽高: 总宽度 = 左右 margin + width（已包含 padding 和 border）
- 计算总宽高: 总高度 = 上下 margin + height（已包含 padding 和 border）

### CSS 定位方式

CSS 定位是布局中的一个重要概念，它决定了一个元素在页面中的位置。以下是 CSS 中的定位方式：

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

### Flexbox 属性

Flexbox（Flexible Box）布局提供了一种更加有效的方式来布局、对齐和分配容器内项目的空间，即使它们的大小未知或是动态变化的。

#### 容器属性（Flex Container）

display

- 属性值: flex | inline-flex
- 使用场景: 用于定义一个 Flex 容器。

flex-direction

- 属性值: row | row-reverse | column | column-reverse
  - row：**默认值**。子元素将水平排列，起始点为容器的起始边缘。
  - row-reverse：子元素将水平排列，起始点为容器的结束边缘，顺序被反转。
  - column：子元素将垂直排列，起始点为容器的起始边缘。
  - column-reverse：子元素将垂直排列，起始点为容器的结束边缘，顺序被反转。
- 使用场景: 用于定义项目的排列方向。

flex-wrap

- 属性值: nowrap | wrap | wrap-reverse
  - nowrap：默认值。flex 项不会换行，即使它们溢出了容器，也会被压缩在一行内。
  - wrap：flex 项会在必要时换行。第一行在容器的顶部，新的一行堆叠在下面。
  - wrap-reverse：flex 项会在必要时换行，但是它们的顺序是相反的：第一行在容器的底部，新的一行堆叠在上面。
- 使用场景: 当项目溢出容器时，用于定义是否换行。

justify-content

- 属性值: flex-start | flex-end | center | space-between | space-around | space-evenly
  - flex-start：flex 项被挤到行的起始位置。
  - flex-end：flex 项被挤到行的结束位置。
  - center：flex 项在行中居中对齐。
  - space-between：flex 项平均分布在行里，第一个项在起始位置，最后一个在结束位置，其它项分布在它们之间。
  - space-around：flex 项平均分布在行里，但所有项两侧都有相同的空间。这意味着项之间的空间是项与容器边缘空间的两倍。
  - space-evenly：flex 项平均分布在行里，所有项之间的空间以及与容器边缘的空间都相等。
- 使用场景: 定义项目在主轴上的对齐方式。

align-items

- 属性值: stretch | flex-start | flex-end | center | baseline
  - stretch：默认值。flex 项被拉伸以填满容器的交叉轴，但仍然遵循 min-width/min-height 和 max-width/max-height 的限制。
  - flex-start：flex 项沿着交叉轴的起始位置对齐。
  - flex-end：flex 项沿着交叉轴的结束位置对齐。
  - center：flex 项在交叉轴方向居中对齐。
  - baseline：flex 项根据它们的基线对齐。基线是指文本基线，即文本所在的虚拟线。
- 使用场景: 定义项目在交叉轴上如何对齐。

#### 项目属性（Flex Item）

flex-grow

- 属性值: \<number\> (默认值为 0)
  - 0：flex 项不会放大。这是默认值。
  - 正数：该值表示放大比例。如果所有 flex 项的 flex-grow 属性都设置为 1，则它们将等分剩余空间。如果一个 flex 项的 flex-grow 属性值大于 1，那么这个项将占据比其它项更多的剩余空间。
- 使用场景: 定义项目的放大比例。

flex-shrink

- 属性值: \<number\> (默认值为 1)
  - 1：flex 项将会缩小以适应容器空间。这是默认值。
  - 0：flex 项不会缩小，即使容器空间不足。
  - 正数：该值表示缩小的比例。如果所有 flex 项的 flex-shrink 属性都设置为 1，则它们将等比例缩小。如果一个 flex 项的 flex-shrink 属性值大于 1，则这个项将缩小得比其它项更多。
- 使用场景: 定义项目的缩小比例。

flex-basis

- 属性值: \<length\> | \<percentage\> | auto (默认值为 auto)
  - auto：flex 项的大小根据它的宽度或高度（取决于主轴方向）来确定。这是默认值。
  - 长度值：可以指定一个长度值（如 px, em, %等），来设置 flex 项的初始大小。
  - 0：将 flex 项的初始大小设置为 0，然后根据 flex-grow 属性进行调整。
- 使用场景: 定义项目在分配多余空间之前的默认大小。

flex

- 属性值: none | \<flex-grow\> \<flex-shrink\> \<flex-basis\>
- 使用场景: flex 是 flex-grow, flex-shrink 和 flex-basis 的简写，默认值为 0 1 auto。

align-self

- 属性值: auto | flex-start | flex-end | center | baseline | stretch
  - auto：flex 项的对齐方式继承自容器的 align-items 属性。这是默认值
  - stretch：flex 项被拉伸填满交叉轴的整个高度，除非设置了明确的高度或宽度。
  - flex-start：flex 项沿交叉轴的起始边对齐。
  - flex-end：flex 项沿交叉轴的结束边对齐。
  - center：flex 项在交叉轴上居中对齐。
  - baseline：flex 项根据它们的基线对齐。
- 使用场景: 允许单个项目与其他项目不同的对齐方式。

### Grid 属性

CSS Grid Layout（网格布局）是一个二维布局系统，它可以处理列和行，下面是 Grid 布局中常用的属性。

#### 容器属性（Grid Container）

grid-template-columns 和 grid-template-rows

- 属性值: \<track-size\> | \<line-name\> | repeat() | auto-fill | auto-fit
  - \<track-size\>：可以是长度、百分比或者fr单位的值，定义网格轨道的大小。
  - \<line-name\>：可以为网格线指定一个名字，方便后续引用。
  - repeat(\<number\>, \<track-size\>)：重复生成指定数量的轨道，轨道大小由\<track-size\>定义。
  - auto-fill：自动填充轨道，直到容器不能放置更多的轨道。
  - auto-fit：类似auto-fill，但是会收缩空闲的轨道。
- 使用场景: 用于定义网格的列和行的大小。可以设置固定尺寸（如 px, em, %），也可以使用 fr 单位表示比例关系。repeat()函数用于重复相同大小的轨道，auto-fill 和 auto-fit 用于自动填充网格容器。

grid-template-areas

- 属性值: \<grid-area-name\> | . | none
  - \<grid-area-name\>：指定一个区域的名称，用于布局。
  - .：表示一个空单元格。
  - none：没有定义网格区域。
- 使用场景: 通过引用 grid-area 属性所命名的区域来定义网格模板。可以创建复杂的布局模式，并且使布局易于理解。

grid-column-gap 和 grid-row-gap（已更新为 column-gap 和 row-gap）

- 属性值: \<length\>
  - \<length\>：设置网格线之间的间隙。
- 使用场景: 设置网格线之间的间隙。这可以用于在网格项之间添加空间，而无需在网格项本身上添加边距。

grid-gap（已更新为 gap）

- 属性值: \<grid-row-gap\> \<grid-column-gap\>
- 使用场景: 同时设置行和列间隙的简写形式。

justify-items 和 align-items

- 属性值: start | end | center | stretch
  - start：将网格项对齐到其所在列的起始边缘。
  - end：将网格项对齐到其所在列的末尾边缘。
  - center：将网格项对齐到其所在列的中心。
  - stretch：拉伸网格项以填满列宽。
- 使用场景: 控制网格项在网格区域内沿着行（水平）和列（垂直）的对齐方式。
  justify-content 和 align-content
- 属性值: start | end | center | stretch | space-around | space-between | space-evenly
- 使用场景: 当网格容器比其网格大时，用于对齐整个网格内容。

grid-auto-columns 和 grid-auto-rows

- 属性值: \<auto-track-size\>
  - auto：自动大小。
  - \<length\>：固定大小。
  - min-content：内容的最小大小。
  - max-content：内容的最大大小。
  - minmax(min, max)：介于最小值和最大值之间的尺寸。
- 使用场景: 为隐式网格（即未通过 grid-template-rows 或 grid-template-columns 显式定义的行或列）设置大小。

grid-auto-flow

- 属性值: row | column | dense
  - auto：自动大小。
  - \<length\>：固定大小。
  - min-content：内容的最小大小。
  - max-content：内容的最大大小。
  - minmax(min, max)：介于最小值和最大值之间的尺寸。
- 使用场景: 控制自动放置网格项的算法，即如何填充网格自动创建的额外行或列。

#### 项目属性（Grid Item）

grid-column-start, grid-column-end, grid-row-start 和 grid-row-end

- 属性值: \<line\> | \<span\> | auto
- 使用场景: 定义网格项的位置和跨越的网格线。可以用来精确地放置网格项。

grid-column 和 grid-row

- 属性值: \<start-line\> / \<end-line\>
- 使用场景: grid-column 和 grid-row 是 grid-column-start/grid-column-end 和 grid-row-start/grid-row-end 的简写形式，用于定义网格项如何跨越行或列。

grid-area

- 属性值: \<name\> | \<row-start\> / \<column-start\> / \<row-end\> / \<column-end\>
- 使用场景: 既可以用来为网格项命名，以便在 grid-template-areas 中引用，也可以定义网格项的位置和大小。

justify-self 和 align-self

- 属性值: start | end | center | stretch
- 使用场景: 控制单个网格项在其网格区域内沿行（水平）和列（垂直）的对齐方式。

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

## CSS 函数和变量

CSS 函数用于在应用样式时执行一些特定的操作或计算。以下是一些常见的 CSS 函数：

- calc(): 进行基本的数学运算，如加、减、乘、除。

  ```css
  div {
    width: calc(100% - 20px);
  }
  ```

- var(): 应用 CSS 变量的值。

  ```css
  :root {
    --main-color: #06c;
  }
  div {
    color: var(--main-color);
  }
  ```

- attr(): 获取元素的属性值并在样式中使用。

  ```css
  a::after {
    content: ' (' attr(href) ')';
  }
  ```

- rgb(), rgba(): 定义颜色值，rgba()还包含透明度设置。

  ```css
  div {
    background-color: rgba(255, 0, 0, 0.5);
  }
  ```

- hsl(), hsla(): 以色相、饱和度和亮度（可选透明度）的方式定义颜色。

  ```css
  div {
    color: hsla(120, 100%, 75%, 0.3);
  }
  ```

- filter: 应用一系列的图像效果如模糊或颜色偏移。

  ```css
  img {
    filter: blur(5px) brightness(0.8);
  }
  ```

- url(): 引入外部资源，如背景图片。

  ```css
  div {
    background-image: url('image.jpg');
  }
  ```

CSS 变量，也称为自定义属性，允许你存储一个值，然后在整个文档中重复使用该值。

- 声明变量：通常在:root 伪类中声明，以便全局使用。

  ```css
  img {
    filter: blur(5px) brightness(0.8);
  }
  ```

- 使用变量：通过 var()函数引用变量。

  ```css
  :root {
    --main-bg-color: #9c88ff;
  }
  ```

- 变量回退值：当变量未定义时，可以设置一个回退值。

  ```css
  div {
    color: var(--text-color, black);
  }
  ```

## CSS 预处理器和后处理器

- **Sass**: 提供变量、嵌套、混合等高级功能。
- **Less**: 类似 Sass，但有自己的语法和特性。
- **PostCSS**: 使用 JavaScript 插件转换 CSS 代码，实现自动前缀、未来 CSS 语法等。
