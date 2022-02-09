---
id: QSOq52vLtC0SDsnq3hlmJ
title: Functions
desc: ''
updated: 1638963742540
created: 1638963609388
---

Funkcje możemy przypisać do zmiennej jako **named function expression** lub funkcję anonimową. Jeśli chcemy z wnętrza funkcji odwoływać się do tej funkcji jak do this w OOP to musimy wykorzystać named function expression.

```javascript
let math = {
  'factit': function factorial(n) {
    console.log(n)
    if (n <= 1) {
      return 1;
    }
    return n * factorial(n - 1);
  }
};

math.factit(3) //3;2;1;
```
