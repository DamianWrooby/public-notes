---
id: 0dc91813-6eeb-4615-9fac-4a0b0d175e58
title: eventLoop
desc: ''
updated: 1620842749368
created: 1620763850378
---

Event loop czyli pętla zdarzeń to mechanizm zarządzający kolejnością zadań silnika JS.

```javascript
console.log('One');

setTimeout(function(){ console.log('Two'); }, 0);

console.log('Three');
```

Kolejność to: 
```
One
Three
Two
```

Dlatego, że setTimeout to asynchroniczne wydarzenie z Web API, które mimo, że ustawione na 0 sekund to nie jest bezpośrednio wrzucane na CALL STACK tylko trafia tam po wszystkich synchronicznych funkcjach.

Dzięki event loop JS może wykonywać dłuższe zadania w tle i ponownie dodawać je do wątku głównego kiedy są już gotowe.