---
id: 58e444c6-a204-43a5-a8ec-fdd2a9babca4
title: JavaScript
desc: ''
updated: 1640605379104
created: 1618680360667
---
## Proxy

Proxy to obiekt w JS, który otacza inny obiekt w celu monitorowania go oraz potencjalnego customowego reagowania na zmiany atrybutów. Proxy jest wykorzystywany np. we Vue do śledzenia zmiennych, metod itp. w reaktywności.

Proxy nie działa tylko na IE.
## Mental model

W javascript mamy różne rodzaje wartości:

Prymitywne:
- number
- boolean
- string
- null
- undefined
- symbols
- BigInt

Wartości prymitywne są **niemutowalne**.

Obiekty i funkcje:
- obiekty
- funkcje (technicznie to również obiekty)

Obiekty i funkcje są **mutowalne**.

Wszystkie pozostałe typy jak tablice, daty, wyrażenia regularne to po prostu obiekty.

W javascript występują także wyrażenia (expressions) jak: 2 + 2

Poniższy kod przekazuje 7 różnych funkcji do console.log

```javascript
for (let i = 0; i < 7; i++) {
  console.log(function() {});
}
```