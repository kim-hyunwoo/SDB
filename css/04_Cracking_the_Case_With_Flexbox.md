# Flex Box

## display flex

```html
<article class="setup">This ...
    <img src="..." height="150" width="150">
    <p>...</p>
</article>
```

```css
article {
    border: 1px solid #dleda6;
}
```

기본적으로 article은 블록 element로 디스플레이 되고, img는 인라인 element로 디스플레이 된다.

```css
article {
    border: 1px solid #dleda6;
    display: inline;
}
```

article의 디스플레이를 inline으로 바꿔주면 더 이상 img와 p를 감싸지 않는다.

```css
article {
    border: 1px solid #dleda6;
    display: flex;
}
```

flex container로 바꾸면 자식 요소를 감싸는 것 뿐만 아니라(block level) 자식 요소가 어떻게 정렬이 되는지의 방식도 변경한다. 기본적으로 수평적으로 정렬된다.

```css
article {
    border: 1px solid #dleda6;
    display: inline-flex;
}
```

inline-flex는 자식요소를 감싸지만 inline-level의 콘테이너이다.

flex 콘테이너는 자식 요소의 정렬에는 관여하지만, 손자 요소의 정렬에는 관여하지 않는다.

```html
<article class="setup">This ...
    <img src="..." height="150" width="150">
    <div>
        <h1>Blah blah</h1>
        <p>...</p>
    </div>
</article>
```

## flex-wrap

nowrap, wrap, wrap-reverse

```html
<article class="clue">
    <img src="">
    <h1></h1>
    <p></p>
    <p></p>
</article>
```

```css
.clue {
    border: 1px solid @dleda6;
    display: flex;
    flex-wrap: wrap;
}
```

충분한 공간이 확보되지 않으면 자식요소들은 밑으로 내려간다.

```css
.clue {
    border: 1px solid @dleda6;
    display: flex;
    flex-wrap: wrap-reverse;
}
```

이미지 요소가 아래로 내려가고 공간이 확보되지 않은 나머지 요소들을 위로 올라간다.

## flex-direction

row, row-reverse, column, column-reverse

공간이 확보되어 있어도 자식요소들을 아래로 위치시키고 싶다면 flex-direction을 사용하면 된다.

```html
<body>
    <header></header>
    <main></main>
    <footer></footer>
</body>
```

```css
body {
    display: flex;
    flex-direction: column;
}
```


## justify-content

The main axis is determined by the flex-direction. Used to distribute space on the main axis, the default value is flex-start and it accepts: flex-start | flex-end | center | space-between | space-around.

```css
@media screen and (min-width: 375px) {
    nav {
        display: flex;
        justify-content: flex-start;
    }
}
```

## order

order is used to determine the order of flex items along the main axis. It defaults to 0 and accepts positive and negative numbers.

```css
@media screen and (min-width: 375px) {
    .nav-clues {
        order: -1; /* 자식 요소가 -1 0 1 의 순서로 배치됨 */
    }
}
```


## align-items

The cross axis is perpendicular to the main axis. Used to align content on the cross axis, the default value is stretch and it accepts : stretch | flex-start | flex-end | center | baseline.

```css
@media screen and (min-width: 375px) {
    .nav {
        align-items: stretch;
        display: flex;
        justify-content: center;
    }
}
```


## flex-grow

(Mostly) Default Behavior

```css
.add-input {
    align-items: center;
    display: flex;
    min-width: 0;
}
```

flex-grow used to specify the ratio of the space an item should fill in the main axis. It accepts numbers and the default is 0.

```css
.add-label,
.add-input,
.add-button {
    flex-grow: 0;
}
```

Assigning a value of 1 makes an item fill as much space as possible. Available space is space of the container - necessary space of items.

```css
.add-input {
    flex-grow: 1;
}
```

Assigning a value of 1 to multiple properties will have them all try to fill as much space as possible.

```css
.add-label,
.add-input,
.add-button {
    flex-grow: 1;
}
```

Empty flex containers will divide the space according to the ratio.

```css
/* if container is 1000px */
.one, /* .one .two .three is empty div */
.three {
 flex-grow: 1; /* 250px for .one and .three */
}
.two {
    flex-grow: 2; /* 500px for .two */
}
```

## flex-shrink

Used to specify the "shrink factor" of a flex itme. it accepts numbers and the default is 1.

```css
.clue-img {
    flex-shrink: 0; /* do not shrink */
}
```

## flex-basis

Used to specify the initial size of a flex item. It defaults to auto and currently supports CSS units: %, px, em, rem, etc.

```css
.clue {
    flex-basis: 47.5%;
}
.clue-img {
    flex-basis: 140px;
}
```

Items with a flex-basis that is an absolute unit will not grow beyond the value by default.

```css
div {
    flex-basis: 250px;
}
```

Items with a relative flex-basis will grow to fill the space

```css
div {
    flex-basis: 25%;
}
```

more...

```css
div {
    flex-basis: fill;
    flex-basis: max-content;
    flex-basis: min-content;
    flex-basis: fit-content;
    flex-basis: content;
}
```


## align-self

Used to align individual flex items by overriding the align-items value. The default value is stretch and it accepts: stretch | flex-start | flex-end | center | baseline.

```css
.fieldset {
    align-items: center;
    display: flex;
}

.add-input {
    align-self: stretch; /* same vertical height as the container */
}
```

```css
body {
    ...
    align-items: stretch;
}

main {
    align-self: center; /* center of the container */
}
```

another way

```css
body {
    ...
    flex-wrap: wrap;
}

main {
    ...
    flex-basis: 100%;
}
```

## align-content

Used to align wrapped flex items. The default value is stretch and it accepts: stretch | flex-start | flex-end | center | space-between | space-around.

```css
body {
    ...
    /* align-items: space-between;  does not effect to wrapped items */
    align-content: space-between;
    flex-wrap: wrap;
}

main {
    ...
    flex-basis: 100%;
}
```

## shorthand for flex

Shorthand property used to set values for flex-grow, flex-shrink, and flex-basis. The default is: 0 1 auto;

```css
.flex {
    flex: 0 1 auto; /* grow shrink basis */
    flex: 1; /* grow */
    flex: 1 0; /* grow shrink */
    flex: 1 47.5%; /* grow basis */
    flex: none; /* shrink */
}
```

```css
.flex {
    flex-flow: row nowrap; /* direction wrap */
    flex-flow: cloumn; /* direction */
    flex-flow: wrap; /* wrap */
}
```











