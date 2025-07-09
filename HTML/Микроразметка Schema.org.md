
### Главные атрибуты schema.org

Семантическая разметка schema.org базируется на трех главных атрибутах:

1. `itemscope` — показывает область действия словаря микроданных и определяет область данных. При помощи этого атрибута поисковый робот понимает, что на странице находится описание определенного объекта. Атрибут `itemscope` всегда используется в связке с атрибутом `itemtype`. 
2. `itemtype` — показывает адрес словаря, задействованного для создания разметки. В нашем случае атрибут `itemtype` будет вести на домен http://schema.org.
3. `itemprop` — позволяет добавить определенное свойство к выбранному элементу. Пара имя-значение.
___

### Микроразметка Menu

```html
<ul itemscope itemtype="http://www.schema.org/SiteNavigationElement">
    <li itemprop="name"><a itemprop="url" href="#">Car Parts</a></li>
    <li class="dropdown">
        <a class="dropdown-toggle" href="#">Car Models</a>
        <ul class="dropdown-menu">
            <li class="nav-item" itemprop="name">
                <a class="dropdown-item nav-link" itemprop="url" href="https://stackoverflow.com">stackoverflow</a>
            </li>
        </ul>
    </li>
</ul>
```
`Itemprop` should be defined on the navigation items and not the dropdown containers. You also do not need to repeat the schema declaration nor do you need to redeclare the `itemscope`.
___

### Микроразметка FAQ

```html
<div itemscope itemtype="http://schema.org/FAQPage">
    <div itemprop="mainEntity" itemscope itemtype="http://schema.org/Question">
        <div itemprop="name">Это вопрос</div>
        <div itemscope itemprop="acceptedAnswer" itemtype="http://schema.org/Answer">
            <span itemprop="text">Здесь размещается ответ на указанный вопрос</span>
        </div>
    </div>
    
    <div itemprop="mainEntity" itemscope itemtype="http://schema.org/Question">
        <div itemprop="name">Это вопрос</div>
        <div itemscope itemprop="acceptedAnswer" itemtype="http://schema.org/Answer">
            <span itemprop="text">Здесь размещается ответ на указанный вопрос</span>
        </div>
    </div>
</div>
```
___

### Микроразметка Breadcrumbs

```html
<div class="breadcrumbs">
	<div class="container">
		<ul class="breadcrumbs__list"  itemscope itemtype="http://schema.org/BreadcrumbList">
			<li class="breadcumbs__item" itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
				<a class="breadcrumbs__link" href="/" itemprop="item">
					<span itemprop="name">Главная</span>
				</a>
				<meta itemprop="position" content="1">
			</li>

			<!-- last item -->
			<li class="breadcumbs__item" itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
				<span itemprop="name">Распил</span>
				<meta itemprop="position" content="2">
			</li>
		</ul>
	</div>
</div>
```
[Структурированные данные для строки навигации](https://developers.google.com/search/docs/appearance/structured-data/breadcrumb?hl=ru#microdata)
___

### Микроразметка Product

```html
<li itemscope itemtype="https://schema.org/Product">
	<span itemprop="name">Товар</span>
	<span itemprop="description">Описание товара</span>
	<img src="http://www.example.com/image.jpg" itemprop="image">
	<div itemprop="offers" itemscope itemtype="https://schema.org/Offer">
		<meta itemprop="price" content="7150.00">
		<meta itemprop="priceCurrency" content="RUB">
		<meta itemprop="availability" href="http://schema.org/InStock">
	</div>
</li>
```
___

### Микроразметка WPHeader

```html
<head itemscope itemtype="http://schema.org/WPHeader">
       <title itemprop="headline"></title>
       <meta itemprop="description" content="описание_страницы">
       <meta itemprop="keywords" content="ключевые_слова_для_страницы">
</head>
```
___

### Микроразметка WPFooter

```html
<footer itemscope itemtype="http://schema.org/WPFooter">
       <meta itemprop="copyrightYear" content="2001">
       <meta itemprop="copyrightHolder" content="ООО 'Контора'">
       <div>© 2001-2024 Все права защищены.</div>
</footer>
```
___

### Ошибка валидатора - Attribute `itemprop` not allowed on element `meta` at this point.

```html
<!DOCTYPE html>
<html itemscope itemtype="http://schema.org/WebPage">
	<head itemscope itemtype="https://schema.org/WPHeader">
	    <meta itemprop="description" name="description" content="my description">
	</head>
</html>
```

Exactly one of the `name`, `http-equiv`, `charset`, and `itemprop` attributes must be specified.

Because the `meta` element in the document in the issue description already has an `itemprop` attribute, when the checker then sees the `name` attribute, it emits the _Attribute `name` not allowed on element `meta` at this point_ error message—because the spec says that a `meta` element can have only one or the other, and the `name` attribute occurs after the `itemprop` attribute.

If the document instead had `<meta name="description" itemprop="description" content="my description">` (that is, with the `itemprop` attribute after the `name` attribute), then the checker would instead emit the message _Attribute `itemprop` not allowed on element `meta` at this point_.
___

[Все примеры микроразметки Schema.org для сайта](https://artzolin.ru/articles/schema-snippets/)
[Микроразметка Schema.org: полное руководство](https://kokoc.com/blog/schema-org-polnoe-rukovodstvo-po-semanticheskoy-razmetke/)
[Google Search Center](https://developers.google.com/search/docs/advanced/structured-data/article)