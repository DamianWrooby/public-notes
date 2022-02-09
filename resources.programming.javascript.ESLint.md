---
id: fc9c0069-1828-4621-9ac3-5b8dc202f33f
title: ESLint
desc: ''
updated: 1624731181180
created: 1624730973904
---

# ESLint

Instalacja
```
npm install eslint --save-dev
```

Teraz mamy do wyboru dwie drogi: możemy albo sami wszystko skonfigurować od zera albo skorzystać z odpowiedniego polecenia

```
npx eslint --init
```

Po udzieleniu odpowiedzi na kilka pytań dostaniemy wstępnie skonfigurowany plik .eslintrc, który możemy następnie dowolnie konfigurować i zmieniać. Najważniejsze ustawienia znajdziemy w opcjach plugins, extends, rules - i też z nich będziemy najczęściej korzystać.

Komendy:
**--ext** - określamy rozszerzenie plików jakie mają być sprawdzane
**--fix** - jeśli chcemy naprawić kod

```
eslint src --ext .ts,.tsx
eslint src --ext .ts,.tsx --fix
```