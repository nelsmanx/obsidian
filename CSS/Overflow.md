![[css-overflow.png]]

```html
<div class="splide__slide-inner">
	<picture class="splide__slide-picture"></picture>
	<div class="splide__slide-info">
		<p class="splide__slide-price"></p>
		<div class="splide__slide-desc">
			<p class="splide__slide-desc-par"></p>
		</div>
	</div>
</div>
```

```css
.splide__slide-inner {
	display: grid;
	grid-template-columns: 540px 1fr;
	gap: 30px;
	height: 430px;
}

.splide__slide-info {
	display: flex;
	flex-direction: column;
	overflow: hidden; /* без этого не сработало*/
}

.splide__slide-desc {
	flex: 1 1 auto;
	overflow-y: auto;
}
```