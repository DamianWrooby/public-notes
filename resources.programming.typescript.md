---
id: 60ecc57c-2551-4fb2-bd6d-5d0ee2cb21ec
title: TypeScript
desc: ''
updated: 1627291548530
created: 1619631232369
---
## React

Najczęsciej dokonujemy destrukturyzacji propsów dlatego nie możemy typować propsów bezpośrednio(każdego z propsów osobno), a musimy stworzyć typ np.
```javascript
type Props = { 
    value: string; 
    value_2: number;
    }

const App = ({value, value_2, ...props}: Props) => {}
```

Możemy otypować to, co nasz komponent zwraca jako JSX.Element, FC lub FunctionComponent ale lepiej jest polegać na inferencji typów w tym przypadku dlatego, że jeśli otypujemy nasz komponent jako FC to automatycznie akceptuje on *children* nawet jeśli tego nie robi.

Default props:
```jsx
const defaultJoke: JokeProps = {
  joke: 'LOL JOE',
  id: 'YEAH',
  status: 200,
};

function JokeItem({ joke = defaultJoke }: JokeProps): JSX.Element {
  return (
    <li>
      {joke.joke} = {joke.id}
    </li>
  );
}
```

### Stan

Jeżeli stan jest nieskomplikowany można polegać na inferencji np.:

```jsx
const [user, setUser] = useState<User | null>(null);
```

### useEffect

Nic nadzywczajnego.

#### Użycie void.

*Good use of void: If you want to use a Promise function but not worry about await or .then(), you can pop a void in front of it:*

```jsx
useEffect(() => {
console.log('Mounted');
// getJoke().then(console.log).catch(console.error);
void getJoke();
}, [getJoke]);
```

### Events

Można obsłużyć zdarzenie inline i bazować na inferencji:

```javascript
onClick={e => yeah(e.target)};
```
lub

```javascript
const onSetType = (e: React.ChangeEvent<HTMLSelectElement>) =>
  setType(e.target.value)
```

### Generics - typy generyczne

Typy generyczne stosujemy np. gdy chcemy, by typ zwracany przez funkcję był taki sam jak typ argumentów:

```javascript
const last = <T>(arr: T[]): T => {
  return arr[arr.length - 1];
}
// Teraz zwrócona wartość będzie miała typ argumentu:
const l1 = last([1, 2, 3]); // number
const l2 = last(['a', 'b']); // string

const makeArr = <T, Y>(a: T, b: Y) => {
  return [a, b]; 
};

// Zwracane a i b będą miały typy T i Y np.
const array = makeArr(1, 'a'); // number, string
```