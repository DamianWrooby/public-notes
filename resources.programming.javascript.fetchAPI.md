---
id: f3dea1eb-99a8-41ee-8708-1bd28ed48867
title: fetchAPI
desc: ''
updated: 1624730825341
created: 1624729977233
---
# FetchAPI


**Fetch zwraca obetnicę.**

Podstawowa składnia fetch ma postać:

```javascript
fetch(url, [options]);
```

Fetch zwraca obietnicę, więc tak samo jak w tamtym rozdziale, możemy je skonsumować za pomocą dostępnych metod - then(), catch(), all() itp.

```javascript
fetch("https://restcountries.eu/rest/v2/name/Poland")
    .then(res => res.json())
    .then(res => {
        console.log(res);
    })
    .catch(error => console.log("Błąd: ", error));
```

Aby uzyskać pełną odpowiedź w interesującym nas formacie powinniśmy zastosować odpowiednią funkcję. W naszym przypadku oczekujemy formatu JSON, więc zastosujmy **response.json()**. Dla innych typów danych trzeba by użyć innych metod.

## Obsługa błędów

Jeżeli wyślemy zapytanie na błędny adres to nie zostanie uruchomiony **catch()**

Czasem połączenie zakończy się powodzeniem ale po prostu zrobimy je na adres, który nie istnieje. Jeżeli takiego połączenia w ogóle nie udało by się nawiązać (np. nie ma dostępu do internetu), wtedy faktycznie zwrócony by został reject(), a co za tym idzie - zadziałał by catch().

Idąc więc za krokiem - aby dodatkowo obsłużyć potencjalne błędy przy naszych połączeniach, powinniśmy wykonać dodatkowe testy:

```javascript
fetch("https://restcountries.eu/rest/v2/name-anka-kaszanka/Poland")
    .then(res => {
        if (res.ok) {
            return res.json()
        } else {
            return Promise.reject(`Http error: ${res.status}`);
            //lub rzucając błąd
            //throw new Error(`Http error: ${res.status}`);
        }
    })
    .then(res => {
        console.log(res)
    })
    .catch(error => {
        console.error(error)
    });
```