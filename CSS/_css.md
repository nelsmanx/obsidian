#css 

### Flip an image (add a mirror effect)
```css
.foo { 
	transform: scaleX(-1);
}
```

### Box-shadow as border
```css 
.bar {
	box-shadow: 0 0 0 3px #fff;
}
```

### Box-shadow inside over image
```css
.parent {
	box-shadow: 0px 0px 0px 1px red inset;
}

.image {
	mix-blend-mode: multiply;
}
```

### Multiple backdrop filters

```css
backdrop-filter: blur(7px) opacity(80%);
```
___

### Background shadow

```css
.item {
    background: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)),          url('/assets/img/product/pic1.jpg');
}

.doctor-banner {
	background: linear-gradient(rgba(255, 255, 255, 0.25), rgba(255, 255, 255, 0.25));
	background-color: #00A1BE;
			}
```
___

### Multiple backgrounds centered

```css
.hero {
    background-image:
        url(/img/about/about-hero-bg-img.png),
        url(/img/about/about-hero-bg-icon.svg);
    background-size:
        473px 533px,
        616px 517px;
    background-position:
        calc(50% + 360px) 100%,
        calc(50% + 324px) calc(100% - 20px);
    background-repeat: no-repeat, no-repeat;
    background-color: var(--about-primary-color);
}
```
___

### Combine background-image with gradient

```css
.promo {
	background:
		url(/images/promo-idx/background.jpg) 50% top/cover no-repeat,
		linear-gradient(270deg, rgba(217, 217, 217, 0) 35.05%, rgba(217, 217, 217, 0.7) 75.71%);
}
```
___


### Background image encoded SVG

```css
.advantages.commerce {
	background-image: url('data:image/svg+xml,<svg ...><path... /></svg>');
}

.advantages.commerce {
	background-image: url('data:image/svg+xml,<svg fill="%23fff" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg" width="1920" height="685" viewBox="0 0 1920 685"><path d="M0,316.814S149.335,284.89,361.435,285c103.925,0.054,223.39,3.6,348.456,12.925,104.879,7.819,401.559,29.74,439.309,32.808,97.71,7.941,189.57,13.872,279.57,13.919C1704.65,344.8,1920,318,1920,318V927.25S1440.24,969.5,960.5,970C481.74,970.5,0,927,0,927V316.814Z" transform="translate(0 -285)"/></svg>');
}
```

Note that the SVG content needs to be url-escaped for this to work, e.g. `#` gets replaced with `%23`.
No line-break!
___


### Взаимодействовие сквозь объект

```css
div {
	pointer-events: none;
}
```
___

### Запрет выделения текста

```css
div {
	user-select: none;
}
```
___

### Upscaling QR 

```css
img {  
	image-rendering: pixelated;  
}
```
___

### Плавное скрытие/появление

```css
.item.is-hidden {
    max-height: 0;
    opacity: 0;
    margin: 0;
}
.item {
    max-height: none;
    opacity: 1;
    margin: initial;
}
```
___

### `<hr>` classes

```html
<hr class="separator">

<style> 
	.separator {
		height: 0; 
		border: none; 
		border-bottom: 1px solid black;
	}
</style>
```
___

### Подчеркивание сквозь текст

The text-decoration-skip-ink CSS property specifies how overlines and underlines are drawn when they pass over glyph ascenders and descenders.

```css
p {  
	text-decoration-skip-ink: none;
	/* or */
	text-decoration-skip-ink: auto;
}
```
___


### Скрытие стрелок input type="number"

```scss
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
.input-number {
  -moz-appearance: textfield;
}

.input-number {
	-moz-appearance: textfield;
	&::-webkit-inner-spin-button, 
	&::-webkit-outer-spin-button { 
		-webkit-appearance: none; 
		border: none;
	}
}
```


### [Why can't an element with a z-index value cover its child?](https://stackoverflow.com/questions/54897916/why-cant-an-element-with-a-z-index-value-cover-its-child)

### height 100%

```css
.section {
	display: flex;
	min-height: 1200px;
}

.container {
	height: inherit;
}
```

Можно использовать display: flex; flex-direction: column; Newstarcamp

Если 2 колонки, слева контент, а справа должна быть картинка высотой как у контента слева, то картинку можно обернуть в блок с position: relative,  а картнику сделать с абсолютным позиционированием Tigor 