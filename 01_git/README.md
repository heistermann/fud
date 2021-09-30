# Level 1: Zusammen arbeiten mit Git

In dieser Lektion werden wir das Konzept der *Versionskontrolle* kennenlernen.
Ein *Versionskontrollsystem* verfolgt Änderungen in Dateien und erlaubt es Euch,
zu früheren Versionen zurückzuspringen. Eine von Euch vielleicht bereits praktizierte
Form der Versionskontrolle seht Ihr im folgenden Bild.

![](http://phdcomics.com/comics/archive/phd101212s.gif)

(Quelle: Jorge Cham on [www.phdcomics.com](http://phdcomics.com/comics/archive/phd101212s.gif))

Ein *verteiltes Versionskontrollsystem* erlaubt es Euch, gemeinsam mit anderen an
Inhalten (z.B. Text, Code) zu arbeiten und Euren Arbeitsfortschritt zu teilen.

Das weltweit erfolgreichste Versionskontrollsystem heißt `git` - und mit `git`
werden wir in diesem Kurs arbeiten.

## Warum??

> Klingt relevant alles, aber auch ganz schön trocken und obendrein noch ziemlich techniklastig.
> Ist das wirklich so wichtig? Wir wollen doch eigentlich R oder Python lernen...

Warum also tun wir uns das an?

- Wir Dozent*innen in diesem Kurs haben das Marterial gemeinsam mit `git` entwickelt
und wir stellen Euch dieses Material in Form eine `git`-Repositories zur Verfügung.
Allein dafür benötigt Ihr minimale Kenntnisse in `git`.
- Wir möchten Euch ermuntern, von Beginn an Eure Arbeit in diesem Kurs mit `git`
zu begleiten. Versionskontrolle ist kein Hexenwerk, aber wie so vieles nutzt man
es nur, wenn man eine gewisse Routine darin entwickelt hat. Und diese Routine 
erzielt man nur durch regelmäßige Nutzung. Wir wollen dies
im positiven Sinne nutzen und starten darum gleich zu Beginn mit `git`.
- Im Sinne der Berufsvorbereitung ist der Umgang mit `git` eine Schlüsselqualifikation.
Sobald Ihr in einem professionellen Kontext Code schreibt, kommt Ihr um `git` nicht herum.
Dabei geht es für Geoökolog*innen gar nicht unbedingt um Softwareentwicklung, sondern
um das Schreiben von Code im Rahmen der Datenanalyse.
- Die Nutzung von `git`, zusammen mit cloud-basierten Hosting-Plattformen wie GitLab oder
GitHub, erlaubt Euch nicht nur, Euren Arbeitsfortschritt zu strukturieren und jederzeit
zu einem früheren Zustand zurückzuschalten. Ihr könnt es auch einfach als eine
komfortable Methode des Backups verstehen... 
- [GitHub](https://github.com) ist die erfolgreichste Plattform für kollaborative Softwareentwicklung.
Dort könnt Ihr nicht nur Eure Projekte veröffentlichen und teilen, sondern auch auf einfache Art und
Weise statische Webseiten entwickeln und hosten. Die Webseite, auf der Ihr Euch
gerade befindet, ist ein Beispiel dafür.


## Vorbereitung 1: Git installieren und einrichten

Auf dieser Seite findet Ihr Anleitungen für die Installation von `git` unter
Linux, Mac und Windows: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git.

```
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```
Unter [diesem Link](https://phoenixnap.com/kb/how-to-install-git-windows) gibt es noch einen Windows-spezifischen Walk-through durch die 
Installationsprozedur.

## Vorbereitung 2: Bei Git.UP registrieren

Öffnet in Eurem Browser die Adresse [https://gitup.uni-potsdam.de](https://gitup.uni-potsdam.de).

Git.UP ist eine von der Uni Potsdam betriebene Instanz von GitLab. GitLab wiederum ist
eine Plattform für Softwareentwicklung, die unter anderen die Speicherung und
das Management von "Repositories" erlaubt. Lösungen wir GitLab oder das noch
bekanntere [GitHub](https://github.com) sind heutzutage Dreh- und Angelpunkt kollaborativer 
(also gemeinschaftlicher) Softwareentwicklung.

Damit Ihr in diesem Kurs GitLab bzw. Git.UP nutzen können, müsst Ihr Euch einmalig
dort registrieren. Das Tolle: Ihr braucht dafür keinen Extra-Account, sondern könnt
Euch direkt mit Eurem UP-Account registrieren und anmelden.

![img](gitup_register1.png)


## Grundidee

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
editieren, kan man aus den Änderungen der Nutzer eine neue Dokumentversion erstellen. 

![img](https://swcarpentry.github.io/git-novice/fig/merge.svg)

(Quelle: [Software Carpentry](https://swcarpentry.github.io/git-novice/01-basics/index.html))

Falls Änderungen an der gleichen Stelle stattfinden, gibt es einen Konflikt, der
aufgelöst werden muss. Auch dafür bieten Versionskontrollsysteme Mechanismen an.


## Verteilte Versionskontrollsysteme

Das lokale Aufzeichnen von Änderungen ist bereits ein wesentliches Merkmal der
Versionskontrolle. Das volle Potenzial des Ansatzes wird aber deutlich, wenn
man mit anderen am gleichen Projekt zusammenarbeitet. Dafür nutzt man meist
eine zentrale, für alle erreichbare Plattform, auf der die Änderungen zusammenlaufen.
 

# Die volle Breitseite: fork, clone, edit, add, commit, push

Bevor wir uns das Konzept der Versionskontrolle weiter erarbeiten, wollen wir
lieber die Technik an einem Beispiel anwenden.

Ein "Repository" ist so etwas wie ein Projektverzeichnis, in dem ein einzelnes Dokument
oder aber auch eine Sammlung von Dokumenten oder Codedateien liegen kann.

Um bestimmte Abläufe kennenzulernen und zu üben, nutzen wir das bereits existierende
Repository [git-lernen](https://gitup.uni-potsdam.de/umweltdv/git-lernen). Bitte mal
im Browser öffnen.

![img](img/repo-git-lernen.png)

Schau Dich ruhig mal um - evtl. sieht das Repository mittlerweile ein bisschen
anders aus als auf dem Bild. Jetzt gerade enthält das Repository nur eine Datei
namens `READM.md`. Das `md` steht für Markdown. Markdown ist eine sogenannte 
Auszeichnungssprache, die wir später noch besser kennenlernen werden. Diese
Webseite habe ich übrigens auch in Markdown geschrieben.

### Fork

Ich möchte Dir aber keine Schreibrechte auf meinem Repository `umweltdv/git-lernen`
geben. Stattdessen legst Du innerhalb von GitLab eine Kopie des Repositories an. 
Innerhalb dieser Kopie hast **Du** dann alle Rechte. Eine solche Kopie nennt man auch
*Fork* (also Gabel), weil sich damit die Entwicklung des Repositories "aufgabelt",
nämlich in Deinen und meinen Zinken. Erstelle jetzt Deinen Fork, indem Du auf das
`Fork`-Button klickst.

![img](img/fork.png)

Du musst dann noch Deinen GitLab-Namespace auswählen, in welchem der Fork landen
soll - wahrscheinlich hast Du nur einen...

![img](img/fork2.png)

Der Browser sollte Dir dann nach kurzer Fortschrittsanzeige Deinen Fork
in Deinem eigenen Namespace zeigen. Yay.

![img](img/fork3.png)

Das ist nun Dein erstes "eigenes" Repository auf GitLab. In dieses "remote" willst
Du all die Änderungen einfügen, die Du *lokal* an dem Projekt vornimmst. Aber dafür 
musst Du das Projekt erstmal auf Deinen lokalen Rechner bekommen. Dafür geht es
weiter mit `git clone`.

### `git clone`

Bewege Dich auf Deinem eigenen Rechner in ein Verzeichnis, in dem Dein Fork
landen soll (also z.B. das Verzeichnis, in dem Du den ganzen Kram aus diesem Modul
ablegst). Öffne in diesem Verzeichnis die *Git Bash*. Unter Windows sollte das
einfach mit dem Kontextmenü über die rechte Maustaste gehen. Unter Linux öffnest
Du einfach ein Terminal in dem Verzeichnis.

Im Browser kannst Du die Adresse des Repositories kopieren und dann in die
Git Bash zusammen mit dem Befehl `git clone` einfügen.

![img](img/clone1.png) 

In der URL in der folgenden Befehlszeile müsste UPNUTZER also durch Deinen
Benutzernamen ersetzt werden:

`$ git clone https://gitup.uni-potsdam.de/UPNUTZER/git-lernen.git`

Anschließend wirst Du nach den Credentials Deines UP-Accounts gefragt und der
Download erfolgt.

![img](img/clone2.png)

Wirf nun einen Blick in Dein Verzeichnis - dort sollte nun das Unterverzeichnis
`umweltdv` aufgetaucht sein. Vergleiche den Inhalt des Unterverzeichnisses mit dem,
was Du im Browser in Deinem GitLab-Fork siehst.

Wechsle mit der Git Bash nun in das Verzeichnis `umweltdv` (`cd` steht für
change directory) und lass Dir den Inhalt anzeigen:

`$ cd umweltdv`


## Learning targets

- Understand the basic idea of version control
- But version control plus remote hosting can deliver many more benefits!
- Know the basic git commands
- Understand the fundamental git/GitLab workflow
- Basic Markdown 


## Structure and main contents

- The basic idea of version control 
- Examples for additional benefits: clarity, sanity, collaboration, hosting, ...
- Examples from real life GitHub repositories that are maintained by us (wradlib, ...)
- Why start with version control before we even started coding? The fundamental 
setup of how we will use Git and GitLab for this course
- Git cheat sheet
- Create a repository in GitLab, clone it to your local machine, edit readme,
commit, push, voila.
- Fork, clone, edit, commit, push, pull request, voila.
- Basic Markdown


## Exercises during the main course
- Write a poem together: each verse is a different file, teacher merges all verses
using some Python code

## Which dataset will be used?
For now: none.

## Optional: Which contents/skills will you address that were not explicitely addressed before?
None.

## Optional: Upon which contents/skills will you explicitely build?
None.

## Optional: Ideas for tasks in the Coding-Lab
- Write a poem/story together: each verse is a different file, submit via pull request 
(teacher merges all verses using some Python code)

## Optional: External resources upon which this lesson builds
- [Git and Github](https://www.earthdatascience.org/courses/intro-to-earth-data-science/git-github/)

