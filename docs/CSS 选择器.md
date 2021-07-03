# CSS 选择器



选择器决定了样式规则对哪些元素有效。



## BEM命名法

BEM是Block（区块）、Element（元素）、Modifier（修饰符）三者的简称。区块是顶级组件的抽象，元素是组件的组成部分，修饰符是组件或元素的状态。区块与元素之间用两个下划线连接，元素与修饰符之间用两个连词线连接。

```css
/* Block component */
.btn {}

/* Element that depends upon the block */
.btn__price {}

/* Modifier that changes the style of the block */
.btn--orange {}
.btn--big {}
```

对应的HTML代码结构如下。

```html
<a class="btn btn--big btn--orange" href="http://css-tricks.com">
  <span class="btn__price">$9.99</span>
  <span class="btn__text">Subscribe</span>
</a>
```

BEM的重要特点就是CSS是扁平式的，不存在元素嵌套。



## 基本选择器

元素名称选择器，选中所有该标签的元素。

`#elementId`选中指定`id`属性的元素。

`.elementClass`选中指定`class`属性的元素。

应该尽量使用 class 选择器，而不是 ID 选择器。

ID 选择器的样式不能在其他元素上复用（记住，在一个页面中，一个id只能出现在一个元素上）。这会导致在其他元素上重复样式，而不是通过class共享样式。

ID 选择器的特殊性比class选择器要强得多。这意味着如果要覆盖使用id选择器定义的样式，就要编写特殊性更强的CSS规则。如果数量不多，可能还不难管理。如果处理规模较大的网站，其CSS就会变得比实际所需的更长、更复杂。



## 通配符选择器

通配符`*`（星号）匹配任何元素。

因为匹配范围太广，会让浏览器加载页面变慢，因此应该谨慎使用通配符。实际适合使用通配符的情况比较少。



## :matches()

`:matches(A, B)`选择器表示匹配A或B。

```css
:matches(.foo, .bar) {
  background-color: green;
}

/* 等同于 */

.foo, .bar {
  background-color: green;
}
```

它可以简化一些选择器的写法。

```css
.syntax-highlighted .css-keyword,
.syntax-highlighted .css-tag {
  color: rgb(170, 13, 145);
}

/* 等同于 */

.syntax-highlighted :matches(.css-keyword, .css-tag) {
  color: rgb(170, 13, 145);
}
```



## :not()

`:not()`表示选中不匹配指定条件的元素。

```css
a:not(.internal) {
  color: red;
}
```

`:not()`可以采用链式写法。

```css
:not(i):not(em)

/* 等同于 */

:not(i, em)
```



## target

target选择器用来匹配当前hash。

产生动画效果。

```css
#further-resources:target {
  animation: highlight .8s ease-out;
}

@keyframes highlight {
  0% { background-color: #FFFF66; }
  100% { background-color: #FFFFFF; }
}
```

弹出效果。

```css
#search-overlay {
  position: fixed;
  top: 1em;
  bottom: 1em;
  right: 1em;
  left: 1em;
  /* … */
  opacity: 0;
  transition: opacity .3s ease-in-out;
  pointer-events: none;
}

#search-overlay:target {
  opacity: 1;
  pointer-events: auto;
  transition: opacity .3s ease-in-out;
}
```

导航栏效果

```css
.main-nav {
  position: fixed;
  top: 0;
  width: 0;
  height: 100%;
  background: #3B3B3B;
  overflow-y: auto;
  transition: width 0.3s ease;
}

#main-nav:target {
  width: 20%;
}

#main-nav:target+.page-wrap {
  width: 80%;

  .open-menu {
    display: none;
  }

  .close-menu {
    display: block;
  }

  .main-header {
    width: 80%;
    left: 20%;
  }
}
```



## 伪类选择器

### 概念

伪元素（pseudo-element）是 HTML 中并不存在的元素。例如，定义第一个字母或第一行文字时，并未在HTML中作相应的标记。

伪类（pseudo-class）是浏览器根据网页元素的状态，自动提供的 CSS 类，无需在 HTML 代码显式标记这些类。例如，使用:first-child可以选择某元素的第一个子元素，你就不用写成class="first-child"。更多关于伪类的内容。

伪元素有四个。

- ::first-line
- ::first-letter
- ::before
- ::after

伪类

- :first-child
- :link：新的、未访问的链接
- :visited：访问过的链接
- :focus：链接获得焦点（如通过Tab键）
- :hover：当访问者将鼠标指针停留在链接上时
- :active: 当访问者激活链接时
- :empty：空选择器

新的、未访问的链接显示为红色；访问过的链接变为橙色；

### 总述

CSS 2.1非常有限，只提供了三个伪类。

- :active
- :focus
- :hover

CSS Selector Level 3添加了更多与HTML表单相关的伪类：

- :enabled
- :disabled
- :checked
- :indeterminate

CSS Basic UI Level 3添加了许多伪类来描述窗口小部件的状态：

- :default
- :valid
- :invalid
- :in-range
- :out-of-range
- :required
- :optional
- :read-only
- :read-write

### :first-child，:last-child

`:first-child`表示一组元素的第一个元素，且该元素必须是父元素的第一个子元素。

```css
li{
  color: blue;
}

li:first-child {
  color: green;
}
```

上面代码中，第一个`li`的字体颜色为绿色，其他`li`都为蓝色。

`:last-child`表示一组元素之中的最后一个，且该元素必须是父元素的最后一个子元素。

```css
li:last-child {
  background-color: lime;
}
```

### :first-of-type，:last-of-type

`:first-of-type`选中一组元素之中的第一个元素。

```css
p:first-of-type {
  font-size: 1.25em;
}
```

上面代码中，`p:first-of-type`只会选中第一个`p`元素。

`:first-of-type`与`:first-child`的区别是，后者必须父元素的第一个子元素。

```html
<ul>
  <p>Hello World</p>
  <li>1</li>
  <li>2</li>
</ul>
```

上面这段 HTML 代码，如果想让第一个`<li>`显示为绿色，必须使用`:first-of-type`，而不能是`:first-child`。

```css
/* 正确 */
ul li:first-of-type {
  color: green;
}

/* 错误 */
ul li:first-child {
  color: green;
}
```

`:last-of-type`选中本类之中最后一个元素。

```css
p:last-of-type {
  font-size: 0.75em;
}
```

上面代码中，`p:last-of-type`只会选中最后一个`p`元素。

### :nth-child()，:nth-last-child()

`:nth-child()`选中指定位置的子元素。`:nth-last-child()`选中指定的倒数位置的子元素。

```css
p:nth-child(2) {
  color: red;
}
```

上面代码中，`:nth-child()`选中所有第二个子元素位置的`p`元素。

注意，如果元素名与`:nth-child()`之间有没有空格，含义是不一样的。没有空格时，表示匹配该种子元素；有空格时，表示匹配该元素的子元素。


```css
p :nth-child(2) {
  color: red;
}
```

上面代码表示，匹配`p`元素内部的第二个子元素，而不是匹配`p`元素本身。

`:nth-child()`和`:nth-last-child()`可以使用通配符作为参数，参见`:nth-of-type()`部分的说明。

```css
ul li:nth-child(3n+3) {
  color: #ccc;
}
```

上面代码选中3、6、9等位置的`li`元素，`n`表示整数序列0、1、2、3……。

这里还有一个技巧，如果要选中前三个元素，可以在`n`前面使用负数符号1`-`。

```css
ul li:nth-child(-n + 3) {
  color: #ccc;
}
```

上面代码只会选中前三个`li`，因为当`n`大于等于`3`时，`-n + 3`会小于等于0，而`li`的序号是从`1`开始的。

相应的，`:nth-last-child(-n + 3)`就会选中最后三个元素，而`:nth-last-child(n + 4)`会选中除了最后三个以外的其他元素。

除了使用`An + B`这种形式的表达式指定位置，还可以使用`odd`或`even`关键字，分别表示奇数位置或偶数位置的元素。

```css
ul li:nth-child(odd) {
  color: #ccc;
}
```

### :nth-of-type()，:nth-last-of-type()

`:nth-of-type()`匹配指定类型和位置的元素，`:nth-last-of-type()`匹配倒数位置的指定类型和位置的元素。

```css
p:nth-of-type(2) {
  color: red;
}
```

上面代码中，`p:nth-of-type(2)`匹配第二个`p`元素。

下面是通配符用法。

```css
li:nth-of-type(2n) {  background: lightslategrey;}li:nth-of-type(3n+2) {  background: blue;}
```

`:nth-of-type`的参数可以是`an + b`的形式。

- “a”是一个整数
- “n”作为英文字母，总是不变的，含义是“0, 1, 2, ....”
- “+”作为一个运算符，可以是“+”，也可以是“-”
- “b”是一个整数，如果提供了运算符，就必须提供“b”

`:nth-of-type`的参数还可以是`even`或`odd`。

```css
li::nth-of-type(odd)  {  background: lightslategrey;}
```

### 空选择器

`:empty`是空选择器，匹配没有任何子节点的元素。

```css
p:empty {  color: red;}
```

下面的 HTML 代码都匹配`:empty`选择器。

```html
<!-- 闭合标签之间没有空格 --><p></p><!-- 闭合标签之间只有注释 --><p><!-- comment --></p>
```

下面的 HTML 代码都不匹配`:empty`选择器。

```html
<!-- 闭合标签之间有空格 -->
<p></p>

<!-- 闭合标签之间有换行符 -->
<p>
<!-- comment -->
</p>

<!-- 闭合标签有子元素 -->
<p><span></span></p>
```

这个选择器的一个作用，就是使用脚本动态添加子元素后，将该元素显示出来。

```html
<!-- No error message -->
<div class="error"></div>

<!-- Yes error message -->
<div class="error">Missing Email</div>
```

上面代码中，为`<div>`标签添加文本内容后，使用下面的 CSS 代码将其显示出来。

```css
.error:empty {
  display: none;
}

.error:before {
  color: red;
  content: "\0274c "; /* ❌ icon */
}
```

选中非空元素可以像下面这样写。

```css
.alert:not(:empty) {
  background: pink;
  padding: 10px;
}
```

### 伪元素（Pseudo-element）

伪元素（::before或者::after）是每个元素额外多出来的DOM节点。


```css
.pebble::before {  ...}.pebble::after {  ...}
```

伪元素使用两个双引号标识，如果希望IE8支持，也可以使用单引号。

```css
button::after {
  content: '';
  position: absolute;
  top: -50%;
  right: -50%;
  bottom: -50%;
  left: -50%;
  background: linear-gradient(to bottom, rgba(229, 172, 142, 0.1), rgba(255, 255, 255, 0.5) 50%, rgba(229, 172, 142, 0.1));
  transform: rotateZ(60deg) translate(-5em, 7.5em);
}

button:hover::after {
  animation: sheen 1s forwards;
}

@keyframes sheen {
  100% {
    transform: rotateZ(60deg) translate(1em, -9em);
  }
}
```

### 伪类

- :empty：没有任何子元素
- :in-range：针对有range属性的input
- :out-of-range：针对有range属性的input
- :optional：没有required属性的input元素
- :required
- :disabled
- :fullscreen
- :not()



## 属性选择器

### 概述

- `[attribute]`	匹配指定属性，不论具体值是什么
- `[attribute="value"]`	完全匹配指定属性值
- `[attribute~="value"]`	属性值是以空格分隔的多个单词，其中有一个完全匹配指定值
- `[attribute|="value"]`	属性值以value-打头
- `[attribute^="value"]`	属性值以value开头，value为完整的单词或单词的一部分
- `[attribute$="value"]`	属性值以value结尾，value为完整的单词或单词的一部分
- `[attribute*="value"]`	属性值为指定值的子字符串

### 修饰符

属性修饰器支持`i`修饰符，表示不区分大小写。

```css
[class=foo i] {
  color: red;
}
```

上面代码中，属性名`foo`后面的`i`，表示不区分`foo`的大小写，所以下面几个 class 都会匹配。

```html
<div class="foo">...</div>
<div class="Foo">...</div>
<div class="fOo">...</div>
```

这个修饰符对于匹配用户的输入，非常有用。

```css
/**
 * 匹配：
 * <input value="hello world">
 * <input value="hello World">
 * <input value="hElLo WoRlD">
 * ...
 */
[value="hello world" i] { /* ... */ }
```



## 优先级

一个网页元素，可以被多个选择器匹配。如果这些选择器都设置了同样的规则，就有一个”优先级“的问题：到底哪个选择器的优先级更高？

规则是针对性越强的选择器，优先级越高。浏览器会计算每条规则的优先级值，然后采用值最高的规则，计算方法如下。

- `!important`后缀为 10000
- 行内样式为 1000
- ID 为 100
- class、伪类、属性选择器为 10
- 标签名、伪元素为 1

然后，累加得到的分数，哪条规则得分最高，就采用哪条规则。如果得分相同，就采用位置靠后的位置。

下面是一些例子。

- `<li style="color: red;">`：行内样式，得分为1000。
- `ul#nav li.active a`：一个 ID、一个 Class、三个标签名，得分为113。
- `body.ie7 .col_3 h2 ~ h3`：两个 Class，三个标签名，得分为23。
- `#footer *:not(nav) li`：一个 ID、两个标签名，得分为102。注意，`*`没有分数，而且`:not`不算伪类，只计算它括号里面的标签名。
- `ul > li ul li ol li:first-letter`：七个标签名，得分为7。注意，`:first-letter`算作伪元素。

