
Всегда необходимо указывать путь относительно текущего файла

```
project/ 
	|- index.html
	|- css/
		|- fonts.css
		|- style.css
	|- fonts/
		|- MuseoSans.woff2
	|- icons/
		|- logo.svg
	|- img/
		|- background.png
	|- scss/
		|- main.scss
```


```html
index.html

<link rel="stylesheet" href="css/style.css">
<img class="image" src="icons/logo.svg" alt="logo">
```

```css
style.css 

.hero {
     background-image: url(../img/background.png);
}
```

```css
fonts.css

@font-face {
    src: url('../fonts/MuseoSans.woff2');
}
```

```css
main.scss

@import url("../css/fonts.css");
```