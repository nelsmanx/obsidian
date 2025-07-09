#php 

### Print information about a variable
```php
<? foreach ($products as $key => $product) {
	print_r($product);
	// echo ($this->render('components/product-item', compact('product')));
} ?>
```
___

### Additional class if items quantity less than 3 
```php
<ul class="article-cards__list <?php if (count($articles) < 3) echo 'article-cards__list--short' ?>">

<li class="<?= ($key == 0) ? 'active' : '' ?>">
```

___
