### Path Aliases List
```php
 @web: Your application base url path
```
___
### Передача параметров в renderFile

```php
<?= $this->renderFile('@frontend/views/site/glz-products.php', ['category_id' => 79, 'h2_title' => 'Примеры наших работ']) ?>
```

```php
if (isset($category_id)) {
	...
}

<section class="services <?= isset($services_class_modifier) ? "services--{$services_class_modifier}" : '' ?>">

<div class="service-item-mobile-col col-md-4 col-sm-6 col-xs-6" style="<?= !in_array($key, $active_services) ? 'display: none;' : '' ?>">
```
___

### The directory is not writable by the Web process: project/web/assets

```bash
chmod 777 /project/web/assets
```

Нужно также проверить права на проект и при необходиости дать разрешение `full controll`
