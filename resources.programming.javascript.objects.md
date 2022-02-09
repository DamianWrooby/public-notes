---
id: 035cff22-3a7a-4f4d-8753-33ae5c10062b
title: Objects
desc: ''
updated: 1640796939041
created: 1627293962133
---

## Porównywanie obiektów

Nie możemy bezpośrednio porównywać obiektów, bo porównywane są referencje:
```javascript
const hero1 = {
  name: 'Batman'
};
const hero2 = {
  name: 'Batman'
};

hero1 === hero1; // => true
hero1 === hero2; // => false

hero1 == hero1; // => true
hero1 == hero2; // => false

Object.is(hero1, hero1); // => true
Object.is(hero1, hero2); // => false
```
### Manual comparison
Możemy obiekty porównać porównując ręcznie ich właściwości:

```javascript
function isHeroEqual(object1, object2) {
  return object1.name === object2.name;
}

const hero1 = {
  name: 'Batman'
};
const hero2 = {
  name: 'Batman'
};
const hero3 = {
  name: 'Joker'
};

isHeroEqual(hero1, hero2); // => true
isHeroEqual(hero1, hero3); // => false
```

### Shallow equality

Polega to na pobraniu kluczy obiektu i porównaniu wartości.

Prosta implementacja:
```javascript
function shallowEqual(object1, object2) {
  const keys1 = Object.keys(object1);
  const keys2 = Object.keys(object2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  for (let key of keys1) {
    if (object1[key] !== object2[key]) {
      return false;
    }
  }

  return true;
}
```
Ta metoda sprawdzi się tylko w przypadku niezagnieżdżonych obiektów.

## Deep equality
To podobna metoda do *Shallow equality* z jedną różnicą. Jeżeli w trakcie płytkiego porównania napotamy obiekt, porównanie dokonywane jest również dla tego zagnieżdżonego obiektu.

Przykładowa implementacja:
```javascript
function deepEqual(object1, object2) {
  const keys1 = Object.keys(object1);
  const keys2 = Object.keys(object2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  for (const key of keys1) {
    const val1 = object1[key];
    const val2 = object2[key];
    const areObjects = isObject(val1) && isObject(val2);
    if (
      areObjects && !deepEqual(val1, val2) ||
      !areObjects && val1 !== val2
    ) {
      return false;
    }
  }

  return true;
}

function isObject(object) {
  return object != null && typeof object === 'object';
}
```

## Kopiowanie obiektów

Żeby skopiować obiekt bez referencji możemy użyć spread operatora lub metody Object.assign(): 

```javascript
const obj2 = {...obj1};

const obj2 = Object.assign(obj1);
```

Jednak w przypadku obiektów i większym niż 1 stopniu zagnieżdżenia to nie zadziała, gdyż dla obiektów zagnieżdżonych nadal będą kopiowane referencje. Żeby tego uniknąć możemy zastosować hack w postaci:

```javascript
JSON.parse(JSON.stringify(obj));
```

Ten sposób nie sprawdzi się w przypadku, gdy nasz obiekt będzie przechowywał wartości, których nie można przechowywać w JSON czyli np. NaN lub undefined.

## Tworzenie kopii obiektu z pominięciem wybranych właściwości

```javascript
const { unwantedProperty, ...copiedObj } = Obj;
```

Pod zmienną **copiedObj** jest teraz kopia obiektu Obj bez propertki **unwantedProperty**.