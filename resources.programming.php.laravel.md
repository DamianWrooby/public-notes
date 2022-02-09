---
id: YLMPsjCrqZVdRtIjm4wri
title: Laravel
desc: ''
updated: 1643313023197
created: 1643277539294
---
Tutorial: https://www.youtube.com/watch?v=g_22EUfibJ8

## Komendy artisan

php artisan list - lista komend

php artisan migrate - uruchomienie migracji

php artisan migrate:refresh - cofnięcie migracji i uruchomienie jeszcze raz

// Refresh the database and run all database seeds...
php artisan migrate:refresh --seed

php artisan migrate:fresh - usunięcie wszystkich tabel i uruchommienie migracji

php artisan make:co? nazwa - utworzenie nowej instancji modelu, migracji, kontrolera itp.

php artisan make:model Post -fmc - utworzy model Post oraz powiązane z nim factory, migration i controller

### Uruchomienie tinkera i wypełnienie bazy
```
php artisan tinker
```

```
App\Models\Post::factory()->count(10)->create()
```
## Database queries

Wszystkie wiersze z tabeli:
```php
$users = DB::table('users')->get();
```


Pojedynczy wiersz z tabeli:
```php
$user = DB::table('users')->where('name', 'John')->first();
// pierwszy wiersz z tabeli users, gdzie wartość kolumny 'name' wynosi 'John'
```

Poszczególne kolumny:
```php
$users = DB::table('users')
            ->select('name', 'email as user_email')
            ->get();
// wszystkie wiersze z tabeli users, ale jedynie kolumny 'name' i 'email'
```

Jeśli masz zbudowane zapytanie i chcesz jedynie dodać jakąś kolumnę:
```php
$query = DB::table('users')->select('name');

$users = $query->addSelect('age')->get();
```

### Join

Pierwszy argument przekazywany do metody join to nazwa tabeli, do której chcesz dołączyć, podczas gdy pozostałe argumenty określają ograniczenia dotyczące kolumn dla złączenia. Możesz nawet połączyć wiele tabel w jednym zapytaniu:
```php
$users = DB::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();

$users = DB::table('users')
            ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();
```

Więcej [tutaj](https://laravel.com/docs/8.x/queries).

## Token

Token nie powinien być przekazywany w query params, bo parametry są logowane m.in. przez dostawcę internetu. Doklejanie ich natomiast do body przy każdym requeście mogłoby być uciążliwe. Trzeba by tworzyć jakieś interceptory, które automatycznie robiłyby to za nas.

Dlatego też stosuje się Bearera - przekazujemy w headersach
_Authorization: Bearer {token}_

Tworzenie hashy w laravelu:
```php
$hash = Hash::make('test'); // $2y$10$rk4tPjLJyJ5iwWUMp/RSke.eye9fBGjjIjOjvVPxXu2gfo2AxxFpG
$check = Hash::check('test', $hash); // true
```

Lub poprzez fasady szyfrujące:
```php
$encrypt = Crypt::encrypt('test 5');
var_dump($encrypt);

$decrypt = Crypt::decrypt($encrypt);
var_dump($decrypt);
```

Klucz szyfrujący jest tworzony wraz z tworzeniem projektu i znajduje się w zmiennych środowiskowych pod nazwą APP_KEY.
Kiedy przenosimy projekt na produkcję np. warto odświeżyć klucz. Trzeba natomiast uważać żeby nie odświeżać klucza, gdy jakieś dane są już zaszyfrowane, bo już ich nie odszyfrujemy. Dobrą praktyką jest zachowanie gdzieś klucza.

Warto określić czas żywotności tokena.
Warto skojarzyć dany token z użytkownikiem i weryfikować czy zalogowany użytkownik to ten sam co posługuje się tokenem żeby zapobiec sytuacji, gdzie zalogowany użytkownik podmienia sobie token na token admina np.

Zazwyczaj przy systemie, który nie przechowuje danych wrażliwych token odświeża się przy każdym zalogowaniu użytkownika.