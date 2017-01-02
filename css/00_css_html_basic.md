# css basics

## Background Images

```css
body {
    background-color: #5f5f5f;
    background-image: url(images/gobbler.png);
    background-position: top left;
    background-repeat: repeat | repeat-x | repeat-y | no-repeat;
}

/* shortened */
body {
    background: #5f5f5f url(images/gobbler.png)
 top left repeat; 
}
```

### 사용예

```html
<div class="main-content">
    <h2>Home</h2>
    <div class="featured-image">
        <h3>Featured: Magic Cake</h3>
    </div>
</div>
```

```css
.featured-image {
    width: 630px;
    heigth: 246px;
    background: #ffffff url(images/featured-cake.png) top left no-repeat;
}

.featured-image h3 {
    margin: 0;
    background-color: #333333;
    color: #ffffff;
    padding: 5px 0 5px 15px;
    text-transform: uppercase;
}
```

## Floating Images

img is an inline-level tag. <p> is a block-level tag/

```html
<ul class="recipes">
    <li>
        <img src="...">
        <h2><a href="...">Magic Cake</a></h2>
        <p>...</p>
    </li>
</ul>
```

```css
.recipes img {
    float: left;
    padding-right: 10px;
}
/*
This makes the image take up some of the left space that the h3 and p boxes would normally eat up as block-level tags.
 */
```

## Adjusting Font Styles

브라우저는 기본 폰트 스타일을 가진다. 이것을 초기화 해줘야 한다. 그리고 각 태그 별로 선택해서 폰트 스타일을 지정해주면 된다.

```css
html, body, h1, h2, h3, p, ol, ul, li, a {
    padding: 0;
    border: 0;
    margin: 0;
    font-size: 100%;
    font: inherit;
}

h2 {
    color: #7facaa;
    margin: 0 0 20px 0;
    text-align: center;
    font-size: 26px;
    font-weight: bold;
}

/* Adjusting the line height */

.main-content p {
    line-height: 16px;
}
```

## Creating Web Forms

### Basic Form

```html
<form>
    <label for="recipe-name">Recipe Name</label>
    <input type="text" id="recipe-name" name="">
    <input type="submit" value="Click to Submit" name="">
</form>
```

The value of the for attribute in the label should be the same as the value of the id attribute in an input field to associate the label and input. Each for/id pair has to be unique on the page.

### Text-area

```html
<form action="">
    <label for="ingredient">Ingredients</label>
    <textarea id="ingredient"></textarea>
    <input type="submit" value="Click to Submit" name="">
</form>
```

labels and inputs are inline-level tags, but it usually makes sense to display one on top of the other like block-level instead of side-by-side.

```css
label, input {
    display: block;
}

label {
    margin-bottom: 10px;
}

input {
    width: 500px;
    margin-bottom: 25px;
}
```

Since the sumit button is technically an input tag, our input selector properties are affecting the way it's displayed. **Attribute Selector** are a way to style a tag based on one of its attribute.

```css
input[type=submit] {
    width: 120px;
    font-size: 30px;
}
/*
selector [<attribute name> = <attribute value>] {

}
 */
```

### Check box

label and input are on separate lines since the checkbox is so small, so we can use attribute selectors again to make just this input and label display inline.

```html
<form>
    <label for="newwletter">GEt Newsletter?</label>
    <input type="checkbox" id="newsletter" name="">
    ...
</form>
```

```css
input[type=checkbox], label[for=newsletter] {
    display: inline;
}
```





































































