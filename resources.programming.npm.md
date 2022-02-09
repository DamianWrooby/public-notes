---
id: f3c59d37-a74b-46a3-8cbd-77c873f332bf
title: Npm
desc: ''
updated: 1643464414192
created: 1627547176295
---

Dev dependencies nie trafiają na produkcję tylko uruchamiane są lokalnie dlatego podatności z nimi związane nie są podatnościami krytycznymi. Paczki NPM mają również swoje zależności, które muszą być doinstalowywane i następnie utrzymywane.

## package.json i package-lock.json

Plik package.json zawiera tylko bezpośrednie zależności, nie zawiera zależności naszych zależności (nested dependencies). Dlatego też nie mamy kontroli nad wersjami zagnieżdżonych zależności poprzez standardowy **package.json**.

Ze stackoverflow:
_Even if you lock down the versions of your direct dependencies you cannot 100% guarantee that your full dependency tree will be identical every time. Secondly you might want to allow non-breaking changes (based on semantic versioning) of your direct dependencies which gives you even less control of nested dependencies plus you again can't guarantee that your direct dependencies won't at some point break semantic versioning rules themselves._

Natomiast plik **package-lock.json** zawiera już dokładne wersje całego drzewa zależności.

_The solution to all this is the lock file which as described above locks in the versions of the full dependency tree. This allows you to guarantee your dependency tree for other developers or for releases whilst still allowing testing of new dependency versions (direct or indirect) using your standard package.json._

## Aktualizacja zależności

W przypadku major wersji paczek aktualizujemy pojedynczo i sprawdzamy co się wywaliło googlując błędy. W przypadku dużych, istotnych paczek jak frameworki czy meta-frameworki warto sprawdzić changelog i instrukcję migracji. Najpierw aktualizujemy mniejsze zmiany minor a później major.

npm-upgrade

