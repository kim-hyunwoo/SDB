# CSS Study

## Clearing Floats

Clearing is necessary if :

* Floated items can be taller than non-floated content.
* All children are floating.

Common float-clearing methods :

1. Clear with a subsequent element
2. Manual clearing
3. The clearfix

```
clear: left / right / both
```

### Clear with a subsequent element

```html
<body>
    <div>
        <img src=" http://placehold.it/350x150">
        <p>blah blah blah blah </p>
    </div>
    <div class="intro">
        <p>Whee!</p>
    </div>
</body>
```

```css
img {
    float: left;
}

.intro {
    clear: both; /* any floats from the previous container won't overwrap */
}
```

problems of this method :

* requires sequence to stay intact - breaks if things move
* Background/border do not extend.

### Manual Clearing

```html
<body>
    <div>
        <img src=" http://placehold.it/350x150">
        <p>blah blah blah blah </p>
    </div>
    <div class="clear"></div>
    <p>Whee!</p>
</body>
```

```css
img {
    float: left;
}

.clear {
    clear: both;
}
```

problems of this method :

* Requires an empty element
* Might not be necessary later

### Clearfix

이 부분은 잘 모르겠음..

HTML문서 구조에서 부모 요소가 자식 요소를 감싸고 있을 때, 자식 요소에게 float 형식을 적용하면 부모 요소가 자식 요소를 더 이상 감싸지 않게되고 높이 값을 파악하지 못하게 되는 버그가 발생한다. 따라서 부모 요소의 높이 값이 0px로 출력되고, 전체적인 HTML 요소들이 뒤엉켜버리는 경우가 많다. 이때 부모 요소가 다시 자식 요소를 감쌀 수 있게 float을 초기화(clear)하여 버그를 고쳐주는(fix) 코드가 필요한데 이것을 clearfix라고 한다.

```css
img {
    float: left;
}

.clearfix:before,
.clearfix:after {
    content: " ";
    display: table;
}

.clearfix {
    clear: both;
}
```

## Inheritance & Specificity

```css
 p { color: #fff; } /* 0,0,0,1 */
 .intro { color: #98c7d4; } /* 0,0,1,0 */
 #header { color: #444245; } /* 0,1,0,0 */
 /* <h1 style="color: #000; " >Mogul</h1> 1,0,0,0 */
 p {color: #fff !important; } /* ! */

 .intro p.article { color: #fff; } /* 0,0,2,1 */
 .intro ul li.active { color: #98c7d4; } /* 0,0,2,2 */
 ```

### 예제

```html
<section id="content">
    <p class="featured">Free coffee with ski rental.</p>
    <p>Choose from 48 different ski colors.</p>
</section>  
```

```css

#content p {
    color: #000; /* 0, 1, 0, 0 */
}

/* 클래스 어트리뷰트로 적용한 css 스타일을 적용되지 않는다. */
.featured {
    color: #777; /* 0, 0, 1, 0 */
}
/*
여기에서 !important를 적용하는 것은 좋은 방법이 아니다.
color: #777 !important!;

다음과 같이 변경해주는 것이 옳다.
 */
 
 #content .featured {
    color: #777;
 }
```

## Staying DRY

D : Don't R : Repeat Y: Yourself

## reset and normalization

[meyerweb](http://meyerweb.com/eric/tools/css/reset/)

[normalize.css](http://necolas.github.io/normalize.css/)

## Image Cropping

```html
<h4>Rental Products</h4>
<ul class="rental">
    <li class="crop">
        <img src="snowboard.jpg" alt="Snowboard">
    </li>
</ul>   
```

```css
.crop {
    height: 300px;
    width: 400px;
    overflow: hidden;
}

.crop img {
    height: auto;
    width: 400px;
}

.crop img {
    height: 300px;
    width: auto;
}
```

## Image Replacement

### Text-indent hides the placeholder text

```html
<a href="#" class="logo">Sven's Snowshoe Emporium</a>
```

```css
.logo {
    background: url(logo.png);
    display: block;
    height: 100px;
    width: 200px;
    text-indent: -9999px;
}
```

### Sprites

이미지를 여러개 사용하지 않고 하나의 파일로 만들어서 사용. 로딩 시간이 줄어듦. HTTP 요청을 여러 번 하지 않아도 됨.

```css
.logo {
    background: url(logo.png);
    display: block;
    height: 100px;
    width: 200px;
    text-indent: -9999px;
}

.logo:hover, .logo:focus {
    background-position: 0 -100px;
}
```

[Sprite-Cow](http://www.spritecow.com/)

[csssprites.com](http://csssprites.com/)

### base64 encoding

* Directly embed images into your CSS
* IE8+

```css
background-image: url(data:image/png;base64,iVBO...;);
```

[base64](http://davidbcalhoun.com/2011/when-to-base64-encode-images-and-when-not-to/)

## Pseudo element

[css-tricks](https://css-tricks.com/pseudo-element-roundup/)
























