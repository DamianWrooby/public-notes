---
id: 66489275-cca9-4aa1-aeb5-f3d37a455e61
title: Events
desc: ''
updated: 1619468379810
created: 1619382601025
---

``` javascript
<button onClick={clickHandler} />
```

Wskazujemy tutaj tylko funkcję zamiast ją wywoływać.
Kiedy ją wywołamy zostanie ona egzekwowana w momencie parsowania kodu JSX, a nie po kliknięciu.

Istnieje niepisana konwencja, by funkcje eventów nazywać z końcówką 'Handler'.

Do zdarzenia inputu możemy przekazać obiekt event, by na bieżąco odczytywać wartość:

``` javascript
const onInputChangeHandler = (event) => {
 setInputValue(event.target.value);
}

<input onChange={onInputChangeHandler}>
```