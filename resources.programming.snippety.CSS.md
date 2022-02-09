---
id: ckIaaNK21lCRGbE7ibBtN
title: CSS
desc: ''
updated: 1632053150534
created: 1631956858943
---

## 10 przydatnych mixinów
https://engageinteractive.co.uk/blog/top-10-scss-mixins

## Trójkąty SCSS

```scss
@mixin arrow($direction, $size: 5px, $color: #fff) {
	width: 0;
	height: 0;

	@if $direction == up {
		border-left: $size solid transparent;
		border-right: $size solid transparent;
		border-bottom: $size solid $color;
	} @else if $direction == down {
		border-left: $size solid transparent;
		border-right: $size solid transparent;
		border-top: $size solid $color;
	} @else if $direction == right {
		border-top: $size solid transparent;
		border-bottom: $size solid transparent;
		border-left: $size solid $color;
	} @else if $direction == left {
		border-top: $size solid transparent;
		border-bottom: $size solid transparent;
		border-right: $size solid $color;
	} @else {
		@error "Unknown direction #{$direction}.";
	}
}

.arrow-right {
	@include arrow(right);
	position: absolute;
    display: inline-block;
    top: 23px;
    right: 65px;
}
```
