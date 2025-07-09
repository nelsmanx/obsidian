# Make all slides the same height based on the tallest slide

Swiper use flexbox layout and stretch set by default, just remove height property from swiper-slide

```css
.swiper-slide {
  height: auto;
}
```