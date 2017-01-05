# Web Animation

## Transitioning Color

Instant Color Change vs. Transition Color Change

Transition은 특정 시간이 경과하는 와중에 서서히 변화하는 것이 특징.

### Prominent Call to Action

A call to action(CTA) is generally the primary goal for a user on a site.
We can use CSS transition to make the "Buy" Button a prominent call to action on the site.

```html
<a class="btn">
    Buy Now!
</a>
```

```css
/* transition: <property> <duration> <timing-function> <delay> */
.btn {
    background-color: #00a0d6;
    transition: background-color 0.4s;
}

.btn {
    background-color: #007da7;
}
```

The fastest transition easily seen by the human eye is .256s

you can transition multiple comma-separated properties.

```css
.btn {
    background-color: #00a0d6;
    color:#ffffff;
    transition: background-color 0.4s, color 0.4s;
    transition: all 0.4s;
    /*
    using all : any property that can be animate will animate.
     */
}

.btn {
    background-color: #007da7;
    color: #e3e3e3;
}
```

transition: <property> <duration> <timing-function> <delay>

* property : defaults to all
* timing-function : defaults to ease
* delay : defaults to 0;

### Which properties nedd prefixes and when?

use a site like [caniuse.com](http://caniuse.com/).

## Transitioning Position

```html
<section>
    <a href="#" class="btn buy-button">
        Buy Now!
    </a>
</section>
```

1. Create 2 inner spans to hold the current and additional information to be shown on button hover.

2. Sytle the initial and hover states of the button.

3. Create a transition between initial and hover stats.


```html
<section>
    <a href="#" class="btn buy-button">
        <span class="top content">Buy Now!</span>
        <span class="bottom content">On Sale $59</span>
    </a>
</section>
```

```css
/* my solution */
* {
    box-sizing: border-box;
}

html,
body {
    height: 100%;
}

body {
    background-color: #4a4a43;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    padding: 0;
}

section {
    display: flex;
    justify-content: center;
    align-items: center;
}

.btn {
    width: 150px;
    height: 50px;
    text-decoration: none;
    border-radius: 25px;
    background-color: #00a0d6;
    color: #FFFFFF;
    font-size: 20px;
    font-weight: bold;
    text-transform: uppercase;
    transition: background-color 0.4s, color 0.4s;
    overflow: hidden;
}

.content {
    text-align: center;
    line-height: 50px;
    display: block;
    width: 100%;
    top: 0;
    position: relative;
    transition: top 0.3s;
}

.btn:hover .content {top: -50px; }

.btn:hover {
    background-color: #007da7;
    color: #e3e3e3;
}
```

## Transitioning Visibility

```html
            <div class="modal-overlay"></div>

            <div class="modal">
                <div class="modal-header">
                    <a href="" class="modal-close">&times;</a>
                    <h3>Purchase Swwet Lands</h3>
                </div>

                <div class="modal-content">
                    <form action="" class="form">

                    </form>
                </div>
            </div>
```

```css
/********************************/
/* modal                        */
/********************************/

.modal,
.modal-overlay {
    visibility: hidden;
    opacity: 0;
    transition: all .4s;
}

.modal.active,
.modal-overlay.active {
    opacity: 1;
    visibility: visible;
}
```

opacity: 0; - hides element, element still takes up same width/height.

visibility: hidden; - makes element transparent to click events.

display: none; - removes element form DOM, does not transition.

## Transforming Rotate

```css
.modal-close {
    font-size: 200%;
    right: 15px;
    top: 0;
    position: absolute;
    transition: transform 4s;
}

.modal-close:hover {
    transform: rotate(360deg);
    transform: rotate(1turn);
}
```

## Transforming Scale and Translate

We want the initial state of out label to provide information as a text placeholder.

On input:focus, we want the label to slide up and scale down, becoming your average label for an input.

1. The input label text is Transitioning Color.
2. Scaling down to 80%. When you scale something down, it still maintains its original box model size.
3. Translating up above the input.

```css
.form-input + .form-label {
    position: relative;
    padding: 0 1em;
    color: #6a7989;
    tranform-origin: center left;
    /* transform-origin: <y origin> <x origin> */
    transition: color .3s, transform .3s;
}

.form-input:focus + .form-label {
    color: #333333;
    transform: scale(0.8) translateY(-3px);
}
```

## Creating and Resuing Keyframes

Keyframe Animations : A list of what should happen over the course of the animation which properties should change, how, and when.

1. Create the Animation
2. Assign the Animation

```css
/*
@keyframes <name-animation> {
    <step 1> {<property> : <value>;}
    <step 2> {<property> : <value>;}
}
*/

@keyframes swing {
    0% {transform: rotate(0deg);}
    100% {transform: rotate(-10deg);}
}

#left-arm {
    tranform-origin: top center;
    animation: swing 2s 0s inifinite ease;
}
/* duration must come before delay; */

#right-arm {
    tranform-origin: top center;
    animation: swing 2s inifinite;
}
```

### multi-step keyframes

```css
@keyframes swing {
    0% {transform: rotate(0deg);}
    25% {transform: rotate(-10deg);}
    50% {transform: rotate(0deg);}
    75% {transform: rotate(-10deg);}
    100% {transform: rotate(0deg);}
}

@keyframes swing {
    0%, 50%, 100% {transform: rotate(0deg);}
    25%, 75% {transform: rotate(-10deg);}
    30%, 70% {transform: scale(0.95);}
}

#left-arm {
    tranform-origin: top center;
    animation: swing 2s 0s inifinite linear;
}
/* duration must come before delay; */

#right-arm {
    tranform-origin: top center;
    animation: swing 2s inifinite 1s linear;
}
```

### Animating forms with keyframes

```css
/* fade in the overlay */
@keyframes fadeIn {
    from {
        opacity: 0;
        visibility: hidden;
    }
    to {
        opacity: 1;
        visibility: visible;
    }
}

.modal-overlay.active {
    animation: fadeIn .25s forwards;
}

/* keyframe for entire modal */
@keyframes slideUp {
    from {
        transform: translateY(400px);
    }
    to {
        transform: translateY(-300px);
    }
}

.modal.active {
    animation: slideUp .65s .5s cubic-bezier(0.17, 0.89, 0.32, 1.28) forwards,
    fadeIn .65s .5s forwards;
}

/* creating the slideUpSmall animation */

@keyframes slideUpSmall {
    from {transform: translateY(80px);}
    to {transform: translateY(0);}
}

.modal-header h3 {
    animation: slideUpSmall .25s .75s forwards,
    fadeIn .25s .75s forwards;
}

.modal.active .form-field {
    animation: slideUpSmall .25s .8s forwards,
    fadeIn .25s .8s forwards;
}
```

## Animating SVGs With CSS

애니메이션을 이용할 때 .png를 사용하면 움직이는 파트마다 .png 파일을 따로 만들어줘야 한다. 반면 SVG를 이용하면 각 파트에 따로 애니메이션을 쉽게 적용할 수 있다. 일러스트레이터를 이용해서 SVG 파일을 만들거나 무료 혹은 구매 후에 사용할 수 있다.

SVG는 vector 기반의 이미지를 포함하는 파일 포맷이다. 

```css
@keyframes flyoff {
    0% {transform: rotate(0deg);}
    10% {transform: rotate(30deg);}
    20% {transform: rotate(0deg);}
    30% {transform: rotate(-30deg);}
    40% {transform: rotate(0deg);}
    45% {opacity: 1;}
    50% {
        transform: rotate(100deg) scale(3);
        opacity: 0;
    }
    100% {
        opacity: 0;
        transform: scale(1);
    }
}

#sprinkles {
    transition: transform 2s;
    transform-origin: 302px 337px;
    animation: flyoff 3s inifinite ease-in;
}

@keyframes shake {
    0% {transform: rotate(0deg);}
    10% {transform: rotate(30deg);}
    20% {transform: rotate(0deg);}
    30% {transform: rotate(-30deg);}
    40% {transform: rotate(0deg);}
    50% {transform: rotate(100deg);}
}

#icing-fill {
    animation: darken 3s inifinite,
                shake 3s inifinite ease-out;
    transform-origin: 302px 337px;
}

#icing-outline {
    animation: shake 3s inifinite ease-out;
    tranform-origin: 302px 337px;
}
```

## 만들어보기

[sweetlands](http://sweetlands.codeschool.io/);

[github](https://github.com/codeschool/WatchUsBuild-QuizAppWithCSSAnimations)

[codepen](http://codepen.io/)

[animate.css](https://daneden.github.io/animate.css/)

[cubic-bezier.com](http://cubic-bezier.com/#.17,.67,.83,.67)

[easings.net](http://easings.net/ko)



















