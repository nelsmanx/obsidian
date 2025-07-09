#modx

## Using modxMinify

```jsx
<link rel="stylesheet" href="[[!modxMinify?group=`css`]]">
<script src="[[!modxMinify?group=`js`]]"></script>
```

## pdoResources template

```php

	[[!pdoResources?
			&sortbyTV=`HitsPage`
			&sortdirTV=`DESC`
			&tpl=`popularItem`
			&limit=`12`
			&parents=`0`
			&includeTVs=`price,HitsPage,image,unit,from`
			&tvPrefix=``
			&where=`{"template:=":3}`
	]] 	

Вывод dropdownmenu
[[!pdoResources?
   &parents=`3`
   *&depth=`0`*
   &tpl=`@INLINE <li class="nav__link-dropdown-item">
				   <a href="[[+uri]]">[[+longtitle]]</a>
				 </li>`
]]

```


## Вывод даты публикации

```php
<div class="news__date">[[+publishedon:date=`%d %B %Y`]]</div>
```


## Вывод текущей даты

```php
[[!+nowdate:default=`now`:strtotime:date=`%Y-%m-%d %H:%M:%S`]]
```
Мы используем несуществующий плейсхолдер, присваиваем ему значение по умолчанию now, которое представляет из себя текущее время в формате unix метки (unix timestamp). После чего форматируем данные так, как нам надо для их вывода.

## Ввод даты в tv в формате d-m-Y

`Системные настройки & События - manager_date_format - d-m-Y`


## Значение tv по умолчанию

```php
[[+expertID:default=`2`]]
```
___

### Отличие `[[+id]]` от `[[*id]]` 

`[[+id]]` - id текущего документа при переборе документов сниппетом (pdoResources, Wayfinder, pdoMenu...)
`[[*id]]` - id документа, открытого напрямую (документ обрабатывается НЕ внутри какого-то итератора, типа pdoResources)
___

### Выходной фильтр/модификаторы

Добавление определенного типа файла на страницу
```php
:cssToHead     
:htmlToHead    // before </head>
:htmlToBottom  // before </body>
:jsToHead      // before </head>
:jsToBottom    // before </body>

[[*id:input=`<link rel="stylesheet" href="/css/style.css">`:cssToHead]]
[[*id:input=`<style></style>`:htmlToHead]]

[[*id:input=`<script src="/assets/js/lazyVideo.js"></script>`:jsToBottom]]
```
[Output Filter/Modifiers](https://docs.modx.com/current/en/building-sites/tag-syntax/output-filters)


### Snippet: phoneHandler

`System Settings & Events` - `Create` - `phone`

```php
<?php

if (!empty($input)) {
    $input = preg_replace("/[^0-9]/", '', $input); 
    if (substr($input,0,1) == 8) $input[0] = 7;
}

return '+'.$input;
```

```html
<a href="tel:[[++phone:phoneHandler]]" class="phone">[[++phone]]</a>
```
___


### Ошибка при отправке формы FormIt

```
(ERROR @ [FormIt] Произошла ошибка при попытке отправить почту. Ошибка соединения с SMTP-сервером https://github.com/PHPMailer/PHPMailer/wiki/Troubleshooting
```

`System Settings & Events` -  `Use SMTP` - `No`
___

### Использование параметра в вызове чанка

```php
[[$goods? &colorScheme=`blue`]]

<section class="goods [[+colorScheme:notempty=`color-scheme--[[+colorScheme]]`]]">
```

При этом код ниже не работает. Скорее всего, страницы, на которых нет tv-поля `banner-title`, будет происходить ошибка и чанк не будет рендериться. 

```php
[[$back-wrap6? &banner-title=`Врачи` &banner-button-text=`Записаться на прием`]]

<h1>[[+banner-title:is=``:then=`[[*longtitle]]`:else=`[[[+banner-title]]`]]</h1>
```
___

### Модификатор :notempty

```php
[[+modifier:notempty=`goods--[[+modifier]]`]]
```
___

### Модификатор :default

Если TV-поле не заполнено, то выводится дефолтное значение
```php
[[+table-title:default=`СТОИМОСТЬ УСЛУГ`]]
```
___

### Вывод заголовка с другой страницы

```php
[[!pdoField?id=`[[+resource-id]]`&field=`pagetitle`]]
```
___

### Проверка на наличие в массиве значений

```php
[[+parent:in=`29,30`:then=`src="/images/goods.svg" `]]
```
___

### PHP ThumbOf

```php
<img src="
[[!phpthumbof? &input=`/images/certs/econom-cert-1.jpg` &options=`&w=350`]]" 
alt="Паспорт безопасности Raceguard Econom" 
title="Паспорт безопасности Raceguard Econom">
```
___

### Вывод tv с определенной страницы

```php
[[#1.tv.header-promo:isnot=``:then=`
	<div class="header__promo">
		<div class="container container--lg">
			<p class="header__promo-title">[[#1.tv.header-promo]]</p>
		</div>
	</div>
`]]
```
___

## Разместить TV выше контента

`Системные настройки & События - tvs_below_content - нет`
___

### Вывод в плейсхолдер

Вывод в плейсхолдер должен быть раньше его использования
```php
[[*id:is=`10`:then=`стоматолога`:toPlaceholder=`t-title`]]

<h2>СТОИМОСТЬ УСЛУГ[[+t-title:notempty=` [[+t-title]]`]]:</h2>
```

Но такая проверка не будет работать 
```php
[[+id:is=`701`:then=`/nevrolog/`:toPlaceholder=`serviceLink`]]
[[+id:is=`700`:then=`/kardiolog/`:toPlaceholder=`serviceLink`]]
```
В плейсхолдере всегда будет находиться значение из последнего сработавшего условия.  То есть несмотря на то, что проверка на 701 будет пройдена в карточке вывода будет пусто, потому что за ним проходится следующая провка на 700. Как бы плейсхолдер созадается один на всю страницу 

___

### Первый элемент в массиве

```php
[[+goodsPacket.0]]
```
___

### Gallery не отображаются фото

```php
// Add backticks core/components/gallery/processors/mgr/item/getlist.class.php

31. public $defaultSortField = '`rank`';
66. $c->groupBy('`rank`');
```
[community.modx.com](https://community.modx.com/t/solved-problem-persists-with-gallery-2-0-0-not-displaying-images-in-manager/5558/2)
___

### Использование `@INLINE` чанка  в `pdoResources`

Условные модификаторы вывода могут не сработать при использовании `@INLINE`
```php
[[!pdoResources?
	&tpl=`@INLINE
	[[+formatted:isnot=``:then=`[[+formatted]]`:else=`[[+menutitle]]`]]`
]]
```

Необходимо использовать отдельный чанк
```php
[[!pdoResources?
	&tpl=`itemTpl`
]]
```
___

### Вывод полного адреса изображения 

```php
[[++site_url]][[*image]]
```
___

### Проверка на пустое значение tv-поля

```php
[[+tv.vrachPageId:notempty=`Поле не пустое`]]
```
___

### Fenom if

```php
{if $goodsPackage[0] == 'bucket' && $weight == 4} {set $imageScale = 0.75} {/if}

<img class="goods-card__image{if $goodsOutOfStock[0] == true} disabled{/if}" 
	{if $imageScale}style="--image-scale: {$imageScale}"{/if}
>
```

Вывод через pdoResources
```php
{if $id == 700} {set $service_link = '/kardiolog/'} {/if}
{if $id == 701} {set $service_link = '/nevrolog/'} {/if}
```

В этом случае `{$id}` - это id документа, который попадает в `pdoResources`, а `$resource.id` - id страницы, на которой выводится чанк. Чтобы посмотреть все поля, которые можно использовать в чанке, нужно вызвать `pdoResources` с пустым `tpl`

___