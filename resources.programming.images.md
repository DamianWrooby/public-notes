---
id: 72094637-d5f5-41e3-9e33-a5be002dc41a
title: Images
desc: ""
updated: 1635254857207
created: 1618680463539
---

## Margin-bottom hack

Padding-bottom jest obliczany na podstawie width rodzica, więc jeśli odpowiednio określimy padding-bottom w procentach dziecku to zostanie zachowany aspect-ratio tego kontenera.

```css
.container {
	width: 100%;
}
.container img {
	padding-bottom:  56.25%; (9/16); // Proporcje 16:9
}
```

### Przykład

```html
<div class="container">
	<div class="image-wrapper">
		<img class="image" src="..." />
	</div>
</div>
```

```css
.container {
	width: 100%;
}
.image-wrapper {
	position: relative;
	padding-bottom: 100%; // Kwadrat
}
.image-wrapper img {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
```

## Załaduj nowoczesne formaty jeśli są wspierane. Jeśli nie, załaduj png

```html
<picture>
	<source srcset="logo.avif" type="image/avif" />
	<source srcset="logo.webp" type="image/webp" />
	<img src="logo.png" alt="" width="640" height="360" />
</picture>
```

## Ładowanie różnych rozmiarów dla różnych szerokości ekranu

```html
<picture>
	<source srcset="logo-768.png" media="(min-width: 768px)" />
	<source srcset="logo-480.png" media="(min-width: 480px)" />
	<img src="logo.png" alt="" width="640" height="360" />
</picture>
```

## Połączenie dwóch powyższych opcji

```html
<picture>
	<source
		sizes="(max-width: 768px) 100vw, 768px"
		srcset="logo-768.avif 768w, logo-480.avif 480w"
		type="image/avif"
	/>
	<source
		sizes="(max-width: 768px) 100vw, 768px"
		srcset="logo-768.webp 768w, logo-480.webp 480w"
		type="image/webp"
	/>
	<source
		sizes="(max-width: 768px) 100vw, 768px"
		srcset="logo-768.png 768w, logo-480.png 480w"
		type="image/png"
	/>
	<img src="logo.png" alt="" width="640" height="360" />
</picture>
```

## Poniższy kod wyświetli webp dla wspieranych przeglądarek, jpeg2000 dla safari oraz JPEG-XR dla IE 11

```html
<picture>
	<source srcset="/images/cereal-box.webp" type="image/webp" />
	<source srcset="/images/cereal-box.jp2" type="image/jp2" />
	<img src="/images/cereal-box.jxr" type="image/vnd.ms-photo" />
</picture>
```

### To samo dla tła

```html
<style>
	div {
		background-image: url("logo.png"); /* Fallback */
		background-image: image-set("logo-small.webp" 1x, "logo-large.webp" 2x);
	}
</style>
```

## Poprawienie metryki CLS (Cumulative Layout Shift)

Elementy `<img>` powinny mieć określone width i height:

```html
<img src="image.jpg" width="640" height="360" alt="" class="image" />
```

Aspect ratio wynosi 16:9

Możemy łatwo zmienić jego rozmiar w CSS:

```html
<style>
	.image {
		height: auto;
		width: 100%;
	}
</style>
```

### Lazy Loading

```html
<img loading="lazy" src="image.jpg" width="640" height="360" alt="" />
```

Preload - dla zasobów above the fold | Można też zastosować dla background-image w CSS:

```html
<html>
	<head>
		<link
			rel="preload"
			as="image"
			href="image.jpg"
			imagesrcset="image-480.jpg 480w, image-768.jpg 768"
			imagesizes="50vw"
		/>
	</head>
	<body>
		<img src="image.jpg" width="640" height="360" alt="" class="image" />
	</body>
</html>
```

---

### Pomocne narzędzia:

SVG - narzędzie SVGO
TinyPNG - optymalizacja PNG
TinyJPG - optymalizacja JPG
Cloudinary - serwis typu CDN, który może nie tylko optymalizować obrazki, ale również dostarczać ich różne wersje (responsive images + nowoczesne formaty)
Imgix - alternatywa dla Cloudinary
Responsive Breakpoints - generator responsywnych obrazków
Image-webpack-loader - plugin do webpacka, który optymalizuje obrazki podczas builda, korzysta z bilbioteki Imagemin
Sharp - popularne narzędzie do optymalizacji obrazków, korzystają z niego nowoczesne frameworki takie jak Gatsby czy Next.js
