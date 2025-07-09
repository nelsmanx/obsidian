#scss

```scss
@use 'mixins' as *; //add at first line in main.scss
```

## Convert Photoshop tracking to letter spacing

```scss
@mixin l-spacing ( $tracking-psd, $font-size) {
	letter-spacing: #{( $tracking-psd / 1000 * $font-size )}px;
}
// @include l-spacing ( 5, 15 );

@mixin l-spacing-em ( $tracking-psd ) {
	letter-spacing: #{( $tracking-psd / 1000 * 1em )}em;
}
// @include l-spacing-em ( 5 );
```

## Convert Photoshop leading to line-height

```scss
@mixin l-height ( $leading-psd, $font-size) {
	line-height: #{ $leading-psd / $font-size };
}

// @include l-height ( 30, 15 );
```

## Convert Photoshop shadow to CSS

```scss
@use 'sass:math';

@mixin b-shadow ( $angle, $distance, $spread, $size, $color ) {

	$offsetX: 999 !default;
	$offsetY: 999 !default;
	$blur: 999 !default;
	$spread: 999 !default;
	
	$angle:  (( 180 - $angle ) * 3.14 / 180) ;
	$offsetX: #{math.round(math.cos( $angle ) * $distance)}px;
	$offsetY: #{math.round( math.sin( $angle ) * $distance )}px;
	$blur: #{ $size - $spread }px;
	$spread: #{ $size * $spread / 100 }px;
	
	box-shadow: #{$offsetX $offsetY $blur $spread $color};
}

// @include b-shadow ( 120, 5, 0, 20 );
```

## Fluid font size

```scss
@use 'sass:math';

@mixin fluid-fz ($max-font-size, $min-font-size, $max-width: 840, $min-width: 360) {
	$rem: 16;
	$rem-max-font-size: $max-font-size / $rem;
	$rem-min-font-size: $min-font-size / $rem;
	$rem-max-width: $max-width / $rem;
	$rem-min-width: $min-width / $rem;
	$slope: ($rem-max-font-size - $rem-min-font-size) / ($rem-max-width - $rem-min-width);
	$slope-rounded: (math.round($slope * 1000)) / 1000;
	$yAxisIntersection: -$rem-min-width * $slope + $rem-min-font-size;
	// rem-min-width: $rem-min-width;
	// rem-max-width: $rem-max-width;
	// slope-rounded: $slope-rounded;
	// yAxisIntersection: $yAxisIntersection;
	font-size: clamp(#{$rem-min-font-size}rem, #{$yAxisIntersection}rem + #{$slope-rounded * 100}vw, #{$rem-max-font-size}rem);
}

// @include fluid-fz(56, 26);

// https://css-tricks.com/linearly-scale-font-size-with-css-clamp-based-on-the-viewport/
```