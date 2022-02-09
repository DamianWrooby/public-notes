---
id: bc0V9BCIwLr3muqEfg8yh
title: Prisma
desc: ''
updated: 1644323360872
created: 1644322466620
---

Get started: https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-node-mysql

## prisma introspect

Za pomocą komendy _prisma introspect_ możemy podpiąć instniejącą bazę danych i prisma na jej podstwie utowrzy schemę.

## prisma migrate

Po zmianach w schemie możemy uruchomić:

```bash
npx prisma migrate dev --name init
```

_init_ to nazwa migracji.

**migrate dev** jest komendą deweloperksą i nie powinna być używana na produkcji

Utworzenie migracji bez wdrożenia jej do bazy:
```bash
npx prisma migrate dev --create-only
```

Możemy utworzyć migrację, wprowadzić w niej zmiany i następnie wdrożyć za pomocą:

```bash
prisma migrate dev
```

## prisma reset

```bash
npx prisma migrate reset
```

**prisma reset** to również komenda deweloperska. Usuwa wszystkie tabele i tworzy na nowo aplikując migracje

## prisma migrate deploy

W środowiskach produkcyjnym i testowym do wdrożenia migracji służy komenda:

```bash
npx prisma migrate deploy
```

The migrate deploy command:

- Does not issue a warning if an already applied migration is missing from migration history
- Does not detect drift (production database schema differs from migration history end state - for example, due to a hotfix
- Does not reset the database or generate artifacts (such as Prisma Client)
- Does not rely on a shadow database