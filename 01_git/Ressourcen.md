---
layout: default
title: Weitere Quellen
nav_order: 8
parent: Level 1 - Git together
---

# Weitere Quellen

## Cheatsheets und weiterführende Git-Kurse

Es gibt wirklich grandiose Ressourcen im Netz, um sich weiter mit dem Thema zu beschäftigen.
Hier nur eine kleine Auswahl:

- [Git Cheatsheet No 1](https://education.github.com/git-cheat-sheet-education.pdf)
- [Git Cheatsheet No 2](https://www.atlassian.com/dam/jcr:e7e22f25-bba2-4ef1-a197-53f46b6df4a5/SWTM-2088_Atlassian-Git-Cheatsheet.pdf)
- [Git-Kurs No 1](https://www.earthdatascience.org/courses/intro-to-earth-data-science/git-github/)
- [Git-Kurs Software Carpentry](https://swcarpentry.github.io/git-novice/index.html)
- [Git-Book](https://git-scm.com/doc)


## Ausgewählte Situationen und Aufgaben mit `git`  

### Änderungen betrachten

Du möchtest vor (oder nach) dem Stagen prüfen, was sich gegenüber dem letzten
Commit geändert hat? Das geht mit `git diff` (vor dem Stagen) bzw. mit `git diff --staged`
(nach dem Stagen).

### Zurück zu einer früheren Version springen

Die Möglichkeit, zu einer früheren Version Deines Projekts zu springen, wird immer
wieder als ein wichtiges Feature einer Versionskontrolle angegeben.

Zunächst musst Du die frühere Version identifizieren, zu der Du springen möchtest.
Jeder Commit in der History hat eine eindeutige ID mittels eines sogenannten Hashs
(SHA). Mit `git log` kannst Du Dir die History anschauen und den passenden commit-SHA
finden (mit `git log --oneline` ist der Output noch übersichtlicher).

```
$ git log --oneline
25d9b5d (HEAD -> master) My third commit.
20e63b3 My second commit.
58d74a2 First commit.
```

Nun kannst Du zum betreffenden Commit springen, z.B. zum "First commit":

`$ git checkout 58d74a2`

Wenn Du nach dem Befehl Deine Dateiinhalte überprüfst, sollten sie mit der früheren
Version ("First commit") übereinstimmen. Der Befehl ist aber nicht dazu angetan, um von hier aus
dauerhaft weiterzuarbeiten. Zunächst springst Du also bitte zurück zur aktuellen Version:

`$ git checkout master`

oder auch mit

`$ git checkout -`

Wenn Du Dich nun entscheidest, dauerhaft zur früheren Version `SHA` zurückzuspringen
und von dort weiterzuarbeiten, hast Du zwei Möglicheiten: Mit `reset` setzt Du
Dein lokales Repository in einen früheren Zustand. Das ist ok, wenn Du nur allein
und lokal arbeitest. Besser ist es aber, den Sprung in einen früheren Zustand
mit einem neuen Commit transparent zu machen. Dazu gibt es den `revert`-Befehl.
Auf diese Weise bleibt die History intakt - wir wollen üblicherweise
nicht die Geschichte umschreiben.

Den letzten Commit (`HEAD`) machen wir mit folgendem Befehl
rückgängig (und rückgängig heißt: Wir erstellen einen neuen Commit, der den letzten
"auslöscht"):

```
Nur den letzten Commit "reverten"
$ git revert HEAD
```
Wenn wir aber noch weiter zurückgehen wollen, müssen wir alle Commits auf dem
Weg dahin reverten. Dafür gibt es unterschiedliche Schreibweisen. Wenn wir z.B.
zum "First commit" zurückmöchten, haben wir folgende Möglichkeiten: 

```
Jeden commit, der "revertet" werden soll, einzeln angeben
$ git revert 25d9b5d 20e63b3

Jeden commit zwischen DA und DORT reverten (mit DA..DORT - schließt DA nicht ein!) 
$ git revert 58d74a2..25d9b5d
```

Allerdings musst Du auf diese Weise für jeden Schritt zurück (also jeden reverteten Commit)
eine Commit-Message angeben...grmpf. Nun sieht Deine History wie folgt aus:

```
5588a5e (HEAD -> master) Revert "My second commit."
433a303 Revert "My third commit."
25d9b5d My third commit.
20e63b3 My second commit.
58d74a2 First commit.
```


Gar nicht so einfach, dieses einfache Feature. In 
[diesem Artikel](https://opensource.com/article/18/6/git-reset-revert-rebase-commands)
werden die unterschiedlichen Möglichkeiten noch mal ausführlich beschrieben.


### Nicht committete Änderungen verwerfen

Wenn Du merkst, dass Du gerade nur Müll produziert hast und einfach alles
wieder loswerden möchtest (also alles, was sich innerhalb und außerhalb der
Staging Area befindet), dann kannst Du dies mit `git reset --hard` oder
`git checkout master` erreichen. Wenn Du nur Änderungen in einer Datei `<file>`
verwerfen möchtest, kannst Du auch `git checkout -- <file>` verwenden.


### Konflikte lösen

Was tun, wenn Du au einem Remote Repository pullst und die Änderungen im Remote
mit Deinen lokalen Änderungen im Konflikt stehen. Sowas möchte man gern vermeiden,
aber manchmal muss es eben sein: Hier kommt man um manuelle Konfliktlösung
üblicherweise nicht herum. Unter diesem [Link](https://docs.github.com/en/github/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line) gibt es eine ausführliche Anleitung dazu.

Wenn Du Dir ganz sicher bist, dass Du die Remote Änderungen (in der git-Terminologie "theirs")
übernehmen möchtest, kannst Du im Falle eines Konflikts auch

```
git checkout --theirs .
git add .
```

bzw. umgekehrt nur *Deine* Änderungen akzeptieren mit

```
git checkout --ours .
git add .
```

### Noch weitere Anwendungsfälle im Kopf?

Hast Du noch weitere Schnipsel, die man hier festhalten sollte? Dann sag Bescheid.

