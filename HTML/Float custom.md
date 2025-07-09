`row-float__right`,  `row-float__left` всегда должны быть выше `row-float__text`

```html
<div class="row-float">
	<div class="row-float__right">
		<img src="" alt="">
	</div>
	<div class="row-float__text">
		<p>Текст</p>
	</div>
</div>
```


```css
/* *** float styles *** */
.row-float {
	--float-el-width: 270px;
	--float-el-margin-around: 10px
}

.row-float:after {
	content: '';
	display: table;
	clear: both;
}

.row-float__right,
.row-float__left {
	position: relative;
	z-index: 1;
	width: var(--float-el-width);
}

.row-float__right {
	float: right;
	margin: 0 0 0 var(--float-el-margin-around);
}

.row-float__left {
	float: left;
	margin: 0 var(--float-el-margin-around) 0 0;
}

.row-float__text {}

@media (max-width: 767.98px) {
	.row-float {
		--float-el-width: 200px;
	}
}

@media (max-width: 575.98px) {
	.row-float {
		display: flex;
		flex-direction: column;
	}

	.row-float:after {
		display: none;
	}

	.row-float__right,
	.row-float__left {
		position: static;
		z-index: auto;
		float: none;
		width: 100%;
		margin: 10px 0 0;
		order: 1;
	}

	.row-float__text {
		order: -1;
	}
}
```