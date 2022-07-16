# CSS重要概念

## CSS有3个重要的概念

### 包含块（containing block）

### BFC和IFC

### 层叠上下文

## 包含块（containing block）

### 如果有2个div，其中一个是父元素，另外一个是子元素，那么父元素会决定子元素的大小和定位。

- 包含块简单的说，就是可以决定一个元素大小和定位的元素。

	- 一般情况下，一个元素的包含块是离它最近的祖先元素的content区。
	- 当一个元素设置为position: absolute;时，这个元素的包含块是离它最近的设置了position: relative;或position: absolute;或position: fixed;的元素的content区。

### 包含块的影响

- 元素的尺寸及位置，常常会受到它的包含块的影响。

	- 对于一些属性，如width、height、padding、margin、绝对定位元素的偏移值，当我们对这些属性赋予百分比值时，就是通过包含块来计算的。

### 包含块的判定

- 根元素

	- 根元素（html元素），是一个页面中最顶端的元素，根元素的包含块称为初始包含块。

- 固定定位元素

	- 如果一个元素的position值是fixed，那么它的包含块是当前浏览器可视窗口。

- 静态定位元素和相对定位元素

	- 当一个元素的position值是static或relative，那么它的包含块由离它最近的祖先块元素的内容区的边缘组成。

- 绝对定位元素

	- 当一个元素的position值是absolute，那么它的包含块由离它最近的position值不是static的祖先元素的padding区的边缘组成。

## 层叠上下文（stacking context）

### 层叠上下文

- 层叠上下文，是HTML中的一个三维概念，从z-index可以看出，网页实际上是一个三维结构。

	- 层叠上下文和块级格式上下文（BFC）一样，是可以被创建出来的。

		- 在CSS 2.1中，有2个条件可以创建层叠上下文。

			- 根元素
			- z-index值不为auto的定位元素。（因为在CSS 2.1中，只有定位元素才能设置z-index）。

### 层叠级别（stacking level）

- 层叠级别用来决定同一个层叠上下文中，背景色和内部元素的层叠顺序。

	- 层叠级别从低到高有7个

		- 边框和背景
		- 负z-index的元素
		- 块盒子元素
		- 浮动盒子元素
		- 行内盒子元素
		- z-index值为0的元素
		- 正z-index的元素

## BFC和IFC

### 在CSS中，一个元素可以看成是一个盒子。

- 在普通文档流中，盒子会参与一种格式上下文（formatting context）。这个盒子可能是块级盒子，也可能是行内盒子。（一个盒子只能是块盒子或行内盒子的一种）。

	- 块级盒子参与BFC（块级格式上下文）。
	- 行内盒子参与IFC（行级格式上下文）。

### 格式上下文

- 格式上下文是CSS 2.1规范中的一个重要概念，它指的是页面中的一块渲染区域，格式上下文决定了其内部元素将如何定位，以及和其他元素的关系。

	- 格式上下文有2种类型

		- 块级格式上下文（Block Formatting Context）
		- 行级格式上下文（inline Formatting Context）

### 块盒子和行内盒子

- 元素类型（display属性）是block、table、list-item的元素，会生成块级盒子。
- 元素类型（display属性）是inline、inline-block、inline-table的元素，会生成行内盒子。

### 块级格式上下文

- BFC是一个独立的渲染区域，只有块级盒子参与，BFC规定了内部的块级盒子是如何布局的。
- 如何创建BFC

	- 具有5种条件的任一种的元素将会触发创建一个新的BFC。

		- 根元素
		- float属性为left或right的元素。
		- position属性是absolute或fixed的元素。
		- overflow属性是auto、hidden、scroll的元素。
		- 元素类型（display属性）是inline-block、table-caption、table-cell的元素。

	- 如果我们要创建一个新的BFC，为元素添加上其中一种属性就可以。
	- 因为根元素也会创建BFC，所以默认情况下，一个页面中的所有块级盒子都会在一个BFC。

- BFC的特点

	- 在一个BFC内部，盒子在垂直方向上一个接着一个的排列。
	- 在一个BFC内部，相邻的margin-top和margin-bottom会叠加。
	- 计算一个BFC高度时，它内部的浮动元素也会计算进去。

		- 这个特点使得创建一个新的BFC可以清除浮动（清除浮动就是消除浮动脱离正常文档流带来的影响，如父元素高度塌陷）。

	- 在一个BFC内部，如果存在一个内部元素也是一个BFC，且存在外部BFC的一个内部元素是一个浮动元素，那么内部BFC不会和float元素区域重叠。
	- 在一个BFC内部，每一个元素的margin-left会紧贴着包含盒子的容器的border-left，即使存在浮动也会遵循。

- BFC的用途

	- 创建BFC来避免垂直外边距叠加。
	- 创建BFC来清除浮动。
	- 创建BFC实现自适应布局。

*XMind: ZEN - Trial Version*