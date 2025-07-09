	#modx

## Создание TV-поля

***Общая информация***
**Название:** faq
**Подпись:** Ответы на вопросы
**Категория:** Ответы на вопросы

***Параметры ввода***
**Тип ввода:** migx
**Замена кнопки "Добавить элемент":** Добавить вопрос

***Доступно для шаблонов***
Выбрать шаблоны, для которых доступно это TV.

**Значение по умолчанию**

```json
[
  {
    "MIGX_id":"1",
    "question":"Вопрос №1",
    "answer":"Ответ №1"
  },
  {
    "MIGX_id":"2",
    "question":"Вопрос №2",
    "answer":"Ответ №2"
  }
]
```

**Вкладки формы**

```json
[
  {
    "fields":[
      {
        "field":"question",
        "caption":"Вопрос"
      },
      {
        "field":"answer",
        "caption":"Ответ",
        "inputTVtype":"textarea"
      },
      {
        "field":"image",
        "caption":"Изображение",
        "inputTVtype":"image"
      },
      {
        "field":"desc-margin",
        "caption":"Добавить отступ снизу у описания",
        "description":"Описание поля",
        "inputTVtype":"checkbox",
        "inputOptionValues": "Да==1",
        "default":"1"  //default value
      },
		  {
	      "field":"birthDate",
	      "caption":"Дата рождения",
	      "inputTV": "dateOnly" // tv without time input
	    }
    ]
  }
]

```

**Разметка колонок**

```json
[
	{
    "header":"ID",
    "sortable":"true",
    "dataIndex":"MIGX_id",
    "show_in_grid":1
  },
  {
    "header":"Вопрос",
    "sortable":"true",
	"width":"300",
    "dataIndex":"question"
  },
  {
    "header":"Ответ",
    "sortable":"true",
	"width":"500",
    "dataIndex":"answer"
  },
  {
    "header":"Изображение",
    "sortable":"false",
		"width":"100",
    "dataIndex":"image",
    "renderer":"this.renderImage"
  },
  {
    "header":"Отступ снизу у описания",
    "sortable":"true",
	"width":"100",
    "dataIndex":"desc-margin",
    "renderer":"this.renderCrossTick"
  },
	{
    "header":"Дата рождения",
    "width":"160",
    "sortable":"true",
    "dataIndex":"birthDate",
    "renderer":"this.renderChunk",
    "renderchunktpl":"[[+birthDate:strtotime:date=`%d.%m.%Y`]]"
	  }

]

**NB:** dataIndex указывает на ключ ****"field" из вкладок форм
```

## Создание чанка для вывода

```html
<div class="question-faq__item">
		<p class="question-faq__question">[[+question]]</p>
		<p class="question-faq__answer">[[+answer]]</p>
	</div>
</div>
```

## Вывод данных через getImageList

```php
<div class="faq__questions-list question-faq__list">
	[[!getImageList? 
	    &tvname=`faq` 
	    &tpl=`faqTpl`
		&docid=`7` // В зависимости от scope может быть [[+id]]
	]]
</div>
```
___

### Вывод порядкового номера элемента

```json
{
    "header":"ID",
    "sortable":"true",
    "dataIndex":"MIGX_id",
    "show_in_grid":1 // <--
  },
```

В чанке выводится через ` [[+idx]] `
!!! Если на странице более одного MIGX, то порядковый номер в каждом новом MIGX не  начинается с единицы, а продолжает последний порядковый номер предыдущего MIGX 

___

### Возможность редактировать поле инлайн

Разметка колонок
```json
"editor":"this.textEditor"
```
___

### Возможность использовать редактор Ace

Вкладки формы
```json
"inputTVtype":"ace"
```
___

### Условия для выборки 

```php
[[!getImageList?
    &where=`{"price-number:=":"1"}`
	    &where=`{"price-number:IN":[1,2]}`
    //Дополнительное условие: checkbox отмечен ("inputOptionValues": "Да==1",)
    &where=`{"price-id:IN":[1,2],"list-price-active:=":"1"}`
    //Дополнительное условие: checkbox не отмечен (вместо единицы пустое значение)
    &where=`{"price-id:IN":[1,2],"list-price-active:=":""}`
]]

// Вывод поля MIGX c другой страницы
<h1 class="doctor__title">
	[[!getImageList?
		&tvname=`migx-doctor-profile-list`
		&where=`{"doctorPageId:=":"[[*id]]"}`
		&tpl=`@CODE: [[+name]]`
		&docid=`[[*doctorPageId]]` 
	]]
</h1>
```


___

### Вывод данных в разных шаблонах

```php
[[!getImageList? 
	&tpl=`@FIELD:template` // template - поле MIGX
]]
```
___

### Сортировка 

```php
[[!getImageList? 
	&sort=`[{"sortby":"list-price-number","sortdir":"ASC"}]`
]]
```
___

### Проверка поля MIGX на пустоту 

Page
```php
[[!pdoResources?
  &parents=`9`
  &tpl=`ourDoctorsItemTpl`
  &includeTVs=`migx-doctor-profile-list`
  &limit=`0`
]]
```

Chunk `ourDoctorsItemTpl`
```php
// {$_modx->unsetPlaceholder('ourDoctorsItemMigxPlaceholder')}
[[!unsetPlaceholder? &placeholder=`ourDoctorsItemMigxPlaceholder`]]

[[!getImageList? 
    &tvname=`migx-doctor-profile-list` 
    &tpl=`ourDoctorsSingleTpl`
	&docid=`[[+id]]`
	&toPlaceholder=`ourDoctorsItemMigxPlaceholder`
]]


[[+ourDoctorsItemMigxPlaceholder:is=``
	:then=`[[$ourDoctorsSingleTpl? 
				&name=`[[+tv.vrach-name]]` 
				&photoDoctorList=`[[+tv.vrach-img-cover]]` 
				&specialtyDoctorList=`[[+tv.vrach-post-cover]]` 
				&doctorPageId=`[[+tv.vrachPageId]]`
			]]`
	:else=`[[+ourDoctorsItemMigxPlaceholder]]`
]]
```

Особенность tv-поля MIGX в том, что его нельзя просто проверить на пустое значение. Сначала нужно вывести результат работы сниппета `getImageList` в `toPlaceholder`, а затем значения `placeholder` проверить на пустоту. Если это делается на странице один раз, то проблем нет, а если же сниппет отрабатывает несколько итераций, то возникает проблема: если хотя бы раз в `placeholder` было записано значение, то если за ним последует пустое значение, оно уже не будет записано в `placeholder` и в нем будет храниться предыдущее не пустое значение. Поэтому проверка `placeholder` на пустоту всегда будет выдавать положительное значение. 
Чтобы этого избежать, перед каждой итерацией нужно сбрасывать значение `placeholder`: 

```php
{$_modx->unsetPlaceholder('ourDoctorsItemMigxPlaceholder')} 

/*
Как оказалось, такой код использовать нельзя.

В MODX, синтаксис `{$_modx->...}` используется для выполнения кода на стороне сервера, используя класс `modX`. Однако в этом конкретном случае, данная строка кода, по всей видимости, интерпретируется как часть chunkа (chunka) или другого элемента, в котором она находится.

Когда MODX пытается обработать эту строку `{$_modx->unsetPlaceholder('ourDoctorsItemMigxPlaceholder')}` как часть chunka, он пытается распарсить её как JSON, что приводит к ошибке `Unexpected token ':'`, так как синтаксис `foo:bar` является недопустимым в JSON.
*/
```

Snippet `unsetPlaceholder`
```php
<?php

$placeholderName = $modx->getOption('placeholder', $scriptProperties, '');

if (!empty($placeholderName)) {
    $modx->unsetPlaceholder($placeholderName);
}

return '';
```

Chunk `ourDoctorsSingleTpl`
```php
<li class="doctors__item">
	<div class="doctors__item-inner">
		<div class="doctors__item-picture-wrap">
			<picture class="doctors__item-picture">
				<img src='[[+photoDoctorList]]' alt="[[+specialtyDoctorList]] [[+name]]">
			</picture>
		</div>
		<p class="doctors__item-specialty">[[+specialtyDoctorList]]</p>
		<p class="doctors__item-name">[[+name:addBreakAfterLastName]]</p>
		<a class="doctors__item-button" href="[[~[[+id]]]]">Записаться на прием</a>
		[[+doctorPageId:notempty=`
		    <a class="doctors__item-button" href="[[~[[+doctorPageId]]]]">Подробнее</a>
		`]]
	</div>
</li>
```
___


[JSON Formatter](https://jsonformatter.curiousconcept.com/# "JSON Formatter & Validator")

### Материалы

[Документация MIGX](https://webstool.ru/documentation-migx-russian.html)
[MIGX | MODX Revo](https://web-revenue.ru/modx-revo/migx)
[Работа с MIGX в MODx Revo](https://dart.agency/blog/obuchenie/rabota-s-migx-v-modx-revo.-chast-3.html)
[MODx Revo, использование MIGX в проектах на примере слайдера](https://dart.agency/blog/modx/modx-revo,-ispolzovanie-migx-v-proektax-na-primere-slajdera.html)