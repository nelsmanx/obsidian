```php
<li class="goods__item ms2_product">
	<form class="goods-card ms2_form" method="post">
        <input type="hidden" name="id" value="{$id}"> // или value="[[*id]]"
    	<input type="hidden" name="count" value="1">
    	<input type="hidden" name="options" value="[]">
    	
		<div class="goods-card-inner">
			<div class="goods-card__article">{$article}</div>
			<div class="goods-card__weight {if $paket?}pack{/if}">
				<svg class="goods-card__weight-icon">
					<use xlink:href="#goods-card-weight-icon"></use>
				</svg>

				<div class="goods-card__weight-value">{$weight}</div>
			</div>
			<div class="goods-card__image-wrap" data-modal="productModal-{$id}">
				<img class="goods-card__image" src="{$image | phpthumbof : "w=400&h=400&zc=1"}" alt="{$pagetitle}" title="{$pagetitle}">
			</div>
			<div class="goods-card__actions">
				<div class="goods-card__counter">
					<div class="goods-card__counter-inner">
						<button class="goods-card__counter-plus" type="button"></button>
						<div class="goods-card__counter-value">1</div>
						<button class="goods-card__counter-minus" type="button"></button>
					</div>
				</div>
				<button class="goods-card__button" type="submit" name="ms2_action" value="cart/add">В КОРЗИНУ</button>
			</div>
		</div>
	</form>
</li>
```
___


### Создание опций товара

Опции товаров miniShop2 предназначены для расширения стандартных свойств товара.
Для создания опций перейдите в «Пакеты» -> «miniShop2» -> «Настройки» и перейдите во вкладку «Опции».
___


### Проврека наличия дополнительной опции товара 

Добравление класса при наличии опции
```php
<div class="goods-card__weight {if $paket?}pack{/if}">
```
___

### Вывод дополнительной опции товара 

```php
<p>Состав: {$composition[0]}</p> 
```
___

### Обязательные элементы при выводе  через сниппет msProducts

```php
<ul class="goods__list">
	[[!msProducts?
		&parents=`2`
		&tpl=`goodsTpl`
		&sortby=`weight`
		&sortdir=`ASC`
	]]
</ul>
```

```php
<li class="goods__item ms2_product">
	<form class="goods-card ms2_form" method="post">
        <input type="hidden" name="id" value="{$id}">
    	<input type="hidden" name="count" value="1">
    	<input type="hidden" name="options" value="[]"> // ???    	
    	
		<button class="goods-card__button" 
		type="submit" 
		name="ms2_action" 
		value="cart/add">
		В КОРЗИНУ
		</button>
	</form>
</li>
```
___

### Поиск товаров

[# SimpleSearch — поиск по сайту](https://web-revenue.ru/modx-revo/simplesearch-poisk-po-saytu)
[SimpleSearch поиск только по товарам miniShop2 c сортировкой по цене](https://modx.pro/help/23727)
[miniShop2 и SimpleSearch](https://modx.pro/help/5203)