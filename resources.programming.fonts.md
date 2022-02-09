---
id: 534e172e-754f-4d4e-b6d1-b3024cbaca9e
title: Fonts
desc: ''
updated: 1618680148353
created: 1618679688821
---

## Ładowanie fontów Google

### Poprawnie
```html
<link rel="preconnect" href="https://fonts.gstatic.com" />
<link
  href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;400&display=swap"
  rel="stylesheet"
/>
```
### Lepiej
```html
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;400&display=swap"
  as="style"
  rel="preload"
/>
<link
  href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;400&display=swap"
  rel="stylesheet"
  media="print"
  onload="this.media='all'"
/>
```
## Fonty self-hosted

font-display: swap - font ładowany jest natychmiastowo, po upływie czasu, gdy nasz customowy font jest gotowy, tekst zostaje podmieniany
```html
<style>    
    @font-face {
    font-family: Lato;
    src: url('font-lato/lato-bolditalic-webfont.woff2') format('woff2');
    font-weight: 700;
    font-style: italic;
    font-display: swap;
  }
  
  body {
      font-family: 'Lato', "Helvetica", "Arial", sans-serif;
  }
</style>
```
## Next.js
_document.tsx
```html
<link rel="preconnect"
      href="https://fonts.gstatic.com"
      crossorigin />

<!-- [2] -->
<link rel="preload"
      as="style"
      href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" />

<!-- [3] -->
<link rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap"
      media="print" onload="this.media='all'" />

<!-- [4] -->
<noscript>
  <link rel="stylesheet"
        href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" />
</noscript>
```

## Strategie ładowania fontów

Zbiór strategii
https://github.com/zachleat/web-font-loading-recipes

Najlepsze pod względem wydajności:
https://www.zachleat.com/web/the-compromise/

[[resources.programming.fonts.web-font-loading-recipe]]