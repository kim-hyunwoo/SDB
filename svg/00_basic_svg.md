# Basic svg

```svg
<svg height="100"
    width="100"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1">
    <rect height="80" width="100" fill="#000000"/>
    <rect height="50" width="80" fill="white" x="10" y="10" />
    <rect height="10" width="40" x="30" y="90" />
</svg>
```


## Circles

```svg
<rect height="100" width="70" fill="white" stroke="#ff2626" stroke-width="10" x="5" y="5" rx="5" ry="5"/>
<circle cx="40" cy="105" r="3" fill="white" />
<ellipse cx="50" cy="50" fill="blue" rx="10" ry="25" />
```

How can we animate and change svg?

svg inline
```html
<!DOCTYPE html>
<html>
<head>...</head>
<body>
    <h1>Schmuffle-Phone Icon</h1>
    <svg height="110"
        width="80"
        xmlns="http://www.w3.org/2000/svg"
        version="1.1">
        <rect height="100" 
                width="70" 
                fill="white" 
                stroke="#ff2626" 
                stroke-width="10" 
                x="5" 
                y="5" 
                rx="5" 
                ry="5"/>
        <circle cx="40" cy="105" r="3" fill="white" />
    </svg>
</body>
</html>
```

```css
circle {
    animation: grow 2s infinite;
    transform-origin: center;
}

@keyframse grow {
    0% {transform: scale(1);}
    50% {transform: scale(0.5);}
    100% {transform: scale(1);}
}
```

## Shapes for you

```html
<body>
    <svg...>
        <circle r="130" cx="134" cy="134" />
        <line x1="47" y1="198" x2="221" y2="198" />
        <polygon points="52,190 134,30, 216,190" />
        <text x="134" y="142">SVG</text>
    </svg>
</body>
```

```css
circle {
    fill:none;
    stroke: #008B6F;
    stroke-width: 7px;
}
line {
    stroke: black;
    stroke-width: 5px;
}
text {
    font-size: 60px;
    text-anchor: middle;
    font-family: 'FilmotypeMajor';
    stroke: #000;
    stroke-width: 3px;
    fill: #F6F7F3;
}
polygon {
    fill: #008b6f;
    stroke: black;
    stroke-width: 2px;
}
```

## Groups

```html
<body>
    <svg...>
        <circle r="130" cx="134" cy="134" />
        <line x1="47" y1="198" x2="221" y2="198" />
        <polygon points="52,190 134,30, 216,190" />
        <text x="134" y="142">SVG</text>
        <g class="triangle_group" transform="tranlate(45,67)">
            <polygon points="7,10 12,0 17,10" />
            <polygon points="0,25 5,15 10,25" />
            <polygon points="15,25 20,15 25,25" />
        </g>
        <g class="triangle_group" transform="tranlate(198,67)">
            <polygon points="7,10 12,0 17,10" />
            <polygon points="0,25 5,15 10,25" />
            <polygon points="15,25 20,15 25,25" />
        </g>
        <g class="triangle_group" transform="tranlate(121.5,211)">
            <polygon points="7,10 12,0 17,10" />
            <polygon points="0,25 5,15 10,25" />
            <polygon points="15,25 20,15 25,25" />
        </g>
    </svg>
</body>
```

```css
polygon {
    fill: #008B6F;
    stroke: #000;
    stroke-width: 2px;
}

.triangle_group polygon {
    fill: #59BFC6;
    stroke-width: 1px;
}
```

### Rotate, Scale

> rotate(degrees x-origin y-origin)

> scale(0.6)

```html
<g class="triangle_group" transform="tranlate(198,67) rotate(10, 12.5 12.5) scale(1.6)">
            <polygon points="7,10 12,0 17,10" />
            <polygon points="0,25 5,15 10,25" />
            <polygon points="15,25 20,15 25,25" />
</g>
```

## Responsively : Using a ViewBox

Viewport Coordinate System : height:auto, width: 50%

ViewBox Coordinate System : height: 268, width: 268

```html
<svg xmlns="http://www.w3.org/2000/svg"
        version="1.1"
        viewBox="0 0 268 268  "> <!-- viewBox ="x-origin y-origin width height" -->
        ...
    </svg>
```

```css
svg {
    height: auto;
    width: 50%;
}
```

## Paths are fun

Exporting an SVG From a Drawing Tool

## Symbol

```html
<svg xmlns="http://www.w3.org/2000/svg"
        class="defined-icon"
        version="1.1"
        viewBox="0 0 268 268  "> <!-- viewBox ="x-origin y-origin width height" -->
        ...
    <symbol id="phone">
        <rect x="5" y="5" width="70" height="100" rx="5" />
        <circle r="3" cy="105" c="40" />
    </symbol>
</svg>
```

```html
<svg xmlns="http://www.w3.org/2000/svg
    viewBox ="0 0 80 110"
    class="displayed-icon"
    version="1.1"
    xmlns:xlink="http://www.w3.org/1999/xlink">

    <use xlink:href="#phone" />
    <!-- also can be outside source -->
    <use xlink:href="path-to-file.svg#phone" />

</svg>
```

```css
.defined-icon {
    display: none;
}

.displayed-icon {
    height: auto;
    width: 30%;
}

#phone rect, #phone circle {
    fill: white;
}

#phone rect {
    stroke: #FF2626;
    stroke-width: 10px;
}
```

### SVG Accessibility

```html
<svg xmlns="http://www.w3.org/2000/svg
    viewBox ="0 0 80 110"
    class="displayed-icon"
    version="1.1"
    xmlns:xlink="http://www.w3.org/1999/xlink">
    <title>Schmuffle Phone Icon</title> <!-- Label for the asset -->
    <desc> <!-- A detailed description of what the asset looks like -->
        A phone with a large red border with rounded
        corners, a white screen, and a white round
        button centered below the screen.
    </desc>
    <use xlink:href="#phone" />
    <!-- also can be outside source -->
    <use xlink:href="path-to-file.svg#phone" />

</svg>
```













