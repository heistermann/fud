---
layout: default
title: Projekt No 1
nav_order: 1
parent: Projekte
---

# Radargestützte Niederschlagsrekonstruktion für das Ereignis im Juli 2021

## Übersicht

Ziel des Projekts ist die Darstellung der Niederschlags für das Hochwasserereignis
im Juli 2021 im Westen Deutschlands. Grundlage dafür ist das deutschlandweite
Radarkomposit des Deutschen Wetterdienstes, welches stündliche Niederschlagshöhen
auf einem 1-km-Gitter für ganz Deutschland bereitstellt. Zusätzlich zu einer
räumlichen Darstellung der Niederschlags soll für ausgewählte Punkte und Flächen
eine Zeitreihe der kumulativen Niederschlagssumme dargestellt werden. Die räumlichen
Darstellungen sollen mit administrativen Grenzen sowie Hauptgewässern ergänzt werden.

## Daten 

- [stündliche Niederschlagsgitter](https://opendata.dwd.de/climate_environment/CDC/grids_germany/hourly/radolan/recent/asc/)
- Grenzen der Bundesländer
- Staatsgrenzen
- Hauptgewässer
- Einzugsgebiet der Ahr


## Ergebnisse

- Kartendarstellung der gesamten Niederschlagshöhe vom 12. bis 19. Juli 2021
- Kartendarstellung der täglichen Niederschlagshöhen vom 12. bis 19. Juli 2021
- Zeitreihen der kumulativen Niederschlagshöhe für
   - für die Gitterzelle mit der höchsten Niederschlaghöhe
   - für das Einzugsgebiet der Ahr (mittlerer Gebietsniederschlag)
   - ...

## Hinweise

- Nunächst die Niederschlagsgitter in einen drei-dimensionalen Array einlesen,
dann aus diesem Array die weiteren Produkte (Akkumulationen, Zeitreihen) generieren.

- ...