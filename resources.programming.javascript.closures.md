---
id: bf6ab2f3-6a0c-49c1-9256-f8affc63cf98
title: Closures
desc: ''
updated: 1620230003055
created: 1620228063517
---
# Domknięcia (closures)

Domknięcie występuje, gdy funkcja korzysta ze zmiennej zadeklarowanej poza tą funkcją.

```javascript
function liveADay() {
  let food = 'cheese'; // Declare `food`
  function eat() {
    console.log(food + ' is good'); // Read `food`
  }
  eat();
}
liveADay();
```

## Duchy wywołań

```javascript
function liveADay() {
  let food = 'cheese';
  function eat() {
    console.log(food + ' is good');
  }
  // Call eat after five seconds
  setTimeout(eat, 5000);
}
liveADay();
```

Nawet jeśli funkcja liveADay() zakończy swoje działanie, zmienna *food* będzie dostępna bo korzysta z niej funkcja eat(). O mechanizm ten dba za nas JS. 