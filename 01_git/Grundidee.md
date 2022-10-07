---
layout: default
title: Grundidee
nav_order: 2
parent: Level 1 - Git together
---

# Grundidee

Die Grundidee der Versionskontrolle ist dem "track changes"-Modus in Textverarbeitungsprogrammen
nicht unähnlich. Die folgende Abbildung zeigt nochmal, dass wir den Inhalt eines Dokuments
letztlich als eine Abfolge von Änderungen verstehen können.

![img](https://swcarpentry.github.io/git-novice/fig/play-changes.svg)

(Quelle: [Software Carpentry](https://swcarpentry.github.io/git-novice/01-basics/index.html))

Wenn wir ein Dokument als eine Abfolge von Änderungen verstehen, können dies 
natürlich Änderungen unterschiedlicher Nutzer sein:

![img](https://swcarpentry.github.io/git-novice/fig/versions.svg)

(Quelle: [Software Carpentry](https://swcarpentry.github.io/git-novice/01-basics/index.html))

Wenn unterschiedliche Nutzer nicht gerade an exakt der gleichen Stelle im Dokumment
editieren, kann man aus den Änderungen der Nutzer eine neue Dokumentversion erstellen. 

![img](https://swcarpentry.github.io/git-novice/fig/merge.svg)

(Quelle: [Software Carpentry](https://swcarpentry.github.io/git-novice/01-basics/index.html))

Falls Änderungen an der gleichen Stelle stattfinden, gibt es einen Konflikt, der
aufgelöst werden muss. Auch dafür bieten Versionskontrollsysteme Mechanismen an.


# Verteilte Versionskontrollsysteme

Das lokale Aufzeichnen von Änderungen ist bereits ein wesentliches Merkmal der
Versionskontrolle. Das volle Potenzial des Ansatzes wird aber deutlich, wenn
man mit anderen am gleichen Projekt zusammenarbeitet. Dafür nutzt man meist
eine zentrale, für alle erreichbare Plattform, auf der die Änderungen zusammenlaufen.
