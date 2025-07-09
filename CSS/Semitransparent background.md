```css
.bar {
	position: relative;
}

.bar::after {
	content: '';
	position: absolute;
	inset: 0;
	z-index: -1;
	width: 100%;
	height: 100%;
	background: url("/img/hero-index/hero-bg.jpg") right center/cover no-repeat;
}

@media (max-width: 1199.98px) {
	.bar::after {
		opacity: 0.5;
		background-position: calc(50% - 160px) center;
	}
}
```