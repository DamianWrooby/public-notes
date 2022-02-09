---
id: 4f940b11-441a-49ab-9a4d-2c29ad482276
title: useContext
desc: ''
updated: 1618680325045
created: 1618680202922
---

Context powinien być stosowany do przechowywania danych pojedyńczej warstwy aplikacji, które nie zmieniają się często (np. motyw). Częsta zmiana może prowadzić do częstego rerenderowania całego drzewa komponentów.


1. Zawsze eksportujemy własny provider żeby mieć lepszą kontrolę nad value

2. Warto zadbać o API żeby mieć pewność, że context będzie aktualizowany w odpowiedni sposób

3. Tworzymy custom hook dla naszego contextu

```javascript
const ThemeContext = React.createContext()

export function ThemeProvider({ children }) {
  const [theme, setTheme] = React.useState('light')

  const handlers = React.useMemo(
    () => ({
      lighten: () => {
        setTheme('light')
      },
      darken: () => {
        setTheme('dark')
      },
      toggle: () => {
        setTheme(s => (s === 'light' ? 'dark' : 'light'))
      },
    }),
    []
  )

  return (
    <ThemeContext.Provider value={[theme, handlers]}>
      {children}
    </ThemeContext.Provider>
  )
}

export const useThemeContext = () => React.useContext(ThemeContext)
```

Aby zadbać o wydajność warto zastosować memoizację, by zapobiec niepotrzebnemu rerenderowaniu
```javascript
function Header() {
    const [theme, { toggle }] = useThemeContext();
  
    return React.useMemo(
      () => (
        <header
          style={{
            backgroundColor: theme === 'light' ? '#eee' : '#111',
          }}
        >
          <Nav />
          <button onClick={toggle}>Toggle theme</button>
        </header>
      ),
      [theme, toggle]
    );
  }
```
### Remember
1. Don't worry about a default value
2. Think locally and small before globally
3. Make your own Provider
4. Expose an API
5. Export a custom hook, don't bother with aConsumer render prop component
6. Memoize the components that consume thecontext, either by:
7. Container and React.memoized presentationalcomponents
8. React.useMemo the component's output
  