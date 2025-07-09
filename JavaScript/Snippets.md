
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
### Download file

```js
function downloadUrl(url) {
	const link = document.createElement('a');
	link.setAttribute('download', name = '');
	link.href = url;
	document.body.appendChild(link);
	link.click();
	link.remove();
}
```