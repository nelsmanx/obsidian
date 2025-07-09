#fancybox #js #html 

```html
<head>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fancyapps/ui@5.0/dist/fancybox/fancybox.css" />
</head>

<div class="info__gallery">
	<ul class="info__gallery-list">
		<li class="info__gallery-item">
			<a class="info__gallery-link" href="/info-image-1.jpg" data-fancybox="suv-gallery">
				<picture class="info__gallery-pic">
					<img src="/image-1.jpg" alt="Сухие углекислые ванны">
				</picture>
			</a>
		</li>
	</ul>
</div>



<script src="https://cdn.jsdelivr.net/npm/@fancyapps/ui@5.0/dist/fancybox/fancybox.umd.js"></script>

<script>
	Fancybox.bind('[data-fancybox="suv-gallery"]', {
		//Thumbs: false,
		Thumbs: {
	        type: "classic",
	    },
	});
</script>
```

____


### Disable all touch events and vertical drag to close

```js
 Fancybox.bind('[data-fancybox]', {
	dragToClose: false,
    Carousel: {
      Panzoom: {
        touch: false,
      },
    },
  });
```