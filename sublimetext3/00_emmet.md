# Emmet Documentation

### Nesting operators

#### Child: >
```
div>ul>li
```

...will produce

```
<div>
    <ul>
        <li></li>
    </ul>
</div>
```

#### Sibling: +
```
div+ul+li
```

...will produce

```
<div></div>
<p></p>
<blockquote></blockquote>
```

#### Climb-up: ^
```
div+div>p>span+em
```

```
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
```

with ^ operator, you can climb one level up to the tree and change context where following elements should appear:

```
div+div>p>span+em^bq
```

```
<div></div>
<div>
    <p><span></span><em></em></p>
    <blockquote></blockquote>
</div>
```

you can use as many ^ operators as you like, each operator will move one level up:

```
div+div>p>span+em^^^bq
```

```
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
<blockquote></blockquote>
```

#### Multiplication: *
```
ul>li*5
```

```
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

#### Grouping: ()
```
div>(header>ul>li*2>a)+footer>p
```

```
<div>
    <header>
        <ul>
            <li><a href=""></a></li>
            <li><a href=""></a></li>
        </ul>
    </header>
    <footer>
        <p></p>
    </footer>
</div>
```

You can nest groups inside each other and combine them with multipication * opeator.

```
(div>dl>(dt+dd)*3)+footer>p
```

```
<div>
    <dl>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
    </dl>
</div>
<footer>
    <p></p>
</footer>
```

### Attribute operators



