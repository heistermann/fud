---
layout: default
title: Schneehöhen
nav_order: 3
parent: Projekte
---

# Rekonstruktion einer Zeitreihe von Schneehöhen aus Fotos einer Wildtierkamera

## Übersicht

Am Versuchsstandort Marquardt (10 km nördlich von Potsdam) wurde im Winter 2020/21 im Rahmen des Cosmic-Sense-Projektes ein Schneemonitoring betrieben. Dafür dienten Messstäbe, die in regelmäßigen Abständen von einer Wildtierkamera aufgenommen wurden.

Aufgabe ist es, Anhand der Länge der aus dem Schnee ragenden Stäbe auf den Fotos sowie deren Zeitstempel eine Zeitreihe der Schneehöhe zu ermitteln und diese mit den Aufzeichnungen eines Ultraschall-Schneepegels zu vergleichen.

## Daten
Gegeben sind die Fotodaten der Wildtierkamera an Standort 02. An diesem Standort wurden 9 Stäbe beobachtet. Die Auswertung soll nur exemplarisch nur für *einen* Stab an diesem Standort erfolgen.

Die Daten liegen hier:
https://boxup.uni-potsdam.de/s/pbSK8DLTBsNLJqS
Passwort: Umweltdatenverarbeitung

	fotos.zip: Archiv der Fotos
	temp_precip_height.json: Beschreibung der Aufzeichnungen des Ultraschall-Schneepegels
	temp_precip_height.txt : Aufzeichnungen des Ultraschall-Schneepegels

## Ergebnisse
- grafischer Vergleich<sup>1</sup> der Schneehöhe an einem Messstab mit der Zeitreihe aus der Ultraschallmessung<sup>2</sup>

## Hinweise
- Die Auswertung soll halbautomatisch erfolgen. Dafür sollen alle Fotos automatisch eingeladen werden <sup>3</sup> und die Lage der Spitze (einmalig, konstant) und des Fußpunkts des Stabs (variabel mit der Schneehöhe) durch Klicken<sup>4</sup> des Nutzers bestimmt werden. 
- Die Zeitpunkte der Fotoaufnahmen lassen sich aus dem Dateidatum oder (sicherer) deren EXIF-Informationen<sup>5</sup> bestimmen.
- Die installierten Stäbe ragten 50 cm aus dem Boden. 10 cm vor der Spitze befindet sich eine weitere Referenzmarkierung, die ebenso genutzt werden kann.
- Die Erfassung soll vor allem für Phasen starker Änderung erfolgen; in Phasen gleicher Schneehöhe kann zwischen weiter auseinanderliegenden Aufnahmen interpoliert<sup>6</sup> werden.

## Detailtipps für die Umsetzung
- <sup>1</sup> Lektion 2, Darstellung mit ´plot()´
- <sup>2</sup> Lektion 2, ´read.table()´
- <sup>3</sup> Lektion 3, ´file.list()´, Lektion 4, ´for()´
- <sup>4</sup> Funktion ´locator()´
- <sup>5</sup> Package ´exif´
- <sup>6</sup> Funktion ´approx()´