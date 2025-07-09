#html #css

```html
<picture class="gallery__pic aspect-ratio">
		<source srcset="/assets/images/img-1.webp">
		<img src="/assets/images/img-1.jpg" alt="">
</picture>
```

```scss
@mixin aspect-ratio($width, $height) {
	padding: calc(100% / ($width / $height)) 0 0;
}

.aspect-ratio {
	position: relative;
	display: block;
	width: 100%;
	height: 0;
}

.aspect-ratio img {
	position: absolute;
	inset: 0;
	display: block;
	width: 100%;
	max-width: 100%;
	height: 100%;
	max-height: 100%;
	object-fit: cover;
	object-position: center;
}

.gallery__pic.aspect-ratio {
	@include aspect-ratio(16, 9);
		padding: calc(100% / (16 / 9)) 0 0;
}
```



### Picture with media source
```html
<picture class="banner__background">
	<source srcset="assets/images/banner-medium.jpg" media="(max-width: 1400px)">
	<img src="assets/images/banner.jpg" alt="">
</picture>
```