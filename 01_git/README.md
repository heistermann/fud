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

**Klingt relevant, aber auch ganz schön trocken und obendrein noch ziemlich techniklastig.
Ist das wirklich so wichtig? Wir wollen doch eigentlich R oder Python lernen...**

- Wir Dozent*innen in diesem Kurs haben das Marterial gemeinsam mit `git` entwickelt
und wir stellen Euch dieses Material in Form eine `git`-Repositories zur Verfügung.
Allein dafür benötigt Ihr minimale Kenntnisse in `git`.
- Wir möchten Euch ermuntern, von Beginn an Eure Arbeit in diesem Kurs mit `git`
zu begleiten. Versionskontrolle ist kein Hexenwerk, aber wie so vieles nutzt man
es nur, wenn man eine gewisse Routine darin entwickelt hat. Und diese Routine 
erzielt man nur durch regelmäßige Nutzung. Wir wollen diesen Feedbackmechanismus
im positiven Sinne nutzen und starten darum gleich zu Beginn mit `git`.
- Die Nutzung von `git`, zusammen mit cloud-basierten Hosting-Plattformen wie GitLab oder
GitHub, erlaubt Euch nicht nur, Euren Arbeitsfortschritt zu strukturieren und jederzeit
zu einem früheren Zustand zurückzuschalten. Ihr könnt es auch einfach als eine
komfortable Methode des Backups verstehen... 
- [GitHub](https://github.com) ist die erfolgreichste Plattform für kollaborative Softwareentwicklung.
Dort könnt Ihr nicht nur Eure Projekte veröffentlichen und teilen, sondern auch auf einfache Art und
Weise statische Webseiten entwickeln und hosten. Die Webseite, auf der Ihr Euch
gerade befindet, ist ein Beispiel dafür.  
  


## Vorbereitung: Git installieren und einrichten

Auf dieser Seite findet Ihr Anleitungen für die Installation von `git` unter
Linux, Mac und Windows: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git.

```
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```



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

