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

## wrap

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
<main>
    <img src="">
    <h1></h1>
    <p></p>
    <p></p>
</main>
```

```css
main {
    display: flex;
}










