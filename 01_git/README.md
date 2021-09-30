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

## Nutzen Eure Dozent:innen das denn auch?

- https://github.com/wschwanghart
- https://github.com/heistermann
- https://github.com/TillF
- 
- 
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
 

## Die volle Breitseite: fork, clone, add, commit, push

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

### git clone

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
`git-lernen` aufgetaucht sein. Vergleiche den Inhalt des Unterverzeichnisses mit dem,
was Du im Browser in Deinem GitLab-Fork siehst.

Wechsle mit der Git Bash nun in das Verzeichnis `git-lernen` (`cd` steht für
change directory) und lass Dir den Inhalt anzeigen:

```
$ cd git-lernen
$ ls -a
.  ..  .git  README.md
```

Die Datei `README.md` ist der Inhalt des Repositories. In der versteckten Datei
`.git` steht alles drin, was `git` sich merken muss: welche Dateien werden getrackt,
welche Änderungen gab es wann und durch wen? 

### Code ändern und git darüber informieren: `add` und `commit`

Apropos Änderungen: Was gab es denn in dem Repository bislang für Änderungen?
Die Abfolge dieser Änderungen nennt man auch die *history*. Schauen wir uns
die bisherige *history* an:

```
$ git log
```

![img](img/log.png)

Bis zum gegenwärtigen Zeitpunkt gibt es nur einen einzigen "Commit". Was ist ein *commit*?
Ein *commit* ist ein neuer Eintrag in der *history*: Mit einem *commit* teilst Du
`git` mit, dass es sich Deine Änderungen gegenüber der vorherigen Version offiziell
merken soll. Wenn Du Änderungen `commit`est, musst Du immer eine kurze Mitteilung
hinzufügen, welche die Änderungen beschreibt. Das ist die *commit message*.
Diese *commit message* lautet für den ersten Commit in diesem Repository originellerweise "Initial commit".

Nimm nun eine Änderung am Repository vor. Was das für eine Änderung ist, bleibt
Dir überlassen. Du könntest z.B. eine neue Markdown-Datei erstellen. Nutze einen
Texteditor (z.B. Notepad++) und erstelle eine neue Textdatei (z.B. `meins.md`)
im Verzeichnis `git-lernen`. Ich habe dafür unter Ubuntu den Editor *gedit* genutzt.

![img](img/firstedit.png)

Speichern nicht vergessen. Jetzt schau Dir an, ob `git` von Deinem Schaffen Notiz
genommen hat:

```
$ git status
```

![img](img/status.png)

`git` hat also gemerkt, dass es eine neuen Datei namens `meins.md` gibt. Diese
Datei ist aber noch "untracked", ist also noch nicht Teil des `git` Repositories,
obowhl sie im gleichen Verzeichnis liegt. Das Hinzufügen zum Repository erfolgt
in zwei Schritten: `add` fügt die Datei der sog. "Staging Area" hinzu, `commit`
fügt dann alle Dateien, die sich in der Staging Area befinden, der git history hinzu.
Das Flag `-m` markiert die *commit message*. 

```
$ git add meins.md
$ git commit -m "Datei meins.md hinzugefuegt."
```

Jetzt schauen wir uns nochmal die History an:

`$ git log`

![img](img/add-commit-log.png)


### Letzter Schritt: `push` Deine Änderungen in Dein Repository auf GitLab

Du hast jetzt eine laufende lokale Versionskontrolle. Wenn Du es für sinnvoll
hältst, kannst Du nun Deine Änderungen auf Dein GitLab-Repository schieben (`push`en).

`$ git push origin master`

Du könntest auch nur `git push` verwenden, aber wenn Du mit mehreren Remote-Repositories
und branches arbeitest, empfiehlt es sich, remote und branch explizit anzugeben.

Schaue nun im Browser auf Deinem GitLab-Repository nach, ob Deine Änderungen angekommen sind.

## Recap

So, das war Dein erster vollständiger Durchlauf durch einen sogenannten git workflow:

- Fork: Ein repository auf einer Hosting-Plattform wie GitLab in Deinen namespace kopieren.
- `git clone`: ein Repository von einer Plattform in ein lokales Verzeichnis runterladen.
- `git log`: Die bisherige History inspizieren.
- `git status`: Bislang aufgelaufene Änderungen anzeigen
- `git add .`: Alle Änderungen in die Staging Area verschieben
- `git commit -m "Eine aussagekräftige Beschreibung"`: Alle Änderungen auf der 
Staging Area in das Repository `commit`en
- `git push`: Aktuellen Stand des Repositories auf ein remote Repository schieben

**Aufgabe**: Wiederhole diesen Workflow ein paar Mal, um etwas Routine zu bekommen.
Füge Dateien hinzu oder ändere den Inhalt bereits bestehender Dateien. Schaue Dir
jeweils den Output von `git status` an.

## Markdown

Was hat es eigentlich mit diesem *Markdown* auf sich? Markdown ist eine Auszeichnungssprache
(markup language). Auszeichnungssprachen nutzt man, um Text zu strukturieren und
formatieren. Im Gegensatz zu anderen Auszeichnungssprachen wie `html` (hypertext
markup langauge) ist Markdown aber sehr einfach zu lesen und zu schreiben. Man kann
auf diese Weise aus reinem Text sehr hübsche Dokumente rendern. So wie diese Webseite.

Auf [dieser Seite](https://codingnconcepts.com/markdown/markdown-vs-html/) seht Ihr
Beispiele für die Auszeichnung von Text in Markdown vs. html. Die Einfachheit von
Markdown hat natürlich ihren Preis: man hat weniger Gestaltungsspielraum und man
benötigt eine Software, die wiederum Markdown in html umwandelt: Denn html ist
und bleibt das Fundament für Internetseiten bzw. für Content, der in Browsern angezeigt wird.

Das tolle ist: Hosting-Plattformen wie GitHub und GitLab, aber auch Software wie
`jupyter` und `R` verfügen über eine eingebaute Markdownunterstützung. Wenn Ihr
eine Markdown-Date (`.md`) in GitLab anschaut, wird diese direkt gerendert. Auch
diese Seite hier wurde in Markdown geschrieben und dann von GitHub in html umgewandelt.
Schaut Euch mal den Quelltext dieser Seite an (in Mozilla Firefox z.B. über `Strg + U`). Brrh.

Wir haben ein Pad vorbereitet, in dem wir alle zusammen Markdown kritzeln
können: [Markdown-Sandkasten](https://pad.gwdg.de/0WuatSJ5SdSzFLh96sX6ug?both). Wir
werden die Seite aber nur während des Kurses freigeben. Ihr könnt diesen genialen
Pad-Service aber auch selbst nutzen, indem Ihr Euch unter [https://pad.gwdg.de](https://pad.gwdg.de)
mit Eurem Uni Potsdam Single-Sign-On registriert.

Eine weiteres schönes Markdown-Cheatsheet gibt es [hier](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

**Frage**: Was spricht Eurer Meinung nach dagegen, Pads wie das eben genutzte für die
gemeinschaftliche Softwarenutzung zu verwenden?


## Mit Hilfe von `git` und GitLab zusammen arbeiten

Wir haben es im Rahmen dieser Veranstaltung noch nicht geschafft zu zeigen, wie
man `git` in Verbindung mit GitLab nutzt, um gemeinsam an Projekten zu arbeiten.
Das ist eigentlich nicht so schwer: Irgendwie müsste man die Änderungen, die man
in seinem Fork gemacht hat, wieder zurück in das ursprüngliche Respository bekommen.
Als Eigentümer des ursprünglichen Respositorys möchte man aber nicht unbedingt,
das x-beliebige Menschen ihren Code in das Repository pushen (hier war das ursprüngliche
Respository [https://gitup.uni-potsdam.de/umweltdv/git-lernen.git/umweltdv/git-lernen]).

Dafür gibt es nun zwei Lösungen: Entweder man vertraut bestimmten Menschen, dass
sie keine Mist bauen und räumt ihnen Schreibrechte in dem Repository ein. Das geht
in GitLab unter `Settings -> Members` im linken Sidebar.

![img](img/permissions.png)

Oft möchte man aber gern Entwicklungen Dritter motivieren, ihnen aber dennoch keine
Rechte im eigenen Repository einräumen. Hier hat sich ein wichtiger Mechanismus
der kollaborativen Softwareentwicklung gebildet: in der GitHub-Welt heißt dieser
"Pull-Request", in der GitLab-Welt "Merge Request". Vereinfacht gesagt kann man
einen "Antrag" (Request) an den Eigentümer des Repository stellen, die eigenen
Änderungen hineinzu**merge**n.

Als Beispiel, wie so etwas abläuft, schauen wir uns mal die Pull Requests
des Python-Pakets `wradlib` an, einer Bibliothek zur Verarbeitung von Niederschlagsradardaten:
[https://github.com/wradlib/wradlib/pulls].

Für eine ausführliche Behandlung und vor allem Übung des Themas fehlt uns in dieser
Veranstaltung leider die Zeit. [Hier](https://www.earthdatascience.org/courses/intro-to-earth-data-science/git-github/github-collaboration/)
findet Ihr eine Einführung in das Thema.

 

## Weitere Ressourcen

Es gibt wirklich grandiose Ressourcen, um sich weiter mit dem Thema zu beschäftigen.

- [Git Cheatsheet No 1](https://education.github.com/git-cheat-sheet-education.pdf)
- [Git Cheatsheet No 2](https://www.atlassian.com/dam/jcr:e7e22f25-bba2-4ef1-a197-53f46b6df4a5/SWTM-2088_Atlassian-Git-Cheatsheet.pdf)
- [Git-Kurs No 1](https://www.earthdatascience.org/courses/intro-to-earth-data-science/git-github/)
- [Git-Kurs Software Carpentry](https://swcarpentry.github.io/git-novice/index.html)
- [Git-Book](https://git-scm.com/doc)




