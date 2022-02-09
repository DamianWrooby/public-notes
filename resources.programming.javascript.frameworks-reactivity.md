---
id: zkXXjemTI2ZjOmQ6mtF1r
title: Frameworks Reactivity
desc: ''
updated: 1639558484916
created: 1639558427352
---

Każda zmienna posiada klasę Dep, która jest prostą implementacją wzorca obserwatora z tablicą subskrybentów i dwiema metodami - depend i notify. Depend dodaje zmienne do obserwowania, a notify uruchamia na nowo obliczanie tych zmiennych. 

![](/assets/images/2021-12-15-09-54-09.png)

Dodatkowo na zmienne zapięte są watchery, które uruchamiają metody w odpowiedniej instancji klasy Dep. Vue sprawdza jakie zmienne występują wewnątrz watchera i uruchamia dep.depend() w instancjach tych zmiennych, żeby dodać ich obserwowanie.

dep.notify() jest uruchamiany automatycznie przy aktualizacji którejś z obserwowanych zmiennych.
We vue realizowane jest to poprzez ustawianie setterów i getterów za pomocą Object.defineProperty(). W getterze dodajemy obserwację przez dep.depend(), a jeśli przypiszemy na nowo jakąś zmienną uruchamiany jest setter i znajdujący się w nim dep.notify()

![](/assets/images/2021-12-15-09-54-39.png)

Wywołany watcher przypisuje do target funkcję i wywołuje ją. W trakcie obliczania np. total uruchamiane są gettery, które dodają całą tę funkcję z target do subscribers i co za tym idzie uruchamiany jest notify() kiedy zostanie przypisana im nowa wartość.
