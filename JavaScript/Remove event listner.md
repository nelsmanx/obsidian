```js
function setYandexTargetName(event, ymName) {
	const targetModalSelector = event.target.dataset.bsTarget;
	const targetModal = document.querySelector(targetModalSelector);
	if (!targetModal) return;

	const modalForm = targetModal.querySelector('form');
	modalForm.setAttribute('data-ym-name', ymName);

	targetModal.addEventListener('hidden.bs.modal', handleModalHiddenWithYmName);
}

function handleModalHiddenWithYmName(event) {
	removeYandexTargetName(event.currentTarget, event.currentTarget.querySelector('form'));
}

function removeYandexTargetName(modal, form) {
	delete form.dataset.ymName;
	modal.removeEventListener('hidden.bs.modal', handleModalHiddenWithYmName);
}
```
___

При этом такой код работать не будет, возможно , возможно проблема заключается в том, что удаляется слушатель события, используя ссылку на ту же самую функцию `removeYandexTargetName`

```js
function setYandexTargetName(event, ymName) {
	...
	targetModal.addEventListener('hidden.bs.modal', removeYandexTargetName);
}

function removeYandexTargetName(event) {
	...
	modalForm.removeEventListener('hidden.bs.modal', removeYandexTargetName);
}
```