---
layout: default
title: Python einrichten
parent: Die Arbeitsumgebung
nav_order: 1
---

# Meine Python-Arbeitsumgebung

## Installiere Miniconda und Python-Pakete

Die Installation führt uns gleich an die Kommandozeile heran. Zunächst installieren wir Miniconda.
Dies ist, einfach gesagt, ein Paketmanager für Python. 

1. Installiere Miniconda. Folge dazu den Anleitungen unter [diesem Link](https://conda.io/miniconda.html).

2. Öffne nun ein Terminalfenster (siehe [hier](https://www.digitalcitizen.life/open-windows-terminal/), wie das für Windows-Nutzer*innen geht).
   Nun sind zwei Befehle auszuführen, die `miniconda` sagen, woher es bevorzugt Pakete beziehen soll.
   
   `$ conda config --add channels conda-forge`
   
   `conda config --set channel_priority strict`

3. Nun lernen wir ein geniales Feature von Miniconda kennen: die sog. "`conda` environments".
   Mit einem Befehl kann man eine neue "Umgebung" (environment) erstellen, ohne dabei befürchten zu müssen, dass
   die bisherige Umgebung dadurch beschäfigt wird. Dies mag jetzt erstmal abstrakt klingen...aber es ist durchaus nicht ungewöhnlich,
   dass die Installation eines Pakets die Funktionalität anderer Pakete beschäfigt. Aber genug jetzt, wir erstellen einfach
   eine neue Umgebung names `umweltdv`:
   
   `$ conda create --name umweltdv python=3.9`

5. Immer, wenn Ihr mit dieser neuen Umgebung etwas anstellen wollt, müsst Ihr sie über das Terminal aktivieren. 
   Vielleicht wird dies der häufigste Befehl, den Ihr in diesem Kurs ausführen werdet...
   
   `$ conda activate umweltdv`

6. Jetzt installieren wir all die Pakete, die wir (voraussichtlich) in diesem Kurs brauchen werden... in einem Rutsch:

   `(umweltdv) $ conda install numpy scipy pandas matplotlib notebook h5py netCDF4 geopandas jupyter_contrib_nbextensions rasterio`
   
   Was das alle für Pakete sind, werden wir Stück für Stück ergründen. Für den Augenblick freuen wir uns, was wir schon alles geleistet haben. 

## Hole Dir die Kursmaterialien auf Deinen Rechner

Wir haben alles, was Ihr für diesen Kurs an Material braucht, in einem zentralen Repository abgelegt. Dies könntet Ihr jetzt auch runterladen, 
aber wir wissen seid unserem letzten Termin etwas besseres: Wir klonen das Repository.

Ihr könnt das Repository schonmal anschauen, wenn Ihr Euch bei https://gitup.uni-potsdam.de einloggt und dann auf 
https://gitup.uni-potsdam.de/heisterm/umweltdv geht.

Überlegt Euch nun, wo Ihr das Repository lokal auf Eurem Rechner speichern möchtet. Öffnet dann in diesem Verzeichnis ein Terminalfenster und dann:

`$ git clone https://gitup.uni-potsdam.de/heisterm/umweltdv.git`

Ihr werdet beim Klonen nach Nutzernamen und Kennwort gefragt. Benutzt Euren UP-Account (Nutzername ohne `@uni-potsdam.de`) 


## Starte `jupyter` und los geht's...

Wir gelangen nun in die heiße Vorbereitungsphase. In Kürze werden wir Eure Arbeitsumgebung starten...aufregend.

Wenn nicht schon geschehen, öffnet im geklonten Arbeitverzeichnis `umweltdv` ein Terminalfester.
Nun aktiviert Eure `conda`-Umgebung `umweltdv` und startet `jupyter`. `jupyter`?? Das ist das Werkzeug, in dem wir 
Python-Code schreiben und ausführen werden. Ein sehr mächtiges Werkzeug, das Ihr in den kommenden Wochen ausführlich
kennenlernen werdet. Nun also:

```
$ conda activate umweltdv
(umweltdv)$ jupyter notebook
```

Der letzte Befehl sollte ein Browserfenster öffnen, in welchem Ihr die Verzeichnisstruktur im Verzeichnis `umweltdv` abgebildet seht.

![1stjupyter](img/1stjupyter.png)

 
