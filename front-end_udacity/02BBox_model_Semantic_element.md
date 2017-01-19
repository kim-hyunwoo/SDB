# Box model and Semantic Element

## block vs inline

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)

![block_vs_inline](https://github.com/khw1031/Assets/blob/master/udacity_frontend/block_vs_inline.png?raw=true)

```css
{
  display: block; /* 웹 브라우저 가로폭을 모두 차지, 세로폭은 content 만큼 차지 no content === no height */
  display: inline;  /* content 만큼만 가로폭을 차지(content-based), width와 height를 설정 불가, margin과 padding 값은 적용 가능하지만 가로폭으로만 적용됨 */
}
```

box-sizing으로 인해 박스의 크기가 정의되는 방식을 바꿀 수 있다.

```css
* {
  box-sizing: content-box; /* default */
  box-sizing: border-box;
}
```

1\. content-box

width = 800px;을 정의했을 때 content가 800px의 크기를 가진다. 만약 border와 padding에 50px을 추가하면 보여지는 사이즈는 900px이 된다.

2\. border-box

width = 800px;을 정의했을 때 content + border + padding의 크기가 800px이 된다.

### user-agent style

웹 브라우저는 기본적으로 user-agent style 값을 가진다. 웹 브라우저마다 조금씩 다른 기본값을 지니고 있다.

## Semantic tags

div로 도배된 코드는 사람이 이해하기 힘들뿐만 아니라 서치 엔진 같은 프로그램이 그 의미를 파악하기도 어렵다. 그래서 나온게 Semantic tag이다. 

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

### vertical-align

The important thing here is vertical-align only works on inline elements (or inline-block) and table-cells. Valid values for vertical-align include top, bottom, and middle, which work exactly like you'd expect, super and sub which can be used to superscript and subscript inline elements (hence their names), or you can use length values (e.g. px, %, em, rem, etc.) to specify a length above the baseline of its parent.

### line-height

You can specify line-height three different ways, by using a (1) number, (2) length, or (3) percentage. By default, line-height is set to normal which is rougly 1.2 (based on the elememt's font-family) multiplied by the element's font-size.

For example, if your element's font-size is set to 16px, then by default its line-height is rougly 19.2px when set to normal. You could achieve the same result by writing line-height as:

line-height: 1.2;
line-height: 19.2px;
line-height: 120%;

Here's three different ways to express line-height. These are all equivalent, assuming the element's font-size is set to 16px (1.2 multiplied by 16px = 19.2px).

### Centering Boxes

![center](http://udacity.github.io/fend/lessons/L5/problem-set/07-making-peach-ice-cream/margin-auto.gif)

If you use the special keyword auto on the left and right margins of an element, you can horizontally center the element within its container. The combination of a set width and margin: 0 auto; (this is the shorthand way of writing margin) will make the element take up the width you specify, and then the remaining horizontal space will be split "automatically" between the left and right margins

### box-sizing

```css
* {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    box-sizing: border-box;
}
```

## flex-box

[guide-css-tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

### Properties for the Parent

```css
.container {
    display: flex;
    display: inline-flex;
    /* defines a flex container. enables a flex context for all its direct children */
}
```

```css
.container {
    /* defining the direction flex items ar placed */
    flex-direction: row | row-reverse | column | column-reverse;
}
```

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
/* By default, flex items will all try to fit onto one line. 
    nowrap : single-line(default)
    wrap : multi-line(left to right)
*/
```

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![justify-content](https://css-tricks.com/wp-content/uploads/2013/04/justify-content.svg)

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![align-items](https://css-tricks.com/wp-content/uploads/2014/05/align-items.svg)

```css
.container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![align-content](https://css-tricks.com/wp-content/uploads/2013/04/align-content.svg)

### Properties for Children

```css
.item {
    order: <integer>;
}
```

![order](https://css-tricks.com/wp-content/uploads/2013/04/order-2.svg)

```css
.item {
    flex-glow: <number>; /* default 0 */
    flex-shrink: <number>;
}
```

![flex-glow](https://css-tricks.com/wp-content/uploads/2014/05/flex-grow.svg)

```css
.item {
    flex-basis: <length> | auto;
}
/*
defines the default size of an element before the remaining space is distributed
 */
```

```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
/*
allows the default alignment(or the one specified by align-items) to bee overridden for individual flex items
 */
```

![align-self](https://css-tricks.com/wp-content/uploads/2014/05/align-self.svg)




























