---
id: I2DEsLAnR5h2nth3tSMrB
title: Events
desc: ''
updated: 1635943902582
created: 1635942764036
---

## Kolejność odpalania eventów

Wyróżniamy trzy fazy propagacji zdarzeń:
- event capturing
- event taret
- event bubbling

![](/assets/images/2021-11-03-13-36-36.png)

Domyślnie zdarzenia odpalają się w kolejności **target -> bubbling** czyli najpierw kliknięty zagnieżdżony element, a następnie w górę drzewa.

```html
<div class="parent">
    <button class="button">Kliknij mnie i sprawdź w konsoli</button>
</div>
```

```javascript
const div = document.querySelector(".parent");
const btn = document.querySelector(".button");

div.addEventListener("click", e => {
    console.log("Kliknięto div");
});

btn.addEventListener("click", e => {
    console.log("Kliknięto przycisk");
});
```

W tym przypadku jeśli klikniemy na button to najpierw zostanie uruchomione zdarzenie zapięte na button, a następnie na rodzicu.

## Zatrzymanie propagacji

Żeby zatrzymać propagację zdarzenia użyjemy:

```javascript
event.stopPropagation(); // Nie uruchomią się zdarzenia zapięte na rodzicach
```

```javascript
event.stopImmadiatePropagation(); // Nie uruchomią się tez inne zdarzenia danego typu zapięte na elemencie
```

## Element nasłuchujący i ten, na którym odpalono zdarzenie

Element który nasłuchuje zdarzenie wcale nie musi być tym, na którym dane zdarzenie zostało wywołane. Możemy dla przykładu nasłuchiwać zdarzenia click dla dokumentu, a zostanie ono odpalone na buttonie, który jest w tym dokumencie.

Właściwość **e.target** wskazuje na element, na którym dane zdarzenie się wydarzyło (czyli nastąpiła faza target), a właściwość **e.currentTarget** na element, do którego podpięliśmy funkcję nasłuchującą dane zdarzenie.

## Duża liczba zdarzeń

Jeśli mamy listę wielu elementów, na które chcielibyśmy podpiąć zdarzenia to zamiast podpinać zdarzenie pod każdy element możemy podpiąć jedno zdarzenie na elemencie rodzica (np. liście) i wewnątrz zdarzenia sprawdzić, który element został kliknięty:

```javascript
list.addEventListener("click", e => {
    //sprawdzam czy kliknięty element jest przyciskiem i ma klasę .delete
    if (e.target.classList.contains("delete")) {
        e.target.closest(".element").remove();
    }
});
```

Niestety - jak to w życiu bywa - nie zawsze będzie to takie proste. W powyższym kodzie sprawdzam, czy e.target wskazuje na przycisk i ma klasę .delete. Problem pojawi się, gdy w takim przycisku będzie jakaś dodatkowa ikonka - FontAwesome czy w formacie svg, a użytkownik klikając w przycisk kliknie właśnie w tą ikonkę. W takim przypadku e.target nie będzie wskazywał na button, a ikonkę.

Aby się z tym uporać możemy zastosować minimum dwa podejścia (takie mi przychodzą do głowy).

Jednym z nich będzie funkcja **closest()**:

```javascript
const cnt = document.querySelector(".btn-parent");
cnt.addEventListener("click", e => {
    console.log("e.target: ", e.target);

    if (e.target.closest(".button")) {
        console.log("Kliknąłeś w button");
    }
});
```

Innym rozwiązaniem może być zastosowanie dla ikony właściwości CSS **pointer-events: none**, która wyłączy możliwość klikania na niej.