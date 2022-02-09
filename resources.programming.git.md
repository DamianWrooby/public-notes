---
id: 80226dba-7f88-4901-a0a1-89255d005e8c
title: Git
desc: ''
updated: 1644318815811
created: 1623499470682
---

**Usuwanie branchy:**

```bash
// delete branch locally
git branch -d localBranchName

// delete branch remotely
git push origin --delete remoteBranchName
```

**Podgląd wersji pliku z konkretnego commita:**

```bash
git show REVISION:path/to/file
```
REVISION - to np. numer commita

**Utworzenie brancha z poprzedniego commita:**

```bash
git branch branchname <sha1-of-commit>
```

**Usunięcie z historii ostatniego commita z zachowaniem wszystkich zmian w plikach:**

```bash
git reset --soft HEAD~1
```
~1 to numer commita:

git log --oneline 

3fad532 Last commit (HEAD) 

3bnaj03 Commit before HEAD (HEAD~1) 

vcn3ed5 Two commits before HEAD (HEAD~2)

**Usunięcie lub dodanie plików do poprzedniego commita bez zmiany nazwy**
```bash
git add .

git commit --amend --no-edit

git push --force-with-lease <repository> <branch>
```

**Zmiana commit message po pushu**

```bash
git commit --amend -m "New message"

git push --force-with-lease <repository> <branch>
```

## GitLab - zmiana hasła

1. git config -e
2. Wstawiamy url = https://**username**@services-gdcw.it-solutions.myatos.net/gitlab/EPI-Front/bootcampfront2021/damianwroblewski/nuxt-training-tracker.git
3. Przy następnym pushu poprosi nas o hasło i będzie można zaktualizować

## Merge vs Rebase

[Link](https://ohmydev.pl/post/git-merge-vs-git-rebase-jakie-sa-roznice-hod)

W skrócie różnica jest taka, że przy zwykłym mergu tworzy się merge commit, a w przypadku rebase nie, co pozwala zachować większy porządek.

W przypadku 3-way merge tworzony jest pomocniczy commit:
![](/assets/images/2021-12-14-22-09-32.png)

**M5 = M4 + F1 + F2**

Dodatkowy commit, który powstaje przy operacji merge nie zawsze jest mile widziany w historii projektu. Są osoby/zespoły, którym zależy aby po połączeniu zmian zachować nieprzerwaną liniowość w historii, która może dawać większe możliwości w zakresie analizy, testów czu debuggowania błędów. W takiej sytuacji bardzo często jako alternatywa dla merge używany jest właśnie rebase.

**Podstawową zasadą, o której należy pamiętać jest fakt, iż rebase modyfikuje historię projektu. Dlatego nie powinien być stosowany na gałęziach prywatnych typu main.**

![](/assets/images/2021-12-14-22-15-59.png)

## Git Flow

Git flow to najpowszechniej stosowana metodologia pracy z git w projektach. 

Jak widać poniżej z gałęzi Main nie wychodzą żadne inne gałęzie oprócz **Hotfix**. Gałąź **hotfix** po zmergowaniu do **main** musi również zostać zmergowana do **develop**.

Workflow wygląda następująco:
- tworzymy od głównej gałęzi gałąź **develop**, z której wychodzą gałęzie **feature** odpowiednio oznaczone np. nr taska, inicjałami itp.
- po skończonej pracy, mergujemy **feature** do gałęzi **develop**,
- jak uzbiera się odpowiednia ilość featureów na kolejny **release**, tworzymy gałąź **release**, na której commitujemy rzeczy związane z releasem czyli dokumentację itp.
- kiedy **release** jest gotowy, jest mergowany do **main** i ten merge opatrzony jest odpowiednim tagiem,
- na koniec mergujemy też **release** do **develop**

![](/assets/images/2021-12-14-22-08-50.png)

## Dobre praktyki

**C - Clear History**
Historia ma być czysta, jak Twój pokój po przeczytaniu książki Jordana Petersona! Ewentualnie jak dobra polska gorzałka! Każdy commit ma być sensowny i ma mieć znaczenie, żadnych "quick fix", "small refactoring". I żadnych gównobranczy sprzed 5 lat...


**H - Hermetic PR**
Jak robisz PR, to ten PR ma być na konkretną zmianę - konkretną funkcjonalność albo jej logiczną część. A jak się nie da w osobnych PR, bo się nie buduje, to rusz głową, żeby dać lepszy tytuł niż "Add new table + add migration + add some test + fix some issues + use new table in code"


**A - Alternative Review**
Code Review nie polega na tym, że patrzysz czy testy zielone i czy u Ciebie działa. Nawet nie wystarczy, czy zrobisz czecklistę SOLID, KISS, MPK, MPO, PDK... Pomyśl, czy nie da się tego zrobić lepiej niż autor PR, to zrobił. Jak się da to zaproponuj, pomyślcie razem.


**D - Detailed Comment**
Bardziej szczegółowe C. Napisz porządnie tę wiadomość. Wiem, że zostałeś programistą, bo ledwo Cię przepuścili w gimbazie za oblanie 99% sprawdzianów z polskiego, ale jak już masz te 20k, umiesz te śmieszne cyferki i literki, to dobrze by było, jakbyś umiał opisać słowami to, co zrobiłeś w kodzie. Nie jakieś gunwo typu "use new method", "fix bug", "modify interface".