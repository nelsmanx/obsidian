#html #css  #list 

```html
<ol class="info__list info__list--ordered">
	<li class="info__li">
		Аппаратные и тепловые физиопроцедуры:
		<ul class="info__list">
			<li class="info__li info__li--nested">Электрофорез</li>
			<li class="info__li info__li--nested">Лазеролечение</li>
			<li class="info__li info__li--nested">Магнитотерапия</li>
			<li class="info__li info__li--nested">Лазеротерапия</li>
			<li class="info__li info__li--nested">Парафинотерапия</li>
			<li class="info__li info__li--nested">Грязелечение</li>
			<li class="info__li info__li--nested">Водолечение</li>
		</ul>
	</li>
	<li class="info__li">
		Массажи:
		<ul class="info__list">
			<li class="info__li info__li--nested">Ручной классический массаж</li>
			<li class="info__li info__li--nested">Аппаратный массаж</li>
		</ul>
	</li>
	<li class="info__li">
		Лечебная физкультура:
		<ul class="info__list">
			<li class="info__li info__li--nested">Первый пункт</li>
			<li class="info__li info__li--nested">Второй пункт</li>
		</ul>
	</li>
</ol>
```


```css
ul,	ol {
	margin: 0;
	padding: 0;
	list-style: none;
}

.info__list--ordered {
	list-style-type: decimal;
	padding-left: 19px;
}

.info__li {
	font-size: 16px;
	font-weight: 500;
	line-height: 2;
	color: #1a1934;
}

.info__li--nested {
	position: relative;
	padding-left: 23px;
	font-weight: 400;
}

.info__li--nested::before {
	content: '';
	position: absolute;
	top: 15px;
	left: 10px;
	display: inline-block;
	width: 2px;
	height: 2px;
	border-radius: 50%;
	background-color: #2d2b5c;
}

@media (max-width: 575px) {
	.info__li {
		line-height: 1.7;
	}

	.info__li--nested {
		padding-left: 10px;
	}

	.info__li--nested::before {
		top: 13px;
		left: 0px;
	}
}
```