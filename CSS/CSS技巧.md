# CSS技巧

## 水平居中

### 文本的水平居中

- 单行文本的水平居中

	- 对元素设置text-align: center;，text-align对文本、inline元素、inline-block元素有效，对block元素无效。

### 元素的水平居中

- 块元素

	- 我们可以对块元素设置width，然后为块元素设置margin-left: auto; margin-right: auto;。

		- 等价写法是margin: 0 auto;
		- 注意必须对块元素设置width，这个方法才有效，如果不设置width，块元素会默认占满整行，这样就不能设置居中了。

- 行内元素或inline-block、inline-flex等元素

	- 我们可以使用text-align: center;来设置水平居中。

## 垂直居中

### 文本的垂直居中

- 对于单行文本

	- 我们可以设置line-height值和父元素height值相等，就可以设置单行文本在父元素中垂直居中了。

- 对于多行文本

	- 使用span元素把多行文本包含起来，再对span元素设置display: inline-block;。

		- 这样就转换为了inline-block元素在父元素中垂直居中的问题。

### 元素的垂直居中

- 对块元素设置垂直居中

	- 对于已经设置width和height的元素（父元素也已经设置width和height），我们可以使用position属性。

		- 我们对父元素设置position: relative;，对子元素设置position: absolute;，再对子元素设置top: 50%; left: 50%; margin-top设置为子元素height一半的负值，margin-left设置为子元素width一半的负值。

			- 这样我们实现了块元素在父元素中的水平垂直居中。
			- 如果只想实现垂直居中，就把margin-left属性和left属性去掉。

- 对inline-block元素垂直居中

	- 对于inline-block元素，我们可以使用display: table-cell;和veritical-align: middle;实现垂直居中。

*XMind: ZEN - Trial Version*