# HTML5 overview

## Doctype

The new HTML5 doctype :

> <!DOCTYPE html>

## Meta Declaration

The meta declaration in HTML4 :

> <meta http-equiv="Content-Type" content="text/html"; charset="UTF-8">

in HTML5:

> <meta charset="utf-8">

## Script Tag

in HTML 4.01:

> <script type="text/javascript" src="file.js"></script>

in HTML5:

> <script src="file.js"></script>

## Link Tag

in HTML 4.01:

> <link rel="stylesheet" type="text/css" href="file.css">

in HTML5:

> <link rel="stylesheet" href="file.css">

## i, b, em and strong tags. New semantic meanings.

in HTML4 i and b tags were font style elements:

* The i tag rendered an italic font style
* The b tag rendered a bold font style

in HTML5 :

* The i tag represents text in an "alternate voice" or "mood"
* The b tag represents "stylistically offset" text

### i tag

Some example uses for the i tag :

* 분류학적 지정
* 기술 용어
* 다른 언어의 관용구
* 음역
* 생각
* 선박의 명칭

i 태그의 사용 예 :

```html
<p><i>I hope this works</i>, he thought.</p>
```

### b tag

Some exaple uses for the b tag:

* 키워드(문서 개요)
* 제품이름(리뷰)
* 상호작용이 가능한 문자 기반의 프로그램에서의 문자
* article lead 안내문장??

b 태그의 사용 예 :

```html
<p><b class="lead">The event takes place this upcoming Saturday, and over 3,000 people have already registered.</b></p>
```

### em, strong tag

In HTML4 :

* The em tag meant emphasis
* The strong tag meant strong emphasis

In HTML5 :

* The em tag : stress emphasis
* The strong tag : strong importance

사용 예 :

```html
<p>Make sure to sign up <em>before</em> the day of the event, <strong>September 16, 2013</strong>.</p>
```


## HTML5 elements

### Section

a generic document or application section.

section vs. div :

> A div has no semantic meaning, but the section element does. It is used for grouping together thematically related content.

You have to ask yourself before using section element.

> "Is all of the content realated?"

### Outline

The following elements have their own self-contained outline:

* Article
* Aside
* Nav
* Section

```html
<h1>This is the title of the page</h1>
<section>
    <h2>This is the title of a sub-section</h2>
</section>

<!-- 아래와 같이 변경해도 같은 사이즈의 크기를 가진다 -->

<h1>This is the title of the page</h1>
<section>
    <h1>This is the title of a sub-section</h1>
</section>
```

### Header

A group of introductory or navigational aids.

```html
<!-- HTML4 -->
<div class="header">
    <!-- ... -->
</div>

<!-- HTML5 -->
<header>
    <!-- ... -->
</header>
```

헤더는 어디에나 위치할 수 있다.

```html
<section>
    <header>
        <h1>The heading of the section</h1>
        <p>this is content in the header</p>
    </header>
    <p>This is some information within the section</p>
</section>
```

### footer

footer for its nearest ancestor sectioning content or sectioning root element. Like header, the footer element is <u>_not_</u> position-dependent. It should describe the content it is contained within.

```html
<!-- HTML4 -->
<div class="footer">
    <!-- ... -->
</div>

<!-- HTML5 -->
<footer>
    <!-- ... -->
</footer>
```

### Aside

Initially, the HTML5 spec defined the aside element as being "tangentially related to the content surrounding it."

aside covers various contexts :

* When used within an article element, the aside contents should be related to that particular article it is contained within.
* When used outside an article element, the aside contents should be specifically related to the site.

Related to the article or page.

```html
<!-- HTML4 -->
<div class="sidebar">
    <!-- ... -->
</div>

<!-- HMTL5 -->
<aside>
    <!-- ... -->
</aside>
```

### Nav

The nav element represents a section of a page that links to other pages or to parts within the page: a section with navigation links.

The nav element is intended for "major navigation."

```html
<!-- HTML4 -->
<ul class="nav">
    <!-- ... -->
</ul>

<!-- HTML5 -->
<nav>
    <ul>
        <!-- ... -->
    </ul>
</nav>
```

### Article

represents a complete, or self-contained, composition in a document, page, application, or site and that is, in principle, independently distributable or reusable, e.g. in syndication.

The article element is another type of section. It is used for self-contained related content.

Determining if a particular piece of content is "self-contained" :

> Ask yourself if you would syndicate the content in an RSS or Atom feed.

uses for the article tag:

* A blog post
* A news story
* A comment on a post
* A review

```html
<!-- HTML4 -->
<div class="article">
    <!-- ... -->
</div>

<!-- HTML5 -->
<article>
    <!-- ... -->
</article>
```

### main

The main element represents the main content of the body of a document or application. The main content area consists of content that is directly related to or expands upon the central topic of a document or central functionality of an application.

Don't :

* Do not include more than one main element in a document
* Do not include the main element inside of an article, aside, footer, header, or nav element.

```html
<main>
    <!-- ... -->
</main>
```

### Figure

The figure element represents a unit of content, optionally with a caption, that is self-contained, that is typically referenced as a single unit from the main flow of the document, and that can be moved away from the main flow of the document without affecting the document's meaning.

```html
<figure>
    <img src="image.jpg" alt="My Picture">
</figure>
```

### Figcaption

represents a caption or legend for a figure.

```html
<figure>
    <img src="image.jpg">
    <figcaption>This is a caption for the picture.</figcaption>
</figure>
```

### Time

represents either a time on a 24 hour clock, or a precise date in the proleptic Gregorian calendar, optionally with a time and a time-zone offset.

```html
<time>2016-10-11</time>

<!-- use the datetime attribute to get our desired format -->
<time datetime="2016-10-11">11/10/2016</time>
<!-- yyyy-mm-dd -->
<!-- without the datetime attribute, content must be a valid date, time, or precise datetime. -->
```

## HTML5 Forms

input type의 default 값은 text이다.

### Search, email, url, date, tel, number, range ...

모바일 환경에서 접근했을 때 키보드의 레이아웃이 알맞게 변한다.

```html
<input type="search">
<input type="email">
<input type="url">
<input type="date">
<input type="tel">
<input type="number">
<input type="range"> <!-- slider -->
<input type="month">
<input type="week">
<input type="time">
<input type="datetime-local">
<input type="color">
```

### Datalist

represents a set of option elements that represents predefined options for other controls. 텍스트 창에 입력을 시작하면 value값을 추천해서 띄워준다.

```html
<input type="text" list="browser">
<datalist id="browser">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Internet Explorer">
    <option value="Opera">
    <option value="Safari">
</datalist>
```


### New form attributes

placehorder : allows you to specify a message that is shown inside the input, hidden when the user starts typing, and then returns when focus is lost on the input.

```html
<input type="text" placeholder="Enter your email...">
```

autofocus : automatically focus the specified input when the page is rendered.

```html
<input type="text" autofocus >
```

required : when the form is submitted, the user will be notified of an error if the field is left blank.

```html
<input type="text" required >
```

pattern : accepts a JavaScript reqular expression that can be used to validate a form field to match the pattern.

```html
<input type="text" pattern="[0-9]{3}">
```

____























































































