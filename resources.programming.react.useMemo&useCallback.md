---
id: 32dd01cd-6d38-4bdd-9beb-8c8f26fb6e6e
title: Usememo&Usecallback
desc: ''
updated: 1618679640983
created: 1618679571795
---

## useCallback

Jako pierwszy argument przyjmuje funkcję, a jako drugi taką samą tablicę zależności, jak useEffect.
```javascript
function MyComponent({ oneProp, anotherProp }) {
    const handleClick = React.useCallback(() => {
        console.log('Clicked!', oneProp, anotherProp)
    }, [oneProp, anotherProp]);

    return <SpecialButton onClick={handleClick} />;
}
```
W ten sposób, funkcja przekazana do React.useCallback jest memoizowana. To znaczy, że dopóki nie zmienią się oneProp lub anotherProp, dopóty handleClick nie będzie nową funkcją.

React.useCallback zapamiętuje stworzoną funkcję i dzięki temu komponent SpecialButton nie przerenderuje się bez potrzeby!


## useMemo

To React Hook, który zwraca zapamiętaną wartość. W pewnym sensie, to uogólniona wersja useCallback. Jako argument przyjmuje funkcję, która zwraca wartość.
```javascript
function MyComponent({ oneProp, anotherProp }) {
    const options = useMemo(() => ({
        data: oneProp,
        data2: anotherProp,
        // …
    }), [oneProp, anotherProp]);

    return <SpecialComponent options={options} />;
}
```
W ten sposób obiekt options będzie za każdym razem tą samą referencją i komponent SpecialComponent nie będzie musiał się przerenderowywać — przynajmniej dopóki nie zmieni się oneProp lub anotherProp!