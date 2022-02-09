---
id: 33881c9e-9972-4119-a6c3-28cfb3984d6a
title: Performance
desc: ''
updated: 1638281869705
created: 1627923777881
---

## Jak uzyskać lepszą wydajność w React?

1. Unikać niepotrzebnego renderowania komponentów wykorzystując React.PureComponent i React.memo.
2. Unikać niepotrzebnego renderowania komponentów, w których tworzone są funkcje lub obiekty. Za każdym razem kiedy renderowany jest taki komponent tworzona jest nowa referencja funkcji czy obiektu. By tego uniknąć możemy skorzystać odpowiednio z hooków useCallback i useMemo.
3. Unikać dużych rozmiarów bundli tworząc build produkcyjny.
4. Leniwie wczytywać komponenty tylko wtedy, gdy są potrzebne korzystając z lazy i Suspense.