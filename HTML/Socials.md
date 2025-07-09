```html
<div class="contacts__socials">
	<ul class="contacts__socials-list">
		<li class="contacts__socials-item">
			<a class="contacts__socials-link" href="[[++social-instagram]]">
				<svg class="svg-instagram-contacts">
					<use xlink:href="assets/images/svg-sprite.svg#instagram"></use>
				</svg>
			</a>
		</li>
		<li class="contacts__socials-item">
			<a class="contacts__socials-link" href="[[++social-telegram]]">
				<svg class="svg-telegram-contacts">
					<use xlink:href="assets/images/svg-sprite.svg#telegram-dark"></use>
				</svg>
			</a>
		</li>
		<li class="contacts__socials-item">
			<a class="contacts__socials-link" href="[[++social-whatsapp]]">
				<svg class="svg-whatsapp-contacts">
					<use xlink:href="assets/images/svg-sprite.svg#whatsapp"></use>
				</svg>
			</a>
		</li>
		<li class="contacts__socials-item">
			<a class="contacts__socials-link" href="[[++social-ok]]">
				<svg class="svg-ok-contacts">
					<use xlink:href="assets/images/svg-sprite.svg#odnoklassniki"></use>
				</svg>
			</a>
		</li>
	</ul>
</div>
```

```css
.contacts__socials-list {
    display: inline-flex;
    justify-content: center;
    align-items: center;
}

.contacts__socials-item {
    position: relative;
    margin-right: 16px;
    margin-left: 16px;
}

.contacts__socials-item::after {
    content: "";
    position: absolute;
    top: 0;
    right: -16px;
    width: 1px;
    height: 18px;
    background-color: rgba(166,166,166,0.4);
}

.contacts__socials-item:first-child {
    margin-left: 0;
}

.contacts__socials-item:last-child {
    margin-right: 0;
}

.contacts__socials-item:last-child::after {
    display: none;
}

.contacts__socials-link {
    display: block;
    line-height: 0;
}

.svg-instagram-contacts {
    width: 18px;
    height: 18px;
}

.svg-telegram-contacts {
    width: 16px;
    height: 15px;
}

.svg-whatsapp-contacts {
    fill: #596269;
    width: 18px;
    height: 18px;
}

.svg-ok-contacts {
    width: 11px;
    height: 18px;
}
```