# CSS编码规范
## 1 前言


本文档的目标是使 CSS 代码风格保持一致，容易被理解和被维护。

虽然本文档是针对 CSS 设计的，但是在使用各种 CSS 的预编译器(如 less、sass、stylus 等)时，适用的部分也应尽量遵循本文档的约定。
## 2 代码风格


### 2.1 文件


#### [建议] `CSS` 文件使用`UTF-8` 编码。

解释：

UTF-8 编码具有更广泛的适应性。

### 2.2 缩进


#### [强制] 使用 `2` 个空格做为一个缩进层级，不允许使用 `4` 个空格 或 `tab` 字符。


示例：

```css
.selector {
  margin: 0;
  padding: 0;
}
```

### 2.3 空格


#### [强制] `选择器` 与 `{` 之间必须包含空格。

示例：

```css
.selector {
}
```

#### [强制] `属性名` 与之后的 `:` 之间不允许包含空格， `:` 与 `属性值` 之间必须包含空格。

示例：

```css
margin: 0;
```
#### [强制] `列表型属性值` 书写在单行时，`,` 后必须跟一个空格。

示例：

```css
font-family: Arial, sans-serif;
```
### 2.4 行长度


#### [强制] 每行不得超过 `120` 个字符，除非单行不可分割。

解释：

常见不可分割的场景为URL超长。


#### [建议] 对于超长的样式，在样式值的 `空格` 处或 `,` 后换行，建议按逻辑分组。

示例：

```css
/* 不同属性值按逻辑分组 */
background:
  transparent url(aVeryVeryVeryLongUrlIsPlacedHere)
  no-repeat 0 0;

/* 可重复多次的属性，每次重复一行 */
background-image:
  url(aVeryVeryVeryLongUrlIsPlacedHere)
  url(anotherVeryVeryVeryLongUrlIsPlacedHere);

/* 类似函数的属性值可以根据函数调用的缩进进行 */
background-image: -webkit-gradient(
  linear,
  left bottom,
  left top,
  color-stop(0.04, rgb(88,94,124)),
  color-stop(0.52, rgb(115,123,162))
);
```

### 2.5 选择器


#### [建议] 当一个 rule 包含多个 selector 时，每个选择器声明独占一行。

示例：

```css
/* good */
.post,
.page,
.comment {
  line-height: 1.5;
}

/* bad */
.post, .page, .comment {
  line-height: 1.5;
}
```
#### [强制] `>`、`+`、`~` 选择器的两边各保留一个空格。

示例：

```css
/* good */
main > nav {
  padding: 10px;
}

label + input {
  margin-left: 5px;
}

input:checked ~ button {
  background-color: #69C;
}

/* bad */
main>nav {
  padding: 10px;
}

label+input {
  margin-left: 5px;
}

input:checked~button {
  background-color: #69C;
}
```

#### [强制] 属性选择器中的值必须用双引号包围。

解释：

不允许使用单引号，不允许不使用引号。


示例：

```css
/* good */
article[character="juliet"] {
  voice-family: "Vivien Leigh", victoria, female;
}

/* bad */
article[character='juliet'] {
  voice-family: "Vivien Leigh", victoria, female;
}
```

### 2.6 属性


#### [强制] 属性定义必须另起一行。

示例：

```css
/* good */
.selector {
  margin: 0;
  padding: 0;
}

/* bad */
.selector { margin: 0; padding: 0; }
```

#### [强制] 属性定义后必须以分号结尾。

示例：

```css
/* good */
.selector {
  margin: 0;
}

/* bad */
.selector {
  margin: 0
}
```





## 3 通用




### 3.1 选择器


#### [强制] 如无必要，不得为 `id`、`class` 选择器添加类型选择器进行限定。

解释：

在性能和维护性上，都有一定的影响。


示例：


```css
/* good */
#error,
.danger-message {
  font-color: #c00;
}

/* bad */
dialog#error,
p.danger-message {
  font-color: #c00;
}
```

#### [建议] 选择器的嵌套层级应不大于 `3` 级，位置靠后的限定条件应尽可能精确。

示例：

```css
/* good */
#username input {}
.comment .avatar {}

/* bad */
.page .header .login #username input {}
.comment div * {}
```



### 3.2 属性缩写



#### [建议] 在可以使用缩写的情况下，尽量使用属性缩写。

示例：

```css
/* good */
.post {
  font: 12px/1.5 arial, sans-serif;
}

/* bad */
.post {
  font-family: arial, sans-serif;
  font-size: 12px;
  line-height: 1.5;
}
```

#### [建议] 使用 `border` / `margin` / `padding` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。

解释：

`border` / `margin` / `padding` 等缩写会同时设置多个属性的值，容易覆盖不需要覆盖的设定。如某些方向需要继承其他声明的值，则应该分开设置。


示例：

```css
/* centering <article class="page"> horizontally and highlight featured ones */
article {
  margin: 5px;
  border: 1px solid #999;
}

/* good */
.page {
  margin-right: auto;
  margin-left: auto;
}

.featured {
  border-color: #69c;
}

/* bad */
.page {
  margin: 5px auto; /* introducing redundancy */
}

.featured {
  border: 1px solid #69c; /* introducing redundancy */
}
```


### 3.3 属性书写顺序


#### [建议] 同一 rule set 下的属性在书写时，应按功能进行分组，并以 **Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果）** 的顺序书写，以提高代码的可读性。

解释：

- Formatting Model 相关属性包括：`position` / `top` / `right` / `bottom` / `left` / `float` / `display` / `overflow` 等
- Box Model 相关属性包括：`border` / `margin` / `padding` / `width` / `height` 等
- Typographic 相关属性包括：`font` / `line-height` / `text-align` / `word-wrap` 等
- Visual 相关属性包括：`background` / `color` / `transition` / `list-style` 等

另外，如果包含 `content` 属性，应放在最前面。


示例：

```css
.sidebar {
  /* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
  position: absolute;
  top: 50px;
  left: 0;
  overflow-x: hidden;

  /* box model: sizes / margins / paddings / borders / ...  */
  width: 200px;
  padding: 5px;
  border: 1px solid #ddd;

  /* typographic: font / aligns / text styles / ... */
  font-size: 14px;
  line-height: 20px;

  /* visual: colors / shadows / gradients / ... */
  background: #f5f5f5;
  color: #333;
  -webkit-transition: color 1s;
    -moz-transition: color 1s;
      transition: color 1s;
}
```


## 4 值与单位


### 4.1 文本


#### [强制] 文本内容必须用双引号包围。

解释：

文本类型的内容可能在选择器、属性值等内容中。


示例：

```css
/* good */
html[lang|="zh"] q:before {
  font-family: "Microsoft YaHei", sans-serif;
  content: "“";
}

html[lang|="zh"] q:after {
  font-family: "Microsoft YaHei", sans-serif;
  content: "”";
}

/* bad */
html[lang|=zh] q:before {
  font-family: 'Microsoft YaHei', sans-serif;
  content: '“';
}

html[lang|=zh] q:after {
  font-family: "Microsoft YaHei", sans-serif;
  content: "”";
}
```

### 4.2 数值


#### [建议] 当数值为 0 - 1 之间的小数时，省略整数部分的 `0`。

示例：

```css
/* good */
panel {
  opacity: .8;
}

/* bad */
panel {
  opacity: 0.8;
}
```

### 4.3 url()


#### [建议] `url()` 函数中的路径不加引号。

示例：

```css
body {
  background: url(bg.png);
}
```


#### [建议] `url()` 函数中的绝对路径可省去协议名。


示例：

```css
body {
  background: url(//baidu.com/img/bg.png) no-repeat 0 0;
}
```


### 4.4 长度


#### [强制] 长度为 `0` 时须省略单位。 (也只有长度单位可省)

示例：

```css
/* good */
body {
  padding: 0 5px;
}

/* bad */
body {
  padding: 0px 5px;
}
```


### 4.5 颜色


#### [建议] RGB颜色值使用十六进制记号形式 `#rrggbb`。

解释：

带有alpha的颜色信息可以使用 `rgba()`。使用 `rgba()` 时每个逗号后必须保留一个空格。


示例：

```css
/* good */
.success {
  box-shadow: 0 0 2px rgba(0, 128, 0, .3);
  border-color: #008000;
}

/* bad */
.success {
  box-shadow: 0 0 2px rgba(0,128,0,.3);
  border-color: rgb(0, 128, 0);
}
```

#### [建议] 颜色值可以缩写时，使用缩写形式。

示例：

```css
/* good */
.success {
  background-color: #aca;
}

/* bad */
.success {
  background-color: #aaccaa;
}
```

#### [强制] 颜色值不允许使用命名色值。

示例：

```css
/* good */
.success {
  color: #90ee90;
}

/* bad */
.success {
  color: lightgreen;
}
```

#### [建议] 颜色值中的英文字符采用小写。如不用小写也需要保证同一项目内保持大小写一致。


示例：

```css
/* good */
.success {
  background-color: #aca;
  color: #90ee90;
}

/* good */
.success {
  background-color: #ACA;
  color: #90EE90;
}

/* bad */
.success {
  background-color: #ACA;
  color: #90ee90;
}
```


