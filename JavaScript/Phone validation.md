```html
<script src="assets/js/inputmask.js"></script>
```

```js
document.addEventListener('DOMContentLoaded', () => {
    Inputmask({"mask": "+7 (f99) 999-99-99", "clearMaskOnLostFocus": true, "showMaskOnHover": false, "placeholder": "_", definitions: { 'f': { validator: "[9]", cardinality: 1, }, } }).mask('input[type="tel"]');
})
```