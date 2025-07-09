```scss 
.foo__list {
    column-count: 2; 
    column-gap: 20px;
}

.foo__item {
	// Avoids any column break within the principal box.
    break-inside: avoid-column; 
}
```

При использовании свойства  `columns`  колонки будут всегда одинаковой ширины 

[Column-count is not working in Chrome](https://stackoverflow.com/questions/41985733/column-count-is-not-working-in-chrome)
```css 
.foo__list {
	display: flex;
}
```






