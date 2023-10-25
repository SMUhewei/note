### css选择器的优先级

```
!important > 内联样式（行内样式）> id选择器 > 类选择器 > 标签选择器 > 通配符选择器
```

样式表的来源：

1.行内样式：

```Css
<p style="color:pink;">行内样式</p>
```

2.内部样式：

```css
<style>
	div {
      color:pink;
	}
</style>
```

3.外部样式：

```Css
<link rel="stylesheet" href="style.css">
```

### display的属性值

| 属性值          | 作用                             |
| ------------ | ------------------------------ |
| none         | 元素不显示，并且会从文档流中移出               |
| block        | 块类型。默认宽高为父元素宽度，可设置宽高，换行显示。     |
| inline       | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示   |
| inline-block | 行内块元素类型。默认宽度为内容宽度，可以设置宽高，同行显示。 |
| list-item    | 像块类型元素一样显示，并添加样式列表标记。          |
| table        | 此元素会作为块级表格来显示。                 |
| inherit      | 规定应该从父元素继承display属性的值          |

### 块级元素

- 独占一行，元素的宽高，margin和padding都可以自己设置

### 行内元素

- 设置宽高无效
- 可以设置水平方向上的margin和padding属性，不能设置垂直方向的padding和margin
- 不会自动换行

### 行内块元素

- 行内块元素的特点 + 可以设置宽高

### 隐藏元素的方法

`dispaly:none`：display：none会参与构建DOM树，但是不会参与构建渲染树，所以设置为display：none的元素不会在页面中占据位置，也不会响应绑定的监听事件。

`visibility:hidden`：元素在页面中占据空间，但是不会响应绑定的监听事件。

`opacity:0`:将元素的透明度设置为0，以此来实现元素的隐藏。元素在页面中占有空间，且能够响应元素绑定的事件。

### 盒模型

盒模型有四个部分构成：分别是margin，border，padding和content

标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同：

- 标准盒模型的width和height范围只包含了content
- IE盒模型的width和height属性的范围包含了border，padding和content。

可以通过修改元素的box-sizing属性来改变元素的盒模型：

- `box-sizing: content-box`来表示标准盒模型（默认值）
- `box-sizing: border-box`表示IE盒模型（怪异盒模型）

### 实现一个三角形

```
#使用border属性可以实现一个三角形
div {
  width:0;
  height:0;
  border-top:50px solid red;
  border-right:50px solid transparent;
  border-left:50px solid transparent;
}
```

### 实现一个圆形

```
div {
  width:100px;
  height:100px;
  border-radius:50%;
  background-color:red;
}
```

### 如何清除浮动

1.给父盒子一个高度

2.在浮动元素之后添加一个空标签，并添加clear：both样式

3.父元素添加overflow:hidden

4.使用伪元素

```
.box::after{
  content:'';
  display:block;
  clear:both
}
.clearfix{
  *zoom:1
}
```

### BFC

块级格式化上下文（Block Formatting Content，BFC）是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其它元素的交互限定区域。

通俗来讲，BFC就是一块被隔离的区域，也可以把它理解为容器，且在这个容器中的子元素不会对外面的元素产生影响。

创建BFC的条件：

- 元素设置浮动：float除none以外的值
- 元素设置绝对定位：position（absolute，fixed）
- display值为：inline-block
- overflow值为：**hidden**,auto,scroll

BFC的特点：

- 垂直方向上，自上而下排列，和文档流的排列方式一致
- 在BFC中上下相邻的两个容器的margin会重叠（Box垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的margin会发生重叠)
- 计算BFC的高度时，需要计算浮动元的高度
- BFC区域不会与浮动的容器发生重叠
- BFC时独立的容器，容器内部元素不会影响外部元素
- 每个元素的左margin值和容器的左border相接触

BFC的作用：

解决margin的重叠问题：由于BFC是一个独立的区域，内部的元素和外部的元素互不影响，将两个元素变为两个BFC,就解决了margin重叠的问题

解决高度塌陷的问题：在对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为0。解决这个问题，只需要把父元素变成一个BFC。常用的办法是给父元素设置`overflow:hidden`

创建自适应两栏布局：可以用来创建自适应两栏布局：左边的宽度固定，右边的宽度自适应。左边设置为`float:left`,右侧设置`overflow:hidden`。这样右边就触发了BFC，BFC的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠，实现了自适应两栏布局。

### position的属性

| 属性值   | 概述                                                         |
| -------- | ------------------------------------------------------------ |
| absolute | 生成绝对定位的元素。相对于已定位的父元素进行定位。           |
| relative | 生成相对定位的元素。相对与其原来的位置进行定位。             |
| fixed    | 生成绝对定位的元素。指定元素相对于屏幕视口（viewport)的位置来指定元素的位置。 |
| static   | 默认值，没有定位，元素出现在正常的文档流中。                 |
| inherit  | 规定从父元素继承position属性的值。                           |





### 盒子水平垂直居中的方法

1.position+transform的translate

left属性：定义了**定位元素**左外边距与其包含块左边界之间的偏移

2.使用flex布局

首先父元素通过`display：flex`开启flex布局

然后将主轴上的项目居中排列：`justify-content:center`

最后将交叉轴的项目居中排列：`align-item:center`

### 文字垂直水平居中

首先设置行高和元素一样高`line-height:xxpx`

然后设置文字水平居中`text-align:center`

最后设置文字垂直方向居中对齐`vertical-align:middle`

### flex

传统的页面布局依赖于display，position，float属性，但是这种布局方式对一些特殊的布局非常不方便，比如说一个盒子水平垂直居中对齐的布局。

所以为了解决一个父容器和多个子元素的布局问题，flex布局应运而生。

flex即flexible box弹性布局，采用flex布局的元素，称为容器。容器中的子元素称为项目。

容器的属性：

1.`flex-direction`决定了主轴的方向

2.`justify-content`定义了项目在主轴上的对齐方式

3.`align-items`定义了项目在交叉轴上如何对齐

4.`flex-wrap`决定一条轴排不了，如何换行

项目的属性：

1.`flex-grow`定义项目的放大比例