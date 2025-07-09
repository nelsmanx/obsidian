```php
<?php
$available_items = [
	1 => [
		'title' => 'Теплое остекление',
		'price' => 'от 5900 р/м2'
	],
	2 => [
		'title' => 'Остекление в цвет фасада',
		'price' => 'от 5 400 р/м2'
	],
];
?>

<?= $this->render('blocks/available_options', ['available_items' => $available_items]) ?>
```

```php
<?php
$this->registerCssFile('@web/css/available-options.css', ['depends' => 'frontend\assets\AppAsset']);
$this->registerJsFile('@web/js/availableOptions.js', ['depends' => 'frontend\assets\AppAsset']);
?>

<ul class="splide__list">
	<? foreach ($available_items as $key => $item) { ?>

		<li class="splide__slide">
			<h3 class="splide__slide-title"><?= $item['title'] ?></h3>
			<div class="splide__slide-inner">
				<picture class="splide__slide-picture">
					<img src="/images/available-options/<?= $key ?>.jpg" alt="<?= $item['title'] ?>">
				</picture>
				<div class="splide__slide-info">
					<p class="splide__slide-price">Цена от <span class="splide__slide-price-accent"><?= $item['price'] ?></span></p>
					<div class="splide__slide-desc">
						<?= $item['desc'] ?>
					</div>
				</div>
			</div>
		</li>

	<? } ?>
</ul>
```


