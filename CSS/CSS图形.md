# CSS图形

## 三角形

### 我们知道，CSS中元素都是由盒子组成的，当盒子的border相交时，浏览器会在这个相交处绘制一条接合线。

- CSS实现三角形就利用了这个接合线，把一个元素的width和height设置为0，然后设置border-width为较大的值，然后把任选三条或两条边框的颜色设置为transparent。

	- 实际上是隐藏了部分border来形成三角形效果。

## 带边框的三角形

### 我们一般使用2个三角形来实现带边框的三角形。

- 上层三角形相对于下层三角形定位布局。

	- 下层三角形设置position: relative;。
	- 上层三角形设置position: absolute;。
	- 一般来说，上层三角形的border-width比下层三角形的border-width小1px。

- 要注意，子元素是相对于父元素的content内容的边界来定位的。（由于这时父元素的width和height属性都设置为0，所以子元素是相对于父元素的中心点定位的）。

## border-radius

### CSS3定义了border-radius属性来实现圆角。

- border-radius的取值是一个长度值，单位可以是

	- px
	- em
	- 百分比

- 一个例子是，border-radius: 20px;是指四个角的圆角半径是20px。

### 使用border-radius还可以实现半圆和圆。

- 实现半圆

	- 把height设置为width的一半，再把左上角和右上角的圆角定义为和height一样，左下角和右下角定义为0。
	- 实际上，它的原理是border-radius设置的是圆角的半径。

- 实现圆

	- 把height和width设置为相等的值，然后把四个角的border-radius设置为height的一半。

### border-radius属性也可以实现椭圆

- border-radius: x/y;

	- x是圆角的水平半径，y是圆角的垂直半径。

		- 我们可以把height和width设置为不同的值，把水平半径x设置为width的一半，把垂直半径y设置为height的一半。

*XMind: ZEN - Trial Version*