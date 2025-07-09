```css
&__list {
		counter-reset: section;
}

&__item::before {
	counter-increment: section;
	content: counter(section) ".";
	/* or */
	content: counter(section, decimal-leading-zero);
}
```