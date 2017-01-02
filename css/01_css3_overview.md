# CSS3 Overview

Border Radius, Box Shadow, Text Shadow, Box Sizing, Multiple Backgrounds, Color, Opacity, Gradients

## Border Radius

```css
.box {
    border-top-left-radius: 15px;
    border-top-right-radius: 15px;
    border-bottom-left-radius: 15px;
    border-bottom-right-radius: 15px;
    border-radius: 15px;
    border-radius: 4px 15px 12px 10px; /* or use % */
    /* border-radius: <top left> <top right> <bottom right> <bottom left> */
}
```

## Box Shadow
```css
.box {
    /* box-shadow: <inset> <offset-x> <offset-y> <blur-radius> <spead-radius> <color> */
    box-shadow: 1px 2px 2px #000;
    box-shadow: 1px 2px 0 2px #000;
}
```

You can specify multiple box-shadows via a comma-separated list :

```css
.box {
    box-shadow:
        1px 1px 2px #000,
        inset 1px 1px 2px blue;
}
```

You can also specify negative values :

```css
.box {
    box-shadow: -1px -2px 2px #000;
}
```

## Text shadow

```css
h1 {
    text-shadow: 1px 2px 2px #000;
    /* text-shadow: <offset-x> <offset-y> <blur-radius> <color> */
}
```

## Box-sizing

The box-sizing property is used to change the default CSS box model, which is used to calculate widths and heights of given elements.

* The CSS box model references the design and layout of given HTML elements.
* Each HTML elements is a "box", which consists of margins, borders, padding, and the content of the element.
* The "box model" refers to how those properties are calculated in conjunction with one another in order to set the element's dimensions.

```css
.box {
    margin: 20px;
    border: 2px solid black;
    padding: 10px;
    width: 300px;
}

/* the box's width(default) :
    content width + padding + border
*/
```


box-sizing :

* content-box (default) : width and height = content (324px)
* padding-box : width and height = content + padding (304px)
* border-box : width and height = content + padding + border (300px)

## Multiple backgrounds

CSS3 allows you to apply multiple backgrounds to an elements. They are stacked in the order in which you specify them.

```css
.element {
    background-image: url(bg1.png), url(bg2.png);
    background-position: top left, center right;
    background-repeat: no-repeat, no-repeat;
}

.element {
    background:
        url(bg1.png) top left no-repeat,
        url(bg2.png) center right no-repeat;
}
```

## Color

```css
.element {
    color: rgba(0,0,0,0.75);
}

.element {
    color: hsla(240, 100%, 50%, 0.75);
}
```

## Opacity

```css
.element {
    opacity: 0.45;
}
```

## Gradients

* Linear gradients : starting point, the ending point, optional stop-color points.
* Radial gradients : extends from an origin, the center of the element, extending outward in a circular or elliptical shape.

```css
/* linear 
    linear-gradient(<angle> to <aside-or-corner>, <color-stops>s)
    to top : 0deg
    to bottom : 180deg (default)
    to right : 270deg
    to left : 90deg
*/
.element {
    background: linear-gradient(to bottom, red, yellow);
    
}

/* Radial
    radial-gradient(<shape> <size> at <position>, <color-stop>s)
    shape : circle or ellipsis(default)
    size: closest-side, closest-corner, farthest-side, farthest-corner(default), px or %.
*/
.element {
    background:
        radial-gradient(aqua, blue);
    background: radial-gradient(circle at top left, aqua, blue);
}
```


## Fonts & Interactions

### Font-face

Using @font-face, we have the ability to provide online fonts for use on our websites.

```css 
@font-face {
    font-family: 'OpenSansRegular';
    src: url('OpenSansRegular-webfont.eot');
    font-style: normal;
    font-weight: normal;
}

/* Using @font-face in our stylesheet : */
h1 {
    font-familyL 'OpenSansRegular', Helvetica, Arial, sans-serif;
}

```

### transform

Using the transform property in CSS3, we can translate, rotate, scale, and skew elements in CSS.

```css 
/* 2D translation using transform */

.element {
    transform: translate(20px, 30px); /* x-axis, y-axis */
    transform: translateX(20px);
    transform: translateY(30px);
}
```


#### Rotate

```css
.element {
    transform: rotate(45deg);
}
```

#### Scale

```css
.element {
    transform: scale(1.2); /* x-axis, y-axis */
    transform: scaleX(1.2);
    transform: scaleY(0.3);
}
```

#### Skew

an element is skewed around the x or y axis by the angle specified.

```css
.element {
    transform: skewX(-25deg);
    transform: skewY(25deg);
}
```

### Transitions

```css
.element {
    background-color: black;
    transition: background-color 0.2s ease-in-out;
}
/*
transition: <property> <duration> <timing-function> <delay>
timing-functions :
ease, ease-in, ease-in-out, cubic-bezier, linear, step-start, step-end, steps()
 */
.element:hover {
    background-color: blue;
}

.element {
    transition-property: background-color;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0.1s;
}

.element {
    background-color: black;
    color: white;
    transition: all 0.2s ease-in-out;
}

.element:hover {
    background-color: gray;
    color: black;
}
```

### Progressive Enhancement

The term "progressive enhancement" refers to the use of newer features that add to the experience in modern browsers that support those features, but doesn't detract from the experience in older browsers.



























