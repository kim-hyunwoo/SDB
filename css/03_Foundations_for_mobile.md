# Foundations for mobile

### Fluid Layouts

```html
    <h1>
        It is quite a three pipe problem.
        <a href="#">Please get me a pipe.</a>
    </h1>
```

```css
h1 {
    font-size: 40px;
    font-weight: bold;
}
```

change font-size px to em.

```css
/* do this */
html {
    font-size: 16px; /* standard font-size */
}
body {
    color: #352a25;
    font-family: Georgia, serif;
    font-size: 62.5% /* 1em = 10px */
}

h1 {
    font-size: 3em;
    /*
    target / context = result
    30px / 10px = 3em
     */
    font-weight: bold;
}

h1 a {
    font-size: 0.4666667em;
    /*
    14px / 30px(h1 font size is the correct context) =
     */
    text-transform: uppercase;
    text-decoration: none;
    color: #6c564b;
}
```


## Fluid Layouts

What Makes a fluid site? 

- Fluid Grid
- Relative Values(percentages)

```css
.site {
    margin: 40px auto;
    width: 90%;
}

.sidebar {
    float: left;
    text-align: center;
    width: 32.446809%;
    /*
    target / context = result
    305px / 940px = 0.32446809
     */
}

.content {
    background-color: #fff;
    box-shadow: 0 3px 4px rgba(0,0,0,.3);
    margin-left: 34.574468%;
    /*
    325px / 940px = .34574468
     */
    padding: 3.389830%;
    /*
    20px / 590px = .03389830
     */
    width: 62.765957%;
    /*
    590px / 940px = 0.62765957
     */
}
```


[responsv.com](http://responsv.com/flexible-math/)


## Adaptive adventures

what is adaptive web design? : design for controlled adaptation

Four Design Keys

* Who is your user?
* How will they use the site?
* Context(i.e. what device?)
* Content(how will it adapt?)

1. Adaptive Markup
2. Break Points
3. Media Queries

```css

/*-------------------------------------------------
    media queries
 -------------------------------------------------*/

@media screen and (max-width: 480px) {

}

```


## Mobile first approach

Focus on the most important things:

* Simplyfy content
* Prioritize layout
* Optimize user experience

1. Fluid Layouts -> 단점 : 뷰포트 사이즈가 작아지면 가독성 떨어짐
2. Adaptive Design -> 모든 universal web을 커버하지 못함
3. Responsive Design -> flexible and universal

Responsive design : content defines break points

화면 크기의 변화에 따라 구성을 바꿔줘야 할 필요가 있는 부분을 찾아서 break point로 삼는다.

```css

/* < 870px */
@media screen and (max-width: 870px) {

}
```

휴대용 디바이스의 방향에 따라 다르게 적용할 수도 있다.

```css
/* Portrait */
@media screen and (orientation:portrait) {
    /* Portrait styles */
}

/* Landscpae */
@media screen and (orientation:landscape) {
    /* Landscape styles */
}
```

```css
@media screen and (max-width: 850px) {
  .banner-caption {
    bottom: 0;
    width: 100%;
  }   
}
```

## Responsive media

### Responsive Images

Use max-width 

```css
img,
embed,
object,
video {
    max-width: 100%;
}
```

### Retina Images

Retina Images = 1.5x - 2x the pixel density

Use Media Queries

```css
@media
only screen and (-webkit-min-device-pixel-ratio: 1.5),
only screen and (min-device-pixel-ratio: 1.5) {
    /* Styles */
}
```

original

```css
.logo {
    background-image: url(images/logo.png) no-repeat;
}
```

```css
@media
only screen and (-webkit-min-device-pixel-ratio: 1.5),
only screen and (min-device-pixel-ratio: 1.5) {
    .logo {
        background-image: url(logo@2x.png);
        -webkit-background-size: 12px 16px;
        background-size: 12px 16px;
    }
}
```

But what about file size?

PictureFill - created by Scott Jehl

[scottjehl](http://scottjehl.com/)

[pictureFill](https://github.com/scottjehl/picturefill)

Creates <picture> element : specify different image sizes to be served by different devices.

```html
<picture alt="Our Alternate Text">
    <!-- Smallest size first - no @media querifier -->
    <source src="content-image.jpeg" >
    <!-- Large size - send to viewports 800px wide and up -->
    <source src="content-image-lrg.jpeg" media="(min-width:800px)">
<!-- Fallback content for non-JS or non-media-query-supporting-browsers -->

    <noscript>
        <img src="content-image.jpeg" alt="Our Alternate Text">
    </noscript>
    <!-- When JS is disabled -->
</picture>
```

<picture> doesn't exist must include picturefill.js in your <head>




