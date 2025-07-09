### Access element in browser

In Chrome, Firefox, Edge and IE11+, when an element is selected, you can access this element in the console window by typing: `$0`

When an element is selected, type the following into the console and the browser will return an object

```js
$0.getBoundingClientRect()

>> { x: 1081, y: 72, width: 49, height: 28, top: 72, right: 1130, bottom: 99, left: 1081 }
```
___

### Design Mode Dev tools

```js
document.designMode="on";
```
___

### Multiple CSS styles 

```js
document.querySelector(".my-element").style.cssText = `  
  display: block;  
  position: absolute;  
`;
```
___


### Закрытие модального окна и переход по ссылке

```js
const menuModalButtons = document.querySelectorAll('#menuModal button');
menuModalButtons.forEach(modalButton => {
	modalButton.addEventListener('click', () => {
		let anchor = modalButton.dataset.href;
		location.href = `${anchor}`;
	});
})
```
___


### Форматирование даты

```js
Date.value.toLocaleString('ru-RU', {
	hour: '2-digit',
	minute: '2-digit'
});
```
___

### Pазница между передачей функции как обработчика событий и её немедленным вызовом

Функция выполнится немедленно:

```js
el.addEventListener('click', foo());
```

Функция выполнится после клика на элементе:

```js
el.addEventListener('click', foo);
el.addEventListener('click', () => foo());
```

Если нужно предотвратить стандартное поведение, то можно передать событие в функцию:

```js
el.addEventListener('click', (e) => foo(e));

const foo = (e) => {
	e.preventDefault();
	// ...
}
```

или 

```js
el.addEventListener('click', () => {
	e.preventDefault();
	foo();
)};
```
___
