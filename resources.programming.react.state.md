---
id: 5d05bbcc-3a85-4b60-8fc3-7ddfede438f8
title: State
desc: ''
updated: 1638435553011
created: 1622730165953
---

# Stan

## Ustawianie flag 

Wykorzystywanie flag w stanie doprowadza do boolean explosion - sytuacji, gdy duża staje się liczba możliwych kombinacji stanu.
Warto ograniczyć liczbę możliwych stanów do minimum wprowadzając je np. w formie enumów.

Zamiast
``` javascript
let isLoading = true;
let isComplete = false;
let hasErrored = false;

const makeFetch = async () => {
  isLoading = true;
  try {
    await fetch('/users');

    isComplete = true;
  } catch (e) {
    hasErrored = true;
  }
  isLoading = false;
};
```

Lepiej zrobić
```javascript
type Status = 'loading' | 'complete' | 'errored';

let status: Status = 'loading';

const makeFetch = async () => {
  status = 'loading';
  try {
    await fetch('/users');

    status = 'complete';
  } catch (e) {
    status = 'errored';
  }
};
```

### Can't perform a React state update on an unmounted component.

```javascript
useEffect(() => {
  let isMounted = true;               // note mutable flag
  someAsyncOperation().then(data => {
    if (isMounted) setState(data);    // add conditional check
  })
  return () => { isMounted = false }; // cleanup toggles value, if unmounted
}, []);
```