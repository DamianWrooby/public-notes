---
id: 767e52f5-c3ec-45ba-b590-5c792a886542
title: useEffect
desc: ''
updated: 1618679523027
created: 1618678931463
---

Każdy render ma swoje własne zmienne stanu, które są stałe w trakcie tego rendera 

Funkcja useEffect zmienia się z każdym renderem i za każdym razem widzi zaktualizowane zmienne stanu odpowiednie dla zakresu tego rendera

Żeby mieć dostęp do ostatniej zakutalizowanej wersji stanu wewnątrz useEffect musimy użyć ref:

```javascript
function Example() {
    const [count, setCount] = useState(0);
    const latestCount = useRef(count);
    
    useEffect(() => {
      // Set the mutable latest value
      latestCount.current = count;
      setTimeout(() => {
        // Read the mutable latest value
        console.log(`You clicked ${latestCount.current} times`);
      }, 3000);
    });
```

## Cykl uruchamiania useEffect

Kolejność uruchamiania useEffect i funkcji zwróconej w return:
 
  1. React renderuje UI w związku ze zmianą propsa

  2. Przeglądarka renderuje widok - widzimy UI dla nowego propsa

  3. React uruchamia funkcję zwróconą w return dla poprzedniego propsa
  
  4. React uruchamia useEffect dla nowego propsa


 Wszystkie zależnośći powinny znaleźć się w depsach
 
 Nie powinniśmy kłamać w dependencjach i nie umieszczać tam zależności, które powinny się tam znaleźć. Należy zmienić kod efektu tak, by nie korzystał z zależności, których nie chcemy.

```javascript
 useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    }, 1000);
    return () => clearInterval(id);
  }, [count]);
```

W tym przypadku możemy pozbyć się count z zależności poprzez przekazanie funkcji do setCount - tzw. Functional updates:
```javascript
useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);
```
Jeżeli aktualizujesz jedną zmienną stanu w oparciu o aktualną wartość innej zmiennej stanu, warto spróbować wymienić te zmienne używając hooka useReducer, by pozbyć się ich z deps

Dążymy do tego by zmniejszyć liczbę dependencji useEffect jednocześnie nie oszukując pomijając jakieś zależności. Dlatego musimy sprawić, by efekt był samowystarczalny =>

### Jak usunąć dependencję kiedy w useEffect aktualizujemy stan na podstawie poprzedniego lub innego stanu?
```javascript
const [state, dispatch] = useReducer(reducer, initialState);
const { count, step } = state;

const initialState = {
    count: 0,
    step: 1,
  };
  
  function reducer(state, action) {
    const { count, step } = state;
    if (action.type === 'tick') {
      return { count: count + step, step };
    } else if (action.type === 'step') {
      return { count, step: action.step };
    } else {
      throw new Error();
    }
  }

useEffect(() => {
  const id = setInterval(() => {
    dispatch({ type: 'tick' }); // Instead of setCount(c => c + step);
  }, 1000);
  return () => clearInterval(id);
}, [dispatch]);
```

### Jak usunąć dependencję kiedy w useEffect aktualizujemy stan na podstawie propsów?

Wstawiamy reducer wewnątrz komponentu żeby czytał propsa step:
```javascript
function Counter({ step }) {
    const [count, dispatch] = useReducer(reducer, 0);
  
    function reducer(state, action) {
      if (action.type === 'tick') {
        return state + step;
      } else {
        throw new Error();
      }
    }
  
    useEffect(() => {
      const id = setInterval(() => {
        dispatch({ type: 'tick' });
      }, 1000);
      return () => clearInterval(id);
    }, [dispatch]);
  
    return <h1>{count}</h1>;
  }
```
### Funkcje jako zależności

Jeżeli używasz jakiejś funkcji tylko w useEffect to przenieś ją do useEffect
```javascript
function SearchResults() {
    // ...
    useEffect(() => {
      // We moved these functions inside!
      function getFetchUrl() {
        return 'https://hn.algolia.com/api/v1/search?query=react';
      }
      async function fetchData() {
        const result = await axios(getFetchUrl());
        setData(result.data);
      }
  
      fetchData();
    }, []); // ✅ Deps are OK
    // ...
  }
```
Jeżeli funkcja nie korzysta z niczego z zakresu komponentu, a chcesz np. żeby była współdzielona przez dwa efekty useEffect to można ją wynieść poza komponent.

// ✅ Not affected by the data flow
```javascript
function getFetchUrl(query) {
  return 'https://hn.algolia.com/api/v1/search?query=' + query;
}

function SearchResults() {
  useEffect(() => {
    const url = getFetchUrl('react');
    // ... Fetch data and do something ...
  }, []); // ✅ Deps are OK

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... Fetch data and do something ...
  }, []); // ✅ Deps are OK

  // ...
}
```