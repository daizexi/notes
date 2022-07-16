# display属性

## 块元素和行内元素

### block元素

- 块元素在浏览器显示中独占一行，并且排斥其他元素与其位于同一行。（显示效果上独占一行。）

	- 常见的6种块元素

		- h1-h6
		- p
		- div
		- ul
		- ol
		- hr

- 块元素可以容纳其他行内元素或块元素。
- 块元素可以设置width属性和height属性。
- 块元素可以定义4个方向上的margin。

### inline元素

- 行内元素可以和其他元素同占一行。

	- 常见的4种行内元素

		- strong
		- em
		- a
		- span

- 行内元素可以包含其他行内元素。
- 行内元素不能设置width和height属性。
- 行内元素可以定义margin-left和margin-right，但不能定义margin-top和margin-bottom。

### inline-block元素

- 我们可以使用display: inline-block;把元素定义成行内块元素。

	- 常见的2种行内块元素

		- img
		- input

- 行内块元素可以定义width属性和height属性。
- 行内块元素可以和其他行内元素在显示效果上共占一行。
- inline-block元素之间会有一些间距。

	- 可以使用为父元素定义一个font-size: 0;来消除这些间距。

		- 如果需要在子元素种显示文字，注意需要为子元素定义font-size属性，否则会因为继承导致子元素font-size属性为0。

### table-cell元素

- table-cell类型元素具备td元素的特点。

	- 结合使用display: table-cell;和vertical-align: middle;可以使图片在父元素种垂直居中。
	- 对图片父元素使用text-align: cnter;可以使图片水平居中。

## display属性

### CSS支持使用display属性来改变元素的类型。

- display属性的7种取值

	- inline

		- 设置为行内元素。

	- block

		- 设置为块元素。

	- inline-block

		- 设置为行内块元素。

	- table

		- 以表格形式显示，类似于table元素。

	- table-row

		- 以表格行形式显示，类似于tr元素。

	- table-cell

		- 以表格单元格形式显示，类似于td元素。

	- none

		- 隐藏元素。

## display:none;

### 我们可以使用display:none;来隐藏一个元素。

- 使用dispaly:none;隐藏的元素不再占据空间。
- 不要使用idsplay:none;来隐藏一些对SEO（搜索引擎优化）关键的元素。

### display:none;和visiblity: hidden;的区别

- display:none;使得元素被隐藏，并且元素不再占据之前的位置。
- visibility: hidden;使得元素被隐藏，但只是看不见了，元素还占据着之前的位置。

*XMind: ZEN - Trial Version*