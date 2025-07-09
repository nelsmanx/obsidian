#migx

### Resource

```php
<div class="color-palette">
	<div class="container">
    <h1 class="color-palette__title">Таблица цветов и фактур натяжных потолков</h1>
	    
    	[[!getImageList? 
    	    &tvname=`color-palette-list`
    	    &tpl=`color-palette-list-tpl`
    	]]
	
	</div>
</div>
```
___

### Chunk: color-palette-list-tpl

```php
<h2 class="color-palette__subtitle">[[+list-title]]</h2>
<ul class="color-palette__list">

	[[!getImageList? 
		// !!! instead of tvname, not name of nested MIGX!!!
	    &value=`[[+list-color-palette]]` 
	    &tpl=`color-palette-item-tpl`
	]]
	
</ul>
```
___

### Chunk: color-palette-item-tpl

```php
<li class="color-palette-item">
	<picture class="color-palette__item-pic">
		<img src="[[+item-image]]" alt="Цвет">
	</picture>
	<p class="color-palette__item-title">[[+item-title]]</p>
</li>
```
___

### TV: color-palette-list

#### Form Tabs
```json
[
  {
    "fields":[
      {
        "field":"list-title",
        "caption":"Название палитры"
      },
      {
        "field":"list-color-palette", // uses in '&value'
        "caption":"Цвета палитры",
        "inputTV":"color-palette-item"
      }
    ]
  }
]
```

#### Grid Columns
```php
[
  {
    "header":"Название палитры",
    "dataIndex":"list-title",
    "width":"500"
  }
]
```

```
'Caption: 'Список палитр'
'Add item' replacement: 'Добавить палитру'
'Tepmlate access': 'Палитра цветов'
```
___

### TV: color-palette-item

#### Form Tabs
```json
[
  {
    "fields":[
      {
        "field":"item-title",
        "caption":"Номер",
        "inputTVtype":"number"
      },
      {
        "field":"item-image",
        "caption":"Изображение",
        "inputTV":"color-palette-item-image"
      }
    ]
  }
]
```

#### Grid Columns
```php
[
  {
    "header":"Номер",
    "sortable":"true",
    "width":"100",
    "dataIndex":"item-title"
  },
  {
    "header":"Изображение",
    "sortable":"false",
    "width":"300",
    "dataIndex":"item-image",
    "renderer":"this.renderImage"
  }
]
```

```
'Caption: 'Образец цвета'
'Add item' replacement: 'Добавить образец цвета'
'Tepmlate access': '[]'
```
___

### TV: color-palette-item-image

```
Параметры ввода - Тип ввода: Изображение
                  Значение по умолчанию: images/color-palette/glossy/glossy-100.jpg 
 
```
___

### Вывод вложенных элементов через PDOPage

``` php
[[!getImageList? 
	&tvname=`color-palette-list` 
	&tpl=`color-palette-list-tpl-ajax`
	&docid=`168`
	&where=`{"list-title:=":"[[+palette-type]]"}`
]]

<div class="color-palette__list-wrap">
    [[!+page.nav]]
    
    <ul class="color-palette__list">
        [[!pdoPage?
            &element=`getImageList`
            &value=`[[+list-color-palette]]`
            &tvname=`color-palette-list`
            &tpl=`color-palette-item-tpl`
            &limit=`12`
            &ajaxMode=`button`
            &ajaxElemWrapper=`.color-palette__list-wrap` 
            &ajaxElemPagination=`.color-palette__list-wrap .pagination` 
            &ajaxElemLink=`.color-palette__list-wrap .pagination a` 
            &ajaxElemRows=`.color-palette__list`
            &ajaxElemMore=`.color-palette__button`
            &ajaxTplMore=`@INLINE <button class="color-palette__button">ПОКАЗАТЬ ВСЮ ПАЛИТРУ</button>`
        ]]
    </ul>
</div>

 
```
___