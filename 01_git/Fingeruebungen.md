---
layout: default
title: Fingerübungen
nav_order: 9
parent: Level 1 - Git together
---

# Fingerübungen

## Übung 1 - Allein

Hab keine Sorge, dass Du etwas falsch machst. Diese Übung findet nur auf Deinem
lokalen Rechner statt. Am Ende kannst Du den Ordner mit dem Repository einfach
wieder löschen und alles ist gut.

- Erstelle mit `git init` ein lokales Repository auf Deinem Rechner.
- Füge nun wiederholt Dateien hinzu oder editiere ihren Inhalt und übe auf diesem
Weg den `git add`  & `git commit` Workflow.
- Lass Dir mit `git diff` die Änderungen anzeigen, bevor Du sie in die Staging Area
hinzufügst.
- Lass Dir Deine History mit `git log` oder `git log --oneline` anzeigen.
- Springe nun mit `git checkout <SHA>` zu einer früheren Version zurück. Schaue Dir
Deine Dateien im Editor an.
 Springe nun `git checkout master` wieder zur aktuellsten Version.

## Übung 2 - Zu zweit

Probiert mal aus, gemeinsam an einem Projekt zu arbeiten. Die folgende Anleitung
sieht zwar umfangreich aus, das Prinzip ist aber eigentlich einfach: `person1` 
legt ein Repository in GitLab an, `person2` erstellt davon einen Fork und fügt
eine Änderung hinzu. Sie möchte nun, dass diese Änderung zurück in das ursprüngliche
Repository von `person1` fließt...und die Geschichte nimmt ihren Lauf.

- `person1` legt auf [https://gitup.uni-potsdam.de](https://gitup.uni-potsdam.de/)
ein neues Repository an - nennen wir es `git-uebung2`. Dieses liegt dann also 
unter `.../person1/git-uebung2`.
- `person1` klont das Repository, fügt eine neue Datei `meins.md` mit einer neuen
Zeile hinzu. Dann `add`, `commit`, `push`.
- `person2` kommt ins Spiel: Sie erstellt einen Fork von `person1/git-uebung2` und
erhält somit auf Git.UP `.../person2/git-uebung2`. Nun klont `person2` den Fork
auf ihren Rechner. Dort fügt sie eine weitere Zeile zur Datei `meins.md` hinzu,
dann `add`, `commit`, `push`.
- Nun kommt's: `person2` möchte `person1` bitten, die neueste Änderung (eine
neue Zeile!) in ihr Repository `person1/git-uebung2` zu übernehmen. Denn die neue
Zeile stellt eine echte Verbesserung dar! Dafür geht `person2` auf die GitLab-Seite
`https://gitup.uni-potsdam.de/person2/git-uebung2` und stellt einen Merge-Request:
Im linken Sidebar `Merge Requests` auswählen --> `New Merge Request`. Der Source
Branch ist `person2/git-uebung2` (master) und der Target Branch ist `person1/git-uebung2`
(master). Nun auf `Compare branches and continue`. `person2` hat nun die Möglichkeit,
den Merge Request weiter zu beschreiben und anzupreisen. Anschließend auf
`Submit Merge Request`.
- `person1` kann auf der Seite `https://gitup.uni-potsdam.de/person1/git-uebung2`
unter Merge Requests den Merge Request inspizieren, kommmentieren oder ggf. auch
Änderungen anfordern. Falls `person1` zufrieden ist, kann sie den `Merge`-Button
drücken.
- Um das Ganze pefekt zu machen, sollte `person2` in ihrem lokalen Fork-Repository
noch das Ursprungsrepo (`upstream`) als remote hinzufügen, damit sie auch Änderungen
von `person1` in ihren Code einpflegen kann: `git remote add upstream https://gitup.uni-potsdam.de/person1/git-uebung2`.
Bitte den Erfolg mit `git remote -v` verifizieren. Nun kann `person2` mittels
`git pull upstream master` ihr lokales Repo mit dem remote `upstream` synchronisieren.
- Bitte probiert das gleich mal aus, indem `person1` ihr lokales Repo mittels
`git pull origin master` mit ihrem remote synchronisiert und nun gleich noch eine
dritte Zeile einfügt oder etwas in einer anderen Zeile ändert. Dann wieder `add`,
`commit` und `push`. Nun soll `person2` diese Änderung mittels `git pull upstream master`
in ihr lokales Repo holen.
- Wow...


 