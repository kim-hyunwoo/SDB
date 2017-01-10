# Sass : Syntactically Awesome Stylesheets

> Sass File -> Sass Compiler -> CSS File

.scss 가 확장명. .sass

scss
```scss
$main: #444;
.btn {
    color: $main;
    display: block;
}

.btn-a {
    color: lighten($main, 30%);
    &:hover {
        color: lighten($main, 40%);
    }
}
```

css
```css
.btn {
    color: #444444;
    display: block;
}

.btn-a {
    color: #919191;
}

.htn-a:hover {
    color: #aaaaaa;
}
```

## Nesting

### Nesting Selectors

```scss
.content {
    border: 1px solid #ccc;
    padding: 20px;

    h2 {
        font-size: 3em;
        margin: 20px 0;
    }
    p {
        font-size: 1.5em;
        margin: 15px 0;
    }
}
```

### Nesting Properties

```scss
.btn {
    text: {
        decoration: underline;
        transform: lowercase;
    }
}
```

```css
.btn {
    text-decoration: underline;
    text-transform: lowercase;
}
```

## Parent Selector &

```scss
.content {
    border: 1px solid #ccc;
    padding: 20px;
    .callout {
        border-color: red;
    }
    &.callout {
        border-color: green;
    }
}
```

```css
.content {
    border: 1px solid #ccc;
    padding: 20px;
}
.content .callout {
    border-color: red;
}
.content.callout {
    border-color: green;
}
```

pseudo 선택자를 사용할 때 유용하다.

```scss
a {
    color: #999;
    &:hover {
        color: #777;
    }
    &:active {
        color: #888;
    }
}
```

```scss
.sidebar {
    float: right;
    width: 300px;
    .users & {  /* .users .sidebar */
        width: 400px; 
    }
}
```

* Nesting is easy, but dangerous. Do not nest unnecessarily.
* Try limiting your nesting to 3 or 4 levels and consider refactoring anything deeper.


## Variable

Variable names begins with $, like $base.

```scss
$base: #777777;

.sidebar {
    border: 1px solid $base;
    p {
        color: $base;
    }
}
```

변수는 재 할당이 가능하다.
```scss
$title: 'My Blog';
$title: 'About me';
//'About me'
```

하지만 !default;를 붙이면 이미 변수가 할당된 변수에 덮어쓰지 않는다.
```scss
$title: 'My Blog';
$title: 'About me' !default;
//'My Blog'
```

이렇게 사용할 수도 있다. 기본값을 정해놓고, 해당 변수를 사용한다면 덮어쓰고, 사용하지 않는다면 기본값을 사용한다.
```scss
//_buttons.scss
$rounded: 3px !default;

.btn-a {
    border-radius: $rounded;
    color: #777;
}
.btn-a {
    border-radius: $rounded;
    color: #222;
}
```

```scss
//application.scss
$rounded: 5px;

@import "buttons";
```

application.scss에서 임포트하면 5px이 $rounded에 덮어쓰기 된다.

### 변수에 할당할 수 있는 것들

1. booleans
2. numbers(with or without units)
3. colors
4. strings(with or without quotes)
5. lists
6. null

### selector, property 이름에 변수 사용하기

```scss
$side: top;

sup {
    position: relative;
    #{$side}: -0.5em; //top: -0.5em
}

.callout-#{$side} {  //.callout-top: -0.5em
    background: #777; 
}
```

## Mixin

Blocks of resusable code that take optional arguments

```scss
@mixin button {
    border: 1px solid #ccc;
    font-size: 1em;
    text-transform: uppercase;
}

.btn-a {
    @include button;  //@include <name of mixin>
    background: #777;
}
```

Make sure the @mixin block comes before the @include, especially when importing files containing mixins.

> @include : use a mixin

> @import : import a file

유용한 사용예

```scss
@mixin box-sizing {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

.content {
    @include box-sizing;
    border: 1px solid #ccc;
    padding: 20px;
}
```

인수 사용하기

```scss
@mixin box-sizing($x) {
    -webkit-box-sizing: $x;
    -moz-box-sizing: $x;
    box-sizing: $x;
}

.content {
    @include box-sizing(border-box);
    border: 1px solid #ccc;
    padding: 20px;
}

.callout {
    @include box-sizing(content-box);
}
```

default 인수값을 지정할 수도 있다.
```scss
@mixin box-sizing($x: border-box) {
    -webkit-box-sizing: $x;
    -moz-box-sizing: $x;
    box-sizing: $x;
}

.content {
    @include box-sizing; //border-box
    border: 1px solid #ccc;
    padding: 20px;
}

.callout {
    @include box-sizing(content-box);
}
```

여러 개의 인자를 전달할 수 있다.

```scss
@mixin button($radius, $color: #000) {
    border-radius: $radius;
    color: $color;
}
.btn-a {
    @include button(4px, #FFF);
}

.btn-b {
    @include button(4px) //#000
}
```

