# Remote

Większość pracy w Git odbywa się lokalnie, w Twoim repozytorium. Git posiada tylko dwa (to oczywiście kłamstwo, ale chciałbym żebyście tak uważali na czas tego warsztatu) polecenia wychodzące poza nasz komputer: `fetch` i `push`.

## Co to jest remote?

Remote to odnośnik do innego repozytorium. Może się ono znajdować na innym komputerze, albo nawet na naszym, ale w innym katalogu. Lokalne repozytorium jest jedno, a zdalnych może być wiele.

Wpisz

```
(1) $ git remote
(2) $ git remote -v (--verbose)
```

aby zobaczyć remote'y dodane do Twojego repozytorium.

Zazwyczaj nie ma potrzeby konfigurowania remote'ów ręcznie. Domyślny remote o nazwie `origin` tworzy się automatycznie przy pierwszym pobraniu repozytorium zdalnego za pomocą polecenia

```
$ git clone <adres>
```

Clone powoduje pobranie na nasz komputer całej zawartości zdalnego repozytorium.

## Pobieranie zmian

Podstawowym poleceniem do ściągania na nasz komputer zmian (commitów) znajdujących się w zdalnym repozytorium jest polecenie `fetch`.

```
(1) $ git fetch
(2) $ git fetch --all
(3) $ git fetch <remote>
```

Po wykonaniu tego polecenia w naszym repozytorium pojawią się wszystkie branch'e, tagi i znajdujące się na nich commity.

## Zdalne branche

Pobrane do naszego repozytorium branche odrobinę różnią się od "zwykłych" branchy.

* mają specjalną nazwę: `<remote>/<branch>`, np. `origin/master`,
* nie można ich checkout'ować i tworzyć na nich commitów (nie mogą być *aktywne*).

By zaktualizować branche zdalne, niezależnie od tego co w danej chwili robimy w repozytorium, wystarczy tylko ponownie wywołać polecenie `fetch`. Jest to bardzo wygodne.

Dodatkowo poleceniem

```
$ git fetch -p (--prune)
```

możemy usunąć branche zdalne, które już nie istnieją w zdalnym repozytorium (bo ktoś je usunął).

Skoro na branchach zdalnych nie da się pracować, to do czego nam takie branche? Mamy kilka możliwości:

* Można zobaczyć co nowego pojawiło się na branchu:
  * `(1) $ gitk origin/master`
  * `(2) $ git log origin/master`
  * `(3) $ git diff origin/master`
* Można stworzyć branch lokalny, który zawiera te same zmiany co zdalny i normalnie na nim pracować, tak jakbyśmy byli na branchu zdalnym:
  * `(1) $ git checkout foobar` (dla brancha zdalnego `origin/foobar`)
  * `(2) $ git checkout -b foobar -t (--track) origin/foobar`

Gdy już poczujesz się pewnie z obsługa polecenia `fetch`, wpisz `$ git help pull` i poczytaj o poleceniu `pull`. W niektórych sytuacjach jest ono wygodniejsze w użyciu niż `fetch`.

## Aktualizowanie repozytorium zdalnego

Gdy zakończymy pracę, wypada podzielić się nią z innymi i wgrać nasze zmiany do repozytorium zdalnego. Służy do tego polecenie `push`.

```
(1) $ git push
(2) $ git push <remote> HEAD
(3) $ git push <remote> <branch-lokalny>:<branch-zdalny>
```

Polecenie (1) wymaga aby branch lokalny był skojarzony z branchem zdalnym (tzw. *tracking*). Jest to zwykle automatycznie ustawiane przy checkout'owaniu brancha. Polecenia (2) i (3) działają zawsze. Śledzenie można ustawić ręcznie poleceniem

```
$ git branch -u (--set-upstream) <remote>/<branch>
```

a stan śledzenia poleceniem

```
(1) $ git status
(2) $ git config -l
```

Jeśli polecenie `push` zakończy się powodzeniem, branch zdalny w repozytorium lokalnym zostanie zaktualizowany, tak że będzie wskazywał na ten sam commit.

UWAGA: Jeśli nadal używasz Git'a w wersji wcześniejszej niż 2.x, to przeczytaj pomoc. `push` bez dodatkowych argumentów działał na *wszystkie* branche lokalne, a nie branch na którym obecnie się znajdujemy.
