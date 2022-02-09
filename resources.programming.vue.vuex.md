---
id: 1a0VA2jyiY7lPOkI53tWa
title: Vuex
desc: ''
updated: 1644313806999
created: 1632403110461
---

## State

Nie możemy aktualizować tablicy w stanie poprzez index:
```javascript
state.array[index] = payload;
```

Trzeba zrobić to splice'em lub poprzez specjalną metodę **Vue.set()**:
```javascript
Vue.set(state.array, index, payload);
LUB
state.array.splice(index, 1, payload);
```

## Getters

Gettery służą jakby do pobierania przygotowanych (obrobionych) danych ze stanu:

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

Do getterów możemy uzyskać dostęp poprzez właściwość stora:
```javascript
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```
Takie gettery są cachowane.

...lub poprzez metodę:
```javascript
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }

```
Gettery wywoływane w ten sposób nie są cachowane.

**Żeby przekazać argument do gettera, musimy w getterze zwrócić funkcję:**
```javascript
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

### Helper mapGetters

The mapGetters helper simply maps store getters to local computed properties:

```javascript
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

## Mutations

Mutacji używamy do pojedynczych zmian w stanie.
Mutacje powinny być zawsze operacjami synchronicznymi.
Mutacje uruchamiamy poprzez commit.

Mutacje powinny być proste i aktualizować stan. Akcje mogą być złożone i nigdy nie powinny aktualizować stanu.

## Actions

Akcje są podobne do mutacji, ale zamiast bezpośrednio mutować stan, wywołują mutacje.
Dzięki temu akcje mogą zawierać asynchroniczne operacje.
Akcje zawierają metody. 
Są stosowane np. do pobierania danych z API lub wywoływania mutacji.
Akcje uruchamiamy poprzez dispatch.

Akcje przyjmują **context**, który najczęściej odnosi się do głównego stora, ale może odnosić się do innego jego modułu. Poprzez context możemy wewnątrz akcji wywoływać gettery, mutacje i inne akcje poprzez **context.getters**, **context.commit** czy **context.dispatch**.

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Najczęściej stosuje się destrukturyzację dla context.commit:
```javascript
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

### Jak przekazywać payload z akcji do mutacji?

![](/assets/images/2021-09-23-18-53-06.png)

### Vuex vs Redux

W odróżnieniu od Reduxa, Vuex mutuje stan - można bezpośrednio nadpisać stan:

```javascript
this.state.users = [...users];
```

W Reduxie trzeba zwrócić nowy stan - tworzona jest nowa referencja:

```javascript
return {...state, ...users};
```