---
id: 6bac7bc5-eefe-42c1-a54d-bae3dde3b961
title: useRef
desc: ''
updated: 1620317209887
created: 1620316597206
---

Referencji używamy kiedy chcemy przechować jakąś wartość związaną ze strukturą jak np. ID timera.

Zmiana wartości useRef nie niesie za sobą rerenderu komponentu. Wartość useRef pomiędzy rerenderami jest stała.

useRef najczęściej stosowany jest, by mieć dostęp do elementów DOM.

```javascript
import { useRef, useEffect } from 'react';

function AccessingElement() {
  // 1. Definiujemy referencję  
  const elementRef = useRef();

   useEffect(() => {
    // 3. Po zamontowaniu elementRef.current zawiera element DOM
    const divElement = elementRef.current;
  }, []);

  return (
    // 2. Przypisujemy referencję do elementu
    <div ref={elementRef}>
      I'm an element
    </div>
  );
}
```

### Podczas pierwszego renderu Ref ma wartość null

## Aktualizacja referencji

Ref powinien być aktualizowany tylko w useEffect lub w funkcjach handlerów

```javascript

import { useRef, useEffect } from 'react';

function MyComponent({ prop }) {
  const myRef = useRef(0);

  useEffect(() => {
    myRef.current++; // Good!

    setTimeout(() => {
      myRef.current++; // Good!
    }, 1000);
  }, []);

  const handler = () => {
    myRef.current++; // Good!
  };

  myRef.current++; // Bad!

  if (prop) {
    myRef.current++; // Bad!
  }

  return <button onClick={handler}>My button</button>;
}
```