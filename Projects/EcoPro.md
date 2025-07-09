	
Шаблон главной        `«New main page» (pages/index2.php)`
CSS                              `/wp-content/themes/ecopro/assets/css/newcss.css`
CSS @media               `/wp-content/uploads/assets/css/media.css`
JS                                `/wp-content/themes/ecopro/src/js/new/main.js `
___


### Записи

Используется шаблон `«Article page» (pages/article-page.php)`
Использование роизвольных полей
```php
<li class="breadcrumb-item">
		<span itemprop="name"><?=$meta_values['title'][0];?></span>
		<meta itemprop="position" content="3">
</li>
```

```php
<div class="caption_body">
	<h1 itemprop="headline"><?=$meta_values['title'][0];?></h1>
	<meta itemprop="image" content="<?=$meta_values['img'][0];?>">
	<meta itemprop="description" content="<?=$meta_values['introtext'][0];?>">
</div>
```
___


### Шаблон по умолчанию

```php
// EcoPro: single.php

if(in_category(14)) {
	include( TEMPLATEPATH.'/pages/himchistka.php' );
} else if(in_category(13)) {
	include( TEMPLATEPATH.'/pages/drugoe.php' );
} else if(in_category(12)) {
	include( TEMPLATEPATH.'/pages/article-page.php' );
} else if(in_category(9)) {
	include( TEMPLATEPATH.'/pages/services-office-page.php' );
} else if(in_category(4)) {
	include( TEMPLATEPATH.'/pages/cases-page.php' );
} else if(in_category(3)) {
	include( TEMPLATEPATH.'/pages/services-page.php' );
}
```

Узнать категорию можно в разделе "Рубрики" - ID
