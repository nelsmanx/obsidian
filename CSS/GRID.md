#css 

## Three centered items

```css
.foo__list {
    display: grid;
    grid-template-columns: repeat(3, minmax(80px, 280px));
    justify-content: center;
    gap: 20px;
}
```
___

## Three and less centered items in four columns

Ситуация: есть 4 колонки, но вих может быть три и менее элементов. 
Нужно, чтобы они располагались по центру.  
Решение очень негибкое. На каждом брейкпоинте нужно менять максиальную ширину элемента, предполагая, что в ряду находится максимальное количество элементов. 
На брейкпоинте 575 предполагается, что в ряду 2 элемента и их нужно максимально растянуть. 
`repeat(auto-fit, max(235px))` заменяется на `repeat(auto-fit, minmax(210px, 1fr))` и добавляются дополнительные стили элементам. 
Затем вручную ловится брейкпоинт, на котором точно не будет больше одного элемента. 

Тоже самое можно сделать проще с помощью flex-box, но тогда все элементы всегда будут располагаться по центру, то есть когда 4 элемента встанут в ряд по 3, образуется буква Т

```css
	.medical-referrals__list {
		display: grid;
		grid-template-columns: repeat(auto-fit, max(303px));
		justify-content: center;
		gap: 20px;
	}
	
	@media (max-width: 1399.98px) {
		.medical-referrals__list {
			grid-template-columns: repeat(auto-fit, max(255px));
		}
	}

	@media (max-width: 1199.98px) {
		.medical-referrals__list {
		    grid-template-columns: repeat(auto-fit, max(210px));
		    gap: 15px;
		}
	}
	
	@media (max-width: 767.98px) {
		.medical-referrals__list {
			grid-template-columns: repeat(auto-fit, max(235px));
		}
	}
	
	@media (max-width: 575.98px) {
		.medical-referrals__list {
		    grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
		}
		
		.medical-referrals__item {
			justify-self: center;
			width: 100%;
			max-width: 300px;
			min-height: auto;
		}
	}
	
	@media (max-width: 489.98px) {
		.medical-referrals__list {
			grid-template-columns: 100%;
		}
	}

```

Полный код блока 

```html
<section class="medical-referrals">
		<div class="container">
			<h2 class="medical-referrals__title">Другие медицинские направления</h2>
			<ul class="medical-referrals__list">
				<li class="medical-referrals__item">
					<p class="medical-referrals__item-title">Бронхиальная астма</p>
					<a class="medical-referrals__item-link" href="">Подробнее</a>
				</li>
				<li class="medical-referrals__item">
					<p class="medical-referrals__item-title">Бронхит</p>
					<a class="medical-referrals__item-link" href="">Подробнее</a>
				</li>
				<li class="medical-referrals__item">
					<p class="medical-referrals__item-title">Пневмония</p>
					<a class="medical-referrals__item-link" href="">Подробнее</a>
				</li>
				<li class="medical-referrals__item">
					<p class="medical-referrals__item-title">Хроническая обструктивная болезнь легких (ХОБЛ)</p>
					<a class="medical-referrals__item-link" href="">Подробнее</a>
				</li>
			</ul>
		</div>
	</section>


	<style>
		.medical-referrals {
			padding: 80px 0 60px;
		}

		.medical-referrals__title {
			margin-bottom: 60px;
			font-size: 32px;
			font-weight: 600;
			line-height: normal;
			text-align: center;
			color: #2d2b5c;
		}

		.medical-referrals__list {
			display: grid;
			grid-template-columns: repeat(auto-fit, max(303px));
			justify-content: center;
			gap: 20px;
			margin: 0;
			padding: 0;
			list-style: none;
		}

		.medical-referrals__item {
			display: flex;
			flex-direction: column;
			min-height: 155px;
			margin-bottom: 0 !important;
			padding: 12px;
			border-radius: 5px;
			background-color: #f7f7f7;
		}

		.medical-referrals__item:hover {
			background-color: #f3fdff;
			transition: background-color 250ms ease-in-out;
		}

		.medical-referrals__item-title {
			flex: 1 1 auto;
			margin-bottom: 30px;
			font-size: 16px;
			font-weight: 700;
			line-height: 1.875;
			text-align: center;
			color: #00a1be;
		}

		.medical-referrals__item-link {
			justify-self: center;
			display: flex;
			justify-content: center;
			align-items: center;
			width: fit-content;
			margin: 0 auto;
			padding: 1em 1.5em;
			font-size: 16px;
			font-weight: 600;
			line-height: normal;
			text-decoration: none;
			color: #00a1be;
			background-color: #f7f7f7;
			border: 1px solid #00a1be;
		}

		.medical-referrals__item:hover .medical-referrals__item-link {
			background-color: #00a1be;
			color: #fff;
			transition: background-color 250ms ease-in-out, color 250ms ease-in-out;
		}

		@media (max-width: 1399.98px) {
			.medical-referrals__list {
				grid-template-columns: repeat(auto-fit, max(255px));
			}
		}

		@media (max-width: 1199.98px) {
			.medical-referrals__list {
				grid-template-columns: repeat(auto-fit, max(210px));
				gap: 15px;
			}

			.medical-referrals__item-title {
				font-size: 14px;
				line-height: 1.5;
			}

			.medical-referrals__item-link {
				font-size: 14px;
			}
		}

		@media (max-width: 991.98px) {
			.medical-referrals {
				padding: 60px 0;
			}

			.medical-referrals__title {
				margin-bottom: 40px;
				font-size: 28px;
			}
		}

		@media (max-width: 767.98px) {
			.medical-referrals__list {
				grid-template-columns: repeat(auto-fit, max(235px));
			}
		}

		@media (max-width: 575.98px) {
			.medical-referrals {
				padding: 40px 0;
			}

			.medical-referrals__title {
				margin-bottom: 30px;
				font-size: 24px;
			}

			.medical-referrals__list {
				grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
			}

			.medical-referrals__item {
				justify-self: center;
				width: 100%;
				max-width: 300px;
				min-height: auto;
			}
		}

		@media (max-width: 489.98px) {
			.medical-referrals__list {
				grid-template-columns: 100%;
			}
		}
	</style>
```
___

### Span all columns

```css
.foo__item {
	grid-column: 1/-1;
}
```
___

### Grid-template-columns: auto-fit vs auto-fill

```css
grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
```

Auto-fit заполняет столбцами все доступное пространство в отличие от auto-fill. Вторым аргументом minmax должен быть 1fr или auto.