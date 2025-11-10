---
layout: default
title: Schnee
nav_order: 7
parent: Projekte
---

# Analyse des Wasseräquivalents der Schneedecke von 1951 bis heute anhand von ERA5-Land

## Übersicht

ERA5 ist eine der weltweit bedeutendsten Reanalysen des Klimasystems. Eine Reanalyse kombiniert Modell und Beobachtungen, um so eine möglichste konsistente Rekonstruktion unterschiedlicher Variablen in Atmosphäre, Landoberfläche und Ozeanen zu erstellen. ERA5-Land fokussiert dabei auf die Landoberfläche.

Auch wenn das Ziel einer Reanalyse die konsistente Abbildung von Beobachtung und Physik anstrebt, bedeutet dies nicht, dass die Reanalyse alle Beobachtungen zuverlässig reproduziert. Dies ist aus einer Vielzahl von Gründen auch gar nicht das Ziel bzw. auch gar nicht möglich. Umso wichtiger ist es, sich für die jeweils interessierenden Klimavariablen einer Region ein Bild über die Unsicherheit der Reanalyse zu verschaffen.

Im vorliegenden Projekt setzt Ihr Euch mit der ERA5-Land Rekonstruktion des Wasseräquivalents der Schneedecke (engl. snow water equivalent, SWE, in mm) auseinander. Dabei sollte Ihr einerseits die langfristige Änderung des SWE gemäß ERA5-Land für unterschiedliche Raumausschnitte ermitteln. Andererseits sollte Ihr das SWE aus ERA5-Land mit Beobachtungen des Deutschen Wetterdienstes (DWD) vergleichen und kurz die Ergebnisse diskutieren. 


## Daten

- NetCDF-Datei `era5-swe-airt.nc` enthält SWE und Lufttemperatur in 2m Höhe für Europa (2951 bis heute).
- Auf dem Opendata-Repository des DWD werden [tägliche Messungen des SWE](https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/daily/water_equiv/) für ein Set von über 1000 Stationen in Deutschland bereitgestellt. Die Daten befinden sich im Unterverzeichnis `dwd-wa`. Dort liegen auch eine Stationsübersicht (`Wa_Tageswerte_Beschreibung_Stationen.txt`) sowie die Beschreibung der Daten (`BESCHREIBUNG_WA.pdf`) bereit. Die Schwierigkeit mit diesem Datensatz: Messungen des SWE (Parameter `WAAS_6`) sind nur ab einer Mindesthöhe der Schneedecke (typischweise 5 cm) sinnvoll durchführbar. Falls keine Messungen durchgeführt wurden, wird ein NoData-Wert angegeben. NoData kann in diesem Fall also zwei Dinge bedeuten: (a) Messungen wurden im Zeitraum grundsätzlich nicht durchgeführt; (b) Messungen wurden aufgrund zu geringmächtiger Schneedecke nicht durchgeführt. Dieser Sachverhalt erschwert die Abschätzung des mittleren SWE über längere Zeiträume (z.B. einen Monat). Um das obenstehende Problem zu beheben, sollt Ihr ferner die Beobachtungen der Schneehöhe (Parameter `SH_TAG`) berücksichtigen, welche durchgehend verfügbar sind.  

## Ergebnisse

t.b.c.

## Hinweise

### Python

t.b.c.