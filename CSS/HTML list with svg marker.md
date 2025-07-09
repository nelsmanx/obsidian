#html #css #list

```css
li {
    position: relative;
    padding-left: 35px;
    font-size: 16px;
    font-weight: 500;
    color: #00a1be;
}

li:not(:last-child) {
    margin-bottom: 25px;
}

li::before {
    --scale: 1;
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: calc(var(--scale) * 20px);
    height: calc(var(--scale) * 20px);
    background: url("/assets/images/list-marker-1.svg") center/contain no-repeat;
}
```