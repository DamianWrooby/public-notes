---
id: 351c0d95-ed03-4106-b20e-86ec72cd715d
title: Web Font Loading Recipe
desc: ''
updated: 1618861904582
created: 1618679791202
---
```html
<!doctype html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>“The Compromise”: Critical FOFT with preload, with a polyfill fallback emulating font-display: optional</title>
	<link rel="preload" href="font-lato/lato-zachleat-optimized.woff2" as="font" type="font/woff2" crossorigin>
	<style>
	@font-face {
		font-family: LatoSubset;
		src: url('font-lato/lato-zachleat-optimized.woff2') format('woff2'),
				 url('font-lato/lato-zachleat-optimized.woff') format('woff');
		unicode-range: U+65-90, U+97-122;
	}
	@font-face {
		font-family: Lato;
		src: url('font-lato/lato-regular-webfont.woff2') format('woff2'),
				 url('font-lato/lato-regular-webfont.woff') format('woff');
		font-display: swap;
	}
	@font-face {
		font-family: Lato;
		src: url('font-lato/lato-bold-webfont.woff2') format('woff2'),
				 url('font-lato/lato-bold-webfont.woff') format('woff');
		font-weight: 700;
		font-display: swap;
	}
	@font-face {
		font-family: Lato;
		src: url('font-lato/lato-italic-webfont.woff2') format('woff2'),
				 url('font-lato/lato-italic-webfont.woff') format('woff');
		font-style: italic;
		font-display: swap;
	}
	@font-face {
		font-family: Lato;
		src: url('font-lato/lato-bolditalic-webfont.woff2') format('woff2'),
				 url('font-lato/lato-bolditalic-webfont.woff') format('woff');
		font-weight: 700;
		font-style: italic;
		font-display: swap;
	}

	body {
		font-family: sans-serif;
	}
	.fonts-loaded-1 body {
		font-family: LatoSubset;
	}
	.fonts-loaded-2 body {
		font-family: Lato;
	}
	</style>
	<script>
	(function() {
		"use strict";

		// Optimization for Repeat Views
		if( sessionStorage.fontsLoadedCriticalFoftPreloadFallback ) {
			document.documentElement.className += " fonts-loaded-2";
			return;
		} else if( "fonts" in document ) {
			document.fonts.load("1em LatoSubset").then(function () {
				document.documentElement.className += " fonts-loaded-1";

				Promise.all([
					document.fonts.load("400 1em Lato"),
					document.fonts.load("700 1em Lato"),
					document.fonts.load("italic 1em Lato"),
					document.fonts.load("italic 700 1em Lato")
				]).then(function () {
					document.documentElement.className += " fonts-loaded-2";

					// Optimization for Repeat Views
					sessionStorage.fontsLoadedCriticalFoftPreloadFallback = true;
				});
			});
		} else {
			// use fallback
			var ref = document.getElementsByTagName( "script" )[ 0 ];
			var script = document.createElement( "script" );
			script.src = "critical-foft-preload-fallback-optional.js";
			script.async = true;
			ref.parentNode.insertBefore( script, ref );

			/*
			 * technically you could trigger the web font load here too and race it with
			 * the polyfill load, this means creating an element with text content that
			 * uses the font and attaching it to the document
			 * <div style="font-family: Lato; font-weight: 400; font-style: italic">A</div>
			 */
		}
	})();
	</script>
</head>
<body>
	<h1>“The Compromise”: Critical FOFT with preload, with a polyfill fallback emulating font-display: optional</h1>
	<p>This is a paragraph. <strong>This is heavier text.</strong> <em>This is emphasized text.</em> <strong><em>This is heavier and emphasized text.</em></strong></p>

	<hr style="margin-top: 2em">
	<ul>
		<li>This demo is a companion to <a href="https://www.zachleat.com/web/comprehensive-webfonts/">A Comprehensive Guide to Font Loading Strategies</a></li>
		<li>See all demos on <a href="https://github.com/zachleat/web-font-loading-recipes">GitHub <code>zachleat/web-font-loading-recipes</code></a></li>
	</ul>
</body>
</html>
```