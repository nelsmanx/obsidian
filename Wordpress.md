#wordpress

```php
// Информация о шаблоне для записи находится в single.php.
// Если у записи есть рубрика, то ID рубрики можно посмотреть в меню "Записи -> Рубрики". Относительно этого ID пишутся условия. ID обязательно ****должны идти в порядке убывания!

if(in_category(14)) {
	include( TEMPLATEPATH.'/pages/himchistka.php' );
} else if(in_category(13)) {
	include( TEMPLATEPATH.'/pages/drugoe.php' );
} else if(in_category(12)) {
	include( TEMPLATEPATH.'/pages/article-page.php' );

// Так же можно создать шаблон и выбирать его в самой записи. Для этого в шаблоне нужно указать его тип:

/*
 * Template name: Отдельный шаблон записи WordPress
 * Template post type: post, page
 */
```

```php
// Можно создавать произвольные поля в записи и затем вызывать их в шаблоне. 
// Например, добавить произвольное поле "h1" и затем вызвать его в шаблоне: 

<div><?=$meta_values['content'][0];?></div>

// Контент, добавленной в верхней части записи через редактор Wordpress Gutenberg вызвается следующим образом:

<?php the_content(); ?>
```

```php
// header по умолчанию вызывается: 
<?php get_header();?>

// Кастомный header вызвается: 
<?php include "new/header2.php"?>
```

```php
<?php if( $post->ID == 454) { ?>
	  <section></section>
<?php } ?>
```

```php
<?php if($meta_values['gallery-id'][0]){?>
	<section></section>
<?php } ?>
```
___


### Вызов ContactForm с дополнительным классом 

```php
<?php echo do_shortcode('[contact-form-7 id="1781" title="hero-services" html_class="hero-services__cta-form"]'); ?>
```
___

### Показать активный шаблон

```php
<?php 
	function show_active_template() {
		global $template;
		print_r( $template );
	}

	show_active_template();
?>
```
---

### Незакрытый тэг в Contac Form 7

Нужно минимизировать разметку формы
___

### Insert PHP Code Snippet
by xyzscripts.com

Для вызова php кода внутри Gutenberg
___

### Подключение по FTP

VSCode, приложение SFTP
```json
{
	"name": "kupioknaszavoda",
	"host": "kupioknaszavoda.ru",
	"protocol": "ftp",
	"port": 21,
	"username": "username",
	"password": "password",
	"remotePath": "/www/kupioknaszavoda.ru/",
	"uploadOnSave": false,
	"useTempFile": false,
	"openSsh": false
}
```
___

### Скрыть/показать панель администратора на сайте

- functions.php
```php
add_filter( 'show_admin_bar', '__return_false');
```

-  `Административная панель WP - Пользователи - Пользователь - Верхняя панель - Показывать верхнюю панель при просмотре сайта`
___

### Вывод полей плагина yoast seo

```php
echo get_post_meta($post->ID, '_yoast_wpseo_metadesc', true);
echo get_post_meta($post->ID, '_yoast_wpseo_title', true);
```
___

### Вывод полей поста

```php
/* Изображение записи */
echo get_the_post_thumbnail_url()
```
___

### Вывод поля без тегов

```php
<meta itemprop="price" content='<?= html_entity_decode(strip_tags($meta_values['hero-subtitle'][0])); ?>'>
```
___

### Проверка на наличие поста в массиве постов
```php
$hero_services_posts = [91, 920, 1302];

if (in_array($post->ID, $hero_services_posts)) {
        include "components/hero-services.php";
    }
```
___

### ACF

```php
<?php $offer = get_field('spoiler-offer', 'option'); ?>
```

Эта строка вызывает функцию `get_field`, которая является частью плагина Advanced Custom Fields (ACF) для WordPress. Функция извлекает значение поля `auto-popup` из опций WordPress и сохраняет его в переменную `$offer`.

- `get_field` - функция плагина ACF для получения значений произвольных полей.
- `'auto-popup'` - это ключ (ID) поля, значение которого мы хотим получить.
- `'option'` - указывает, что поле является частью опций WordPress, а не относится к определенной записи или странице.
- 
Значение поля `auto-popup` находится в боковом меню плагина Advanced Custom Fields (ACF) - `Options Pages` - `Настройки темы` - `Перейти`.  Далее нужно перейти в блок и изменить нужное поле
