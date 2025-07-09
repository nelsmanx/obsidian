#html 

```html
<div class="single-video__wrap">
	<div class="single-video aspect-ratio">
		<iframe src="https://www.youtube.com/embed/YrRZyOerzSg" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
	</div>
</div>
```

```css
.aspect-ratio iframe {
	position: absolute;
	inset: 0;
	display: block;
	width: 100%;
	max-width: 100%;
	height: 100%;
	max-height: 100%;
	object-fit: cover;
	object-position: center;
	margin: 0;
}

.single-video__wrap {
	max-width: 800px;
	margin-left: auto;
	margin-right: auto;
}

.single-video.aspect-ratio {
	padding: calc(100% / (16 / 9)) 0 0;
}
```