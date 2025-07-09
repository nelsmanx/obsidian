```vue
<calc-option v-model="options.ruchnoyPodyemNaEtazh" :title="'Ручной подъем на\u00A0этаж'"></calc-option>
```
___

### Swiper nav styles in Splide style

```css
#swiper-hero .swiper-nav {
	position: absolute;
	z-index: 2;
	top: 50%;
	right: 38px;
	left: 38px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	height: 0;
}

#swiper-hero .swiper-nav-prev,
#swiper-hero .swiper-nav-next {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 54px;
	height: 54px;
	padding: 0;
	border: none;
	border-radius: 50%;
	cursor: pointer;
}

#swiper-hero .swiper-nav-prev {
	background: linear-gradient(90deg, rgba(30, 30, 30, 1) 0%, rgba(35, 36, 38, 1) 100%);
}

#swiper-hero .swiper-nav-next {
	background: #3e3e42;
}

#swiper-hero .swiper-nav-prev:before,
#swiper-hero .swiper-nav-next:before {
	content: '';
	width: 8px;
	height: 15px;
	background-size: contain;
	background-repeat: no-repeat;
	background-position: center;
}

#swiper-hero .swiper-nav-prev:before {
	background-image: url("data:image/svg+xml,%3Csvg width='8' height='15' viewBox='0 0 8 15' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M7.09437 0.155454C7.19945 0.0499659 7.32665 -9.53674e-07 7.47045 -9.53674e-07C7.61424 -9.53674e-07 7.74144 0.0499659 7.84653 0.155454C8.05116 0.360876 8.05116 0.699549 7.84653 0.904971L1.27619 7.5007L7.84653 14.0964C8.05116 14.3018 8.05116 14.6405 7.84653 14.8459C7.64189 15.0514 7.30453 15.0514 7.0999 14.8459L0.153474 7.87267C-0.051158 7.66725 -0.051158 7.32858 0.153474 7.12316L7.09437 0.155454Z' fill='white'/%3E%3C/svg%3E");
}

#swiper-hero .swiper-nav-next:before {
	background-image: url("data:image/svg+xml,%3Csvg width='10' height='17' viewBox='0 0 10 17' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M1.11671 16.6395C0.998472 16.7577 0.855339 16.8137 0.693534 16.8137C0.531729 16.8137 0.388594 16.7577 0.270352 16.6395C0.0400911 16.4092 0.0400911 16.0296 0.270352 15.7993L7.66359 8.40606L0.270352 1.01284C0.0400911 0.782579 0.0400911 0.402956 0.270352 0.172696C0.500613 -0.0575652 0.880231 -0.0575652 1.11049 0.172696L8.92691 7.98911C9.15717 8.21938 9.15717 8.599 8.92691 8.82926L1.11671 16.6395Z' fill='white'/%3E%3C/svg%3E%0A");
}
```
___

### Multiple data-v attribute, scoped styles

With `scoped`, the parent component's styles will not leak into child components. However, a child component's root node will be affected by both the parent's scoped CSS and the child's scoped CSS. This is by design so that the parent can style the child root element for layout purposes.

```vue
<form data-v-f65d12bb data-v-995dcb0d class="order cart__order cart__order">
```

Родительский `data-v` не присвоится, если в компоненте есть несколько корневых элементов

If your component has multiple root elements, you would need to define which element will receive this class. You can do this using the `$attrs` component property
```vue
<!-- MyComponent template using $attrs -->
<p :class="$attrs.class">Hi!</p>
<span>This is a child component</span>

<MyComponent class="baz" />

Will render:
<p class="baz">Hi!</p>
<span>This is a child component</span>
```
___

### Inputmask

`/plugins/inputMask.js`

```js
import Inputmask from "inputmask";

const vInputmask = {
	mounted: (el, binding) => Inputmask(binding.value).mask(el)
};

const inputmaskTelOptions = {
	"mask": "+7 (\\999) 999-99-99",
	"showMaskOnHover": false
}

<input v-inputmask="inputmaskTelOptions">

// or

const vInputmaskTel = {
	mounted: (el) => Inputmask({
		"mask": "+7 (\\999) 999-99-99",
		"showMaskOnHover": false
	}).mask(el)
};

<input v-inputmask>
```


### Динамический класс 

```js
const classModifier = ref('');
```

```html
<main :class="{ [`main--${classModifier}`]: classModifier }">

<div class="menu-mobile" 
	 :class="['menu-mobile', { 'menu-mobile--active': isMobileMenuActive }]"
>	
```

Если `classModifier = 'large'`, то элемент получит класс `main--large`. 
Если `classModifier = ''` (пустая строка) или `false`, то дополнительный класс не будет применен.

Этот синтаксис `[main--${classModifier}]` является сочетанием двух возможностей современного JavaScript:
1. Вычисляемые имена свойств (Computed Property Names)
2. Шаблонные литералы (Template Literals)

Давайте разберем подробнее:

1. Квадратные скобки `[]` используются для создания вычисляемого имени свойства. Это позволяет динамически определять имя свойства объекта.
2. Внутри квадратных скобок используются обратные кавычки `` ` `` (backticks), которые обозначают шаблонный литерал.
3. `${classModifier}` внутри шаблонного литерала — это интерполяция, которая позволяет вставить значение переменной `classModifier` в строку.

Таким образом, эта конструкция создает динамическое имя свойства, которое будет выглядеть как `"main--значение_classModifier"`.

```js
let classModifier = 'active';
let obj = {
  [`main--${classModifier}`]: true
};
console.log(obj); // { "main--active": true }

classModifier = 'large';
obj = {
  [`main--${classModifier}`]: true
};
console.log(obj); // { "main--large": true }
```