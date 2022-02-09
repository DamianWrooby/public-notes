---
id: rGROgD9Xsxzcn1rPcwA0S
title: This
desc: ''
updated: 1638981856616
created: 1631102866291
---

O tym co znajduje się w this decyduje kontekst wywołania funkcji - to w jaki sposób funkcja jest wywoływana, a nie to jak została zadeklarowana.

Funkcja jest w obiekcie - **this = obiekt z kontekstu (z lewej strony wywołania)**



Zwykła funkcja, która nie znajduje się w obiekcie (nie jest metodą obiektu lub klasy) - **this = obiekt window**


W **strict mode** this w global scope jest _undefined_

Callbacki są na łasce funkcji wyższego rzędu z której są wywoływane:

```javascript
function higherOrderFunction(callback) {
	callback();
}

function getThis() {
	console.log(this);
}

higherOrderFunction(getThis); // this z funkcji callback wskazuje na kontekst funkcji higherOrderFunction bo z jej wnętrza została wywołana
```