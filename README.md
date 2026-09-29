# git-collab-game
Prosta gra dla przećwiczenia współpracy w zespole podczas rozwijania jednego repozytorium

## ZESPÓŁ

Zespół składa się z trzech osób:
- osoba A - Release Manager
- osoba B - Programista Modułu
- osoba C - Tester Integrator

### Właściciel repozytorium
- Jedna osoba z zespołu wykonuje forka niniejszego repozytorium. 
- Na profilu właściciela pojawia się repozytorium LOGIN_GITHUB/git-collab-game.
- Właściciel daje dostęp do repozytorium osobom ze swojego zespołu:

  ```Repository → Settings → Collaborators → Add people```

## PRZYGOTOWANIE
- Każdy członek zespołu klonuje repozytorium od właściciela do lokalnego folderu (pendrive...)

## RUNDA 1: SZTAFETA PO MAIN/MASTER

__Cel:__ zobaczyć najprostszy przypadek synchronizacji, w którym pull --ff-only wykonuje fast-forward.

### 1. Osoba A - release manager - wykonuje:

```
git checkout main
git pull --ff-only
```

po czym edytuje plik `release-room/status.md`:
```
Koordynator: LOGIN_OSOBY_A
```

by finalnie:
1. dodać plik do stage'a
2. wykonać commita zmian
3. wypchnąć zmiany do origina

### Osoba B - programista modułu:
1. Czeka na sygnał od OSOBY A, że może działać
2. wykonuje:

```
git checkout main
git pull --ff-only
git log --oneline -5
```

3. Po potwierdzeniu, że w logu widać commit OSOBY A, edytuje plik `release-room/modules/logika.md`:
```
# Moduł logiki

Odpowiedzialny: LOGIN_OSOBY_B
Stan: GOTOWY
Opis zmiany: Dodano walidację danych wejściowych.
```

by finalnie:
1. dodać plik do stage'a
2. wykonać commita zmian
3. wypchnąć zmiany do origina

### Osoba C - tester integrator:
1. Czeka na sygnał od OSOBY B, że może działać
2. Wykonuje:

```
git checkout main
git pull --ff-only
git log --oneline -5
```

3. Po potwierdzeniu, że w logu widać commity OSOBY A i OSOBY B, edytuje plik `release-room/modules/testy.md`:
```
# Testy

Odpowiedzialny: LOGIN_OSOBY_C
Stan: GOTOWY
Opis zmiany: Sprawdzono podstawowe scenariusze wydania.
```

by finalnie:
1. dodać plik do stage'a
2. wykonać commita zmian
3. wypchnąć zmiany do origina

### WSZYSCY
1. Po sygnale od OSOBY C, że zakończyła pracę, wykonują:
```
git pull --ff-only
git log --oneline -5
```


## RUNDA 2: PRACA RÓWNOLEGŁA, NA BRANCHACH

Wszyscy zaczynają od identycznego stanu na głównym branchu:

```
git checkout main
git pull --ff-only
```

### 1. WSZYSCY

Każda z osób w zespole tworzy brancha (lokalnie, u siebie) o unikalnej nazwie, według schematu:

```
git checkout -b feature/interfejs-loginOsobyNaGithub
```

### 2. OSOBA A:

Edytuje plik: `release-room/modules/interfejs.md`, a następnie:

```
git add release-room/modules/interfejs.md
git commit -m "Przygotuj moduł interfejsu"
git push -u origin feature/interfejs-loginOsobyANaGithub
```

### 3. OSOBA B:

Dodaje bardziej szczegółową informację do modułu logiki: `release-room/modules/logika.md`, a następnie:

```
git add release-room/modules/logika.md
git commit -m "Uzupełnij opis walidacji"
git push -u origin feature/logika-loginOsobyBNaGithub
```

### 4. OSOBA C:

Dodaje wyniki testów `release-room/modules/testy.md`, a następnie:

```
git add release-room/modules/testy.md
git commit -m "Uzupełnij wyniki testów"
git push -u origin feature/testy-loginOsobyCNaGithub
```

### 5. INTEGRACJA BRANCHY

OSOBA A - Release Manager - przechodzi na branch `main`:

```
git checkout main
git pull --ff-only
```

Następnie pobiera i integruje branche od wszystkich członków zespołu:

```
git pull --no-rebase --no-edit origin feature/interfejs-loginOsobyANaGithub
git pull --no-rebase --no-edit origin feature/logika-loginOsobyBNaGithub
git pull --no-rebase --no-edit origin feature/testy-loginOsobyCNaGithub
```

By finalnie wypchnąć zintegrowany branch `main`

```
git push origin main
```

