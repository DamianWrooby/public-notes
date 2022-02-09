---
id: 4e384a5f-0c3d-4c0d-add5-016bfebd0f7b
title: useReducer
desc: ''
updated: 1620151529074
created: 1620149236029
---

## Budowa

``` javascript
// Inicjalizujemy stan
const initialState = {catches: []};

// Reducer to funkcja aktualizująca stan, którą wywołujemy dispatchem
function reducer(state, action) {
  switch (action.type) {
    case 'SET_CATCHES':
      return {...state, catches: action.catches}; // Reducer zwraca nowy stan
    default:
      throw new Error(); 
  }
}

const [state, dispatch] = useReducer(reducer, initialState);

// Funkcja wywołująca dispatch. To co znajduje się wewnątrz to action
const addCatches = (newCatches) => {
    dispatch({type: 'SET_CATCHES', catches: newCatches}); 
}
```

## Kiedy używać useState, a kiedy useReducer?

- Kiedy jest to jedynie niezależny element stanu, którym zardządzasz: **useState**


- Kiedy jeden element twojego stanu zależy od wartości innego elementu twojego stanu w celu aktualizacji: **useReducer**