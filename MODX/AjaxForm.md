
```php
[[!AjaxForm?
	&snippet=`FormIt`
	&form=`expresscalcModalForm`
	&emailTpl=`expresscalcModalFormEmail`
	&hooks=`crm_mail,FormItSaveForm,spam,email`
	&emailFrom=`[[++emailsender]]`
	&emailSubject=`[[++site_name]] - Новая заявка`
	&emailTo=`[[++email_manager]]`
	&validate=`phone:required`
	&validationErrorMessage=`Введите свой номер телефона`
	&successMessage=`Заявка успешно отправлена<script></script>`
]]
```


```js
const modal = document.querySelector(selector);
modal.querySelector('input[name="room"]').value = this.roomName;   modal.querySelector('input[name="glass"]').value = this.glassName; modal.querySelector('input[name="color"]').value = this.colorName;
```


```php
/* ### ## expresscalcModalForm ### */ 
<form class="modal__form" action="[[~[[*id]]]]" method="POST">
	<input class="modal__input" type="tel" name="phone" placeholder="Введите ваш телефон" required="">
	<input class="modal__input" type="text" name="name" placeholder="Ваше имя" required="">
	<input type="hidden" name='page' value="[[++site_url]][[*id:is=`1`:then=``:else=`[[~[[*id]]]]`]]">
	<input type="hidden" name='room' value="">
	<input type="hidden" name='glass' value="">
	<input type="hidden" name='color' value="">
	<button class="modal__btn-submit" type="submit">ПОЛУЧИТЬ БЫСТРЫЙ РАСЧЕТ</button>
</form>
```


```php
/* ### expresscalcModalFormEmail ### */ 

<h2>Заявка на быстрый расчет со страницы [[+page]]</h2>

<p>Имя: <b>[[+name]]</b></p>
<p>Телефон: <b>[[+phone]]</b></p>
<p>Назначение: <b>[[+room]]</b></p>
<p>Тип стекла: <b>[[+glass]]</b></p>
<p>Материал рамы (цвет): <b>[[+color]]</b></p>
```


### Phone validation

```php
[[!AjaxForm?
   &snippet=`FormIt`
   &form=`standartForm`
   &emailTpl=`standartEmail`
   &hooks=`rcv3,spam,email,FormItSaveForm`
   &emailFrom=`[[++emailsender]]`
   &emailSubject=`Новая заявка на обратную связь с сайта kobico.ru`
   &emailTo=`[[++email_address]]`
   &validate=`phone:phoneValidate`
   &customValidators=`phoneValidate`
   &validationErrorMessage=`<div class="error-message">Неверный формат номера телефона</div>`
   &successMessage=`Заявка успешно отправлена!<script>window.location.href="/thank-you-page"</script>`
        ]]
```

```php
// snippet 'phoneValidate'

$pattern = "/^\+\d \(\d{3}\) \d{3}-\d{2}-\d{2}$/";

if (!preg_match($pattern, $value)) {
    $validator->addError($key, 'Неверный формат номера телефона');
    $success = false;
} else {
    $success = true;
}

return $success;
```

```html
<script src="/assets/js/inputmask.js"></script>
```
[Inputmask](https://robinherbots.github.io/Inputmask/)

```js
// script.js

document.addEventListener('DOMContentLoaded', () => {
	Inputmask({
		"mask": "+7 (f99) 999-99-99",
		"clearMaskOnLostFocus": true,
		"showMaskOnHover": false,
		"placeholder": "_",
		definitions: {
			'f': {
				validator: "[9]",
				cardinality: 1,
			},
		}
	}).mask('input[type="tel"]');
});
```

