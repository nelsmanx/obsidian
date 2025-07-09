#css #grid 

```css
.emulated-flex-gap {
  --gap: 12px;
  display: flex;
  flex-wrap: wrap;
  margin: calc(-1 * var(--gap)) 0 0 calc(-1 * var(--gap));
}

.emulated-flex-gap > * {
  margin: var(--gap) 0 0 var(--gap);
}
```

```css
.emulated-flex-gap {
  --row-gap: 20px;
  --column-gap: 25px;
  display: flex;
  flex-wrap: wrap;
  margin: calc(-1 * var(--row-gap)) 0 0 calc(-1 * var(--column-gap));
}

.emulated-flex-gap > * {
  margin: var(--row-gap) 0 0 var(--column-gap)
}
```
