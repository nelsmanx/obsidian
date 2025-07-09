#css 

```css

@media (max-width: 991.98px) {
	:root {
		--section-spacing: 60px 0;
		--fz-title-h1: 35px;
		--fz-title-h2: 30px;
	}
}

@media (max-width: 767.98px) {
	:root {
		--fz-title-h1: 32px;
	}
}

@media (max-width: 575.98px) {
	:root {
		--section-spacing: 40px 0;
		--fz-title-h1: 28px;
		--fz-title-h2: 24px;
	}
}

```



```css
:root {
	--fz-title-h1: 48px; /* lh 1.05em */
	--fz-title-h2: 36px; /* lh 1.25em */
	--fz-title-h3: 28px; /* lh 1.25em */
}

@media (max-width: 991.98px) {
	:root {
		--fz-title-h1: 40px; /* lh 1.125em */  /* k = 0.83 */
		--fz-title-h2: 32px; /* lh 1.25em */  /* k = 0.88 */
		--fz-title-h3: 24px; /* lh 1.25em */  /* k = 0.85 */
	}
}

@media (max-width: 575.98px) {
	:root {
		--fz-title-h1: 32px; /* lh 1.25em */ /* k = 0.80 */
		--fz-title-h2: 26px; /* lh 1.15em */ /* k = 0.81 */
		--fz-title-h3: 22px; /* lh 1.14em */ /* k = 0.92 */
	}
}

```