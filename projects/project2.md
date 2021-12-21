---
layout: default
title: Hurricane-Zugbahnen
nav_order: 2
parent: Projekte
---

# Analyse historischer Hurricane-Tracks

## Übersicht

Hurricanes gehören zu den Naturereignissen mit dem größten Schadenspotenzial. Dieses Projekt hat zum Ziel, das Hurricane-Risiko in den US-Staaten zu quantifizieren. Um dieses Ziel zu erreichen, ist es Eure Aufgabe, einen nicht-tabellarisch gespeicherten Datensatz zu Hurricane-Zugbahnen einzulesen und in einen räumlichen GIS-Vektordatensatz zu transformieren. Auf Basis simplifizierter Annahmen zu Durchmesser der Hurricanes, Immobilienwerten und Vulnerabilität, werden Sie die mittleren jährlichen Schadenshöhen abschätzen, welche die Basis für die Berechnung von Versicherungsprämien sind. Um die Ergebnisse zu präsentieren, werden Karten und Abbildungen präsentiert. 

## Daten

- HURDAT2: Ladet die neuesten Best Track Data (HURDAT2) für den Atlantik von der Seite: https://www.nhc.noaa.gov/data/ herunter. Der Dateiname sollte wie folgt lauten: hurdat2-1851-2020-052921.txt, wobei der exakte Name je nach Datum variieren könnte. Zusätzlich zu der Datei gibt es eine Beschreibung der Datei als pdf (hurdat2-format-nov2019.pdf). Das Dokument beschreibt das Format der Datei und gibt die Einheiten an, mit denen Windgeschwindigkeiten, Distanzen, etc. angegeben werden. Achtung, vielfach werden hier Knoten, Meilen, etc. angegeben. Stellt sicher, dass ihr Eure Analyse in SI-Einheiten macht, also in m bzw. km, m/s bzw. km/s. Diese Daten einzulesen wird nicht ganz einfach. Aber mit den erlernten Programmierkenntnissen wird das schon klappen. Überführt die Daten in ein Geodatenformat (in R mithilfe der Pakete `sf` oder `sp`). Das Koordinatenreferenzsystem der Daten ist WGS84 (EPSG:3462).

- Grenzen der US-Counties (shapefile) und Zensusdaten: Zusätzlich zu den Hurricane-Daten, liegt das Shapefile UScounties.shp vor. Die Attributtabelle beihaltet die Spalte FIPS. Diese benötigt ihr, um die räumlichen Daten mit den Zensusdaten in der Datei DataSet.xlsx zu verknüpfen. Die Excel-Datei beinhaltet viele Informationen zu den counties. Einer dieser Informationen ist die Spalte: HSG495212 - Median value of owner-occupied housing units, 2008-2012, also der Medians des Hauswertes in den jeweiligen Counties.  

## Ergebnisse

- eine Funktion, welche die HURDAT2-Daten einliest, und die mit zukünftig aktualisierten Daten ebenso funktionieren würde.
- Kartendarstellung der Hurricane-Zugbahnen
- Darstellung der Zeitreihen der Häufigkeit von Hurricanes von 1850 bis heute
- Risikoanalyse mit Kartendarstellung

## Hinweise (für R)

- Verwendet die Funktion readLines um die Daten einzulesen. Extrahiert für jedes gelistete Tiefdruckgebiet die Breiten- und Längegrade der Zugbahn und speichert sie als räumliches sf-Objekt ab (z.B. mit st_linestring(...)). Notiert zudem dabei, ob es sich bei einem der Tiefdruckgebiete auch um ein Hurricane handelt, am besten in einem data.frame. In der vierten Spalte gibt es den Eintrag HU, der diese Klassifizierung vornimmt. Kombiniert dann alle st_linestrings in eine Simple Feature Collection mittels st_sfc(...)). Fügt zum Schluss die Simple feature Collection mit dem data.frame zusammen mit Hilfe der Funktion st_sf().

- Bei der Kombination der Bundesstaaten (die ihr am besten mit der Funktion st_read einlest) und der Zensusdaten sollte euch die Funktion merge helfen.

- Um das Hurricane-Risiko zu berechnen, verwendet folgende Risikogleichung: R = H*V*A, wobei R das Risiko ($/y) ist, H die jährliche Häufigkeit von Hurricanes (1/y), V die Vulnerabilität (keine Einheit) und A die Werte ($), die der Naturgefahr ausgesetzt sind. Um H zu bestimmen, geht wie folgt vor: Nehmt einen charakteristischen Umfang eines Hurricanes an (z.B. 600 km). Erzeugt einen Buffer um die Hurricane Tracks (Achtung, hier wird der Radius benötigt). Berechnet für jedes County, wie häufig es von einem Hurricane getroffen wurde (z.B. st_intersects). Rechnet diese Werte in Häufigkeiten pro Jahr um. Um A zu bestimmen: Hier nehmen wir die Medianwerte der Immobilien im Zensusdatensatz. Um V zu bestimmen: Hier müssen wir kreativ sein, denn hier liegen uns keine Daten vor. Darum treffen wir hier einfach die Annahme, dass ein Hurricane zu Schaden am Haus führt, der 5% seines Wertes entspricht.  

