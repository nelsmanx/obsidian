```js
const menuModalButtons = document.querySelectorAll('#menuModal button');
menuModalButtons.forEach(modalButton => {
	modalButton.addEventListener('click', () => {
		let anchor = modalButton.dataset.href;
		location.href = `${anchor}`;
	});
})



const anchorsTags = document.querySelectorAll('[data-anchor-target]');
anchorsTags.forEach(anchor => {
	anchor.addEventListener('click', () => {
		let targetID = anchor.dataset.anchorTarget;
		let targetSection = document.querySelector(targetID);
		let targetSectionOffset = targetSection.getBoundingClientRect().top;
		console.log(targetSectionOffset);
		window.scroll({
			top: targetSectionOffset,
			left: 0,
			behavior: 'smooth'
		});
	});
})
```
___

```css
#goods:target {
    scroll-margin-top: var(--scroll-margin-top); 
}
```

```js
document.documentElement.style.setProperty('--scroll-margin-top', `${headerVisibleHeight}px`);
```

Doesn't work with header { position: sticky }

### JS method

```js
const activeCategory = document.querySelector('#active-category');
	if (activeCategory) {
		const activeCategoryPosition = activeCategory.getBoundingClientRect();
		const pageY = activeCategoryPosition.top + window.scrollY;
		window.scrollTo(0, pageY - appStore.headerHeight);
	}
```