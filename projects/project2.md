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

- Ladet die neuesten Best Track Data (HURDAT2) für den Atlantik von der Seite: https://www.nhc.noaa.gov/data/ herunter. Der Dateiname sollte wie folgt lauten: hurdat2-1851-2020-052921.txt, wobei der exakte Name je nach Datum variieren könnte. Zusätzlich zu der Datei gibt es eine Beschreibung der Datei als pdf (hurdat2-format-nov2019.pdf). Das Dokument beschreibt das Format der Datei und gibt die Einheiten an, mit denen Windgeschwindigkeiten, Distanzen, etc. angegeben werden. Achtung, vielfach werden hier Knoten, Meilen, etc. angegeben. Stellt sicher, dass ihr Eure Analyse in SI-Einheiten macht, also in m bzw. km, m/s bzw. km/s. Diese Daten einzulesen wird nicht ganz einfach. Überführt die Daten in ein Geodatenformat mithilfe der Pakete `sf` oder `sp`.

- Grenzen der US-Bundesstaaten (shapefile)

## Ergebnisse

- Kartendarstellung der Hurricane-Zugbahnen
- Darstellung der Zeitreihen der Häufigkeit von Hurricanes von 1850 bis heute
- Risikoanalyse mit Kartendarstellung

## Hinweise

