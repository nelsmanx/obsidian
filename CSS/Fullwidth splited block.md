```html
<div class="positions">
	<div class="positions__background"></div>
	<div class="container">
		<div class="positions__content"></div>
	</div>
</div>
```

```css
.positions {
		position: relative;
	}

	.positions__background {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		position: absolute;
		z-index: -1;
		inset: 0;
		width: 100%;
		height: 100%;
	}

	.positions__background::before {
		content: '';
		background-color: #ffe4ea;
	}

	.positions__background::after {
		content: '';
		background-color: #fff;
	}
```

![[samokat2kolesa.ru_vakansii-kurera-zhenshchiny_.png]]