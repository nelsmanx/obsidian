### Without border-radius

```css
.foo {
    border-style: solid;
    border-width: 3px;
    border-image: linear-gradient(116.79deg, rgba(255, 255, 255, 0.65) 0%, rgba(255, 255, 255, 0));
    border-image-slice: 1;
}
```
___

### With border-radius

```css
button {
    position: relative;
}

button::before {
    content: "";
    position: absolute;
    z-index: -1;
    inset: 0;
    border-radius: 16px;
    border: 2px solid transparent;
    background: linear-gradient(56deg, #eeab0c 0%, #e41d32 98.04%, #e41d32 100%) border-box;
    -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
}

/* or */

button {
	position: relative;
	background-color: #fff;
	
	--border-radius: 8px;
	--border-width: 1px;
	border-radius: var(--border-radius);
}

.presentation__button::before {
	content: "";
	position: absolute;
	inset: calc(-1 * var(--border-width));
	z-index: -1;
	width: calc(100% + calc(2 * var(--border-width)));
	height: calc(100% + calc(2 * var(--border-width)));
	
	border-radius: var(--border-radius);
	background: linear-gradient(-105deg, #E41D32, #EEAB0C)
}
```

