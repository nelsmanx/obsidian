``` js
// ##### FIXED-HEADER #####
document.addEventListener("DOMContentLoaded", () => {
	const headerVisible = document.querySelector('.menuline');
	if (!headerVisible) return;

	const headerHidden = document.querySelector('.header-info');
	let headerHiddenHeight;
	let headerVisibleHeight;

	computeOffsets();
	scrollingHandler();

	window.addEventListener('scroll', scrollingHandler);
	window.addEventListener('resize', computeOffsets);

	function computeOffsets() {
		headerVisibleHeight = headerVisible.getBoundingClientRect().height;
		headerHiddenHeight = headerHidden.getBoundingClientRect().height;
	}

	function scrollingHandler() {
		if (window.pageYOffset > headerHidden.getBoundingClientRect().height) {
			computeOffsets();
			document.body.style.paddingTop = `${headerVisibleHeight}px`;
			headerVisible.classList.add('is-fixed');
			headerVisible.style.boxShadow = '0px 0px 10px 5px rgb(100 100 100 / 15%)';
		} else {
			document.body.style.removeProperty('padding-top');
			headerVisible.classList.remove('is-fixed');
			headerVisible.style.removeProperty('box-shadow');
		}
	}
});

```

```css 
.header-visible.is-fixed {
	position: fixed;
	z-index: 10;
	top: 0;
	left: 0;
	right: 0;
	background-color: #fff;
}
```