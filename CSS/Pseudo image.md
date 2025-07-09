#css 

```css
.foo::after {
    --scale: 1;
    content: '';
    position: absolute;
    top: -25px;
    right: calc(50% - 565px);
    width: calc(var(--scale) * 300px);
    height: calc(var(--scale) * 200px);
    background: url("/assets/images/branch-image-1.png") 0 0/contain no-repeat;
}
```