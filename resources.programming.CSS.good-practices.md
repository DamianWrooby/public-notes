---
id: 3VwBYpoYtOO2RopVttrpD
title: good-practices
desc: ''
updated: 1640038292845
created: 1640037898869
---

1. Nie stylować po id i tagach - kod staje się zbyt specyficzny i utrudnia to utrzymanie. Po tagach można stylować np. html i body gdy chcemy dokonać resetu (normalizacji).

2. Nie używać styli inline i !important.

3. Zarządzanie z-index:
```css
:root {
  --z-index-box: 1000;
  --z-index-popup: 1100;
  --z-index-modal: 1200;
}

.box {
  z-index: var(--z-index-box);
}

.popup {
  z-index: var(--z-index-popup);
}

.modal {
  z-index: var(--z-index-modal);
}
```
4. Spójny format kolorów.

5. Zmienne lokalne żeby było wiadomo co skąd pochodzi:
```css
.box {
  --offset: 50px;
  --header-height: 144px;
  --header-margin-bottom: 6px;
  position: absolute;
  top: calc(var(--offset) + var(--header-height) + var(--header-margin-bottom));
}
```
6. Spójna konwencja nazewnicza.

7. Nie usuwać outline! Z powodów dostępności.

