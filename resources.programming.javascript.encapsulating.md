---
id: vLwcu2YpkvbFZ9v9EH5Hk
title: Encapsulating
desc: ''
updated: 1638780198394
created: 1638779659690
---

## Sposoby na enkapusulację w JS

[Kursjs.pl](https://kursjs.pl/kurs/obiekty/obiekty-enkapsulacja.php)

Dawniej stosowało się podkreślnik przy prywatnych polach klasy by poinformować, że dana metoda jest prywatna.

Można enkapsuację uzyskać także poprzez ES Modules.

Jeżeli w danym projekcie nie używasz modułów, możesz jawnie okryć swój kod funkcją - np. IIFE, lub zastosować tak zwany wzorzec fabryki. Polega on na stworzeniu funkcji, która zwraca nam jakiś obiekt. Zwracany obiekt widzi swoje środowisko (czyli wnętrze funkcji), natomiast reszta kodu nie będzie miała do tych rzeczy dostępu.

### Gettery i settery

Możemy skorzystać także z getterów i setterów. Dają one kontrolę nad tym co otrzymuje użytkownik. **Setter jest wywoływany podczas każdego przypisania właściwości** np.: 

```javascript
ob.name = 'Tom';
```

### Prywatne właściwości i metody w nowym Javascript

```javascript
class KebabMachine {
    #kebab = { //prywatne
        roll : false,
        stuff : []
    }

    #makeRoll() { //prywatna
        this.#kebab.roll = true;
    }

    #makeStuff(stuff) { //prywatna
        this.#kebab.stuff = stuff.map(el => el.toLowerCase());
    }

    createKebab(...components) { //publiczna
        this.#makeRoll();
        this.#makeStuff(components);

        return this.#kebab;
    }
}
```
