#html #list

```css

li {
    position: relative;
    padding-left: 23px;
}

li:not(:last-child) {
    margin-bottom: 15px;
}

li::before {
    content: '';
    position: absolute;
    top: 12px;
    left: 10px;
    display: inline-block;
    width: 2px;
    height: 2px;
    border-radius: 50%;
    background-color: #2d2b5c;
}

```

```css
ol {
	list-style-type: decimal;
	list-style-position: inside
}
```