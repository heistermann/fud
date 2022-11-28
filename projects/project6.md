---
layout: default
title: Bodenfeuchte
nav_order: 6
parent: Projekte
---

# Räumliche und zeitliche Bodenfeuchtedynamik im Einzugsgebiet des Wüstebachs

## Übersicht

Das Einzugsgebiet des Wüstebachs liegt in der Eiffel und ist ein sogenanntes
TERENO-Gebiet. [TERENO](https://www.tereno.net) steht für **Ter**restrial **En**vironmental **O**bservatories
und zeichnet Gebiete aus, in denen ein langfristig angelegtes Umweltmonitoring
stattfindet, um Prozesse des globalen Wandels besser zu verstehen. Das Monitoring
im Wüstebachgebiet umfasst u.a. ein dichtes Messnetz von 150 Sensoren (SoilNet),
welche in Tiefen von 5, 20 und 50 cm die Bodenfeuchte erfassen. Auf diese Weise
lässt sich sowohl die horizontale wie die vertikale Heterogenität dieser wichtigen
Größe untersuchen. 


## Daten

- Stündlich SoilNet-Messungen von 2009 bis 2021 (5, 20 und 50 cm Tiefe) an 150 Messpunkten 
(auch "Knoten" genannt). Achtung: Die Daten wurden einer Plausibilitätskontrolle unterzogen.
Unplausible Daten wurden mit dem Wert -1 multipliziert, so dass unplausible Werte einerseits
leicht herausgefiltert werden können, andererseits aber der usprüngliche Messwert rekonstruiert werden kann.
- Beobachtungen der Klimastation im Wüstebachgebiet
- DEM (Raster)
- Einzugsgebietsgrenzen (Shapefile)
- Wüstebach (Shapefile)

Die Zeitreihendaten stehen auf dem [TERENO-Datenportal](https://ddp.tereno.net/ddp/) zur Verfügung,
wurden bereits zur Nutzung in diesem Kurs heruntergeladen, vorprozessiert und unter Box.UP
bereitgestellt ([Link](https://boxup.uni-potsdam.de/s/pbSK8DLTBsNLJqS),Passwort: Umweltdatenverarbeitung).

## Ergebnisse

- Ermittle für jeden Monat von 2009 bis 2021 den prozentualen  Anteil verfügbarer und plausibler Daten.
  Die maximale Zahl der Datenpunkte pro Monat: Stunden pro Monat x 150 (Messknoten) x 3 (Messtiefen).
- Ermittle diejenigen Messpunkte, an denen über den gesamten Zeitraum mindestens 80 Prozent der Daten
vorliegen
- Stelle nun als Zeitreihe das Monatsmittel aller SoilNet-Knoten von 2009 bis 2021 dar: für 5, 20 und 50 cm Tiefe.
  Stelle in einem zusätzlichen Panel der prozentualen Anteil verfügbarer Daten dar.
  sowie als Mittelwert über alle Tiefen.
- Identifiziere die beiden Termin mit der höchsten bzw. niedrigsten Bodenfeuchte. Berechne
  außerdem den Mittelwert für jeden einzelnen Knoten. Stelle diese Ergebnisse als Karten
  dar, in denen an der Position jedes Knotens die entsprechende Bodenfeuchte farbkodiert ist.
  Stelle als Kartenhintergrund dar: Einzugsgebietsgrenzen, Wüstebach, Höhenlinien aus dem
  DEM.

## Hinweise

...