---
layout: default
title: Klimaprojektionen
nav_order: 4
parent: Projekte
---

# Analyse und Darstellung von Klimaprojektionen (Niederschlag) für Potsdam

## Übersicht

Dynamische Regionale Klimamodelle (RCMs) simulieren die physikalischen Prozesse der 
Atmosphäre für einen begrenzten räumlichen Ausschnitt. Sie werden zum einen dafür genutzt, 
um das Klima (nicht das Wetter!) der jüngeren Vergangenheit nachzubilden.
Zum anderen kann man mit ihnen, basierend auf Emissionszenarien, das Klima der Zukunft simulieren. 
 
In diesem Projekt soll für den Standort Potsdam (Telegrafenberg) folgendes analysiert werden:
 
a) die Güte der Niederschlagssimulationen des vom Max-Planck Institut betriebenen RCM 'REMO2009' im Vergleich zu den Messdaten (1971-2000)

b) die Änderungen des Niederschlags, wie sie von 'REMO2009' unter Berücksichtigung des RCP4.5 Szenarios (moderate Emissionsentwicklung) für 2071-2100 projiziert werden


## Daten

- Niederschlagsbeobachtungen am Standort Potsdam-Telegrafenberg (DWD)
- Niederschlagssimulation für 1971-2000, MPI-REMO2009 0.11° räumliche Auflösung (Griddaten, NetCDF)
- Niederschlagsprojektion für 2071-2100 (RCP4.5), MPI-REMO2015 0.11° räumliche Auflösung (Griddaten, NetCDF)

Die Stationsdaten (.txt-Datei) sowie die Griddaten der Klimasimulationen (.nc-Dateien) 
stehen auf Box.UP zu Verfügung ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj), Passwort: umweltdatenverarbeitung).

**Beachte:** Die RCM Daten werden immer in Blöcken für fünf Jahre bereitgestellt. 
Pro Klimaperiode (30 Jahre) müsstest du also auf sechs nc-Dateien zugreifen...
Aufgrund der Größe der nc-Dateien (1GB pro Datei) haben wir diese aber nur für die
jeweils ersten fünf Jahre der beiden Zeiträume (also 1971-1975 und 2071-2075)
auf Box.UP bereitgestellt. Die Daten für die übrigen Jahre (1976-2000 und 2076-2100)
haben wir bereits in Tabellenform gegossen. Du findest die Tabellen (csv) für diejenige
Gridzelle, in der sich die Messstation Potsdam-Telegrafenberg befindet, ebenfalls unter dem o.g. Link.

Die Originaldaten (nc-Dateien) der RCMs wurden bei [Copernicus](https://cds.climate.copernicus.eu/cdsapp#!/dataset/projections-cordex-domains-single-levels)
heruntergeladen. 

## Ergebnisse

**zu a)**

- grafische Darstellung der mittleren Monatsniederschläge (gemessen und simuliert)
- grafische Darstellung der Abweichung der simulierten mittleren Monatsniederschläge im Vergleich zu den Messdaten
- Mittlere jährliche Häufigkeit von Tagen mit Niederschlagshöhen >= 30 mm (gemessen vs. simuliert)

**zu b)**

- Klimaänderungssignal (in %) im mittleren monatlichen Niederschlag
- Änderung der mittleren jährlichen Häufigkeit von Tagen mit Niederschlagshöhen >= 30 mm
- Probiere es gerne auch mit größeren Schwellenwerten als 30 mm/d 


## Hinweise für R

- Einlesen der Stationsdaten (siehe Lektion 2)
- Einlesen und Zugreifen auf die Klimasimulationsdaten für die jeweils ersten fünf Jahre (NetCDF, siehe Lektion 5)
- Erstellen von Dataframes für die Klimasimulationen (Lektion3)
- Anbinden der Tabellen aus den csv-Dateien (Daten für die übrigen je 25 Jahre) an die erstellten Dataframes (Lektion 2).
- Berechnung der Monatsmittel (Funktion aggregate(); Lektion 2)
- Berechnung der Anzahl an Tagen mit Niederschlägen größergleich Schwellenwert (z.B. über regelbasierte Teilmengen; Lektion 2)
- Berechnung der Abweichungen und Änderungen
- Visualiersung der Ergebnisse (Lektionen 2 und 3)

- Die größte Herausforderung im Zusammenhang mit den nc-Dateien ist die Identifizierung der Gridzelle im RCM, in der die Station Telegrafenberg verortet ist. Diese Gridzelle lässt sich mittels einer 'nearest neighbour' Suche identifizieren. Das folgende Scriptbeispiel für R zeigt den Ablauf: 
- Die Koordinaten der Klimastation Telegrafenberg lauten: 13.062 (longitude); 52.382 (latitude)

   ```
	% Beispiel in R
	% -------------
	library(ncdf4)
	
	rcm <- nc_open(filename)
	lon <- ncvar_get(rcm, "lon")
	lat <- ncvar_get(rcm, "lat")
	vlon <- as.vector(lon)
	vlat <- as.vector(lat)
	
	lonlat.rcm <- data.frame(lon = vlon, lat = vlat)
	lonlat.obs <- c(13.062, 52.382)
	
	% Nearest Neighbour Suche. Ergebnis: Vektor 'nnb' - Reihe (erster Eintrag) und Spalte (zweiter Eintrag) in der Koordinatenmatrix des RCM
	nnb <- c()
	
	% 1. Berechnung der Distanz zwischen der Stationskoordinate und allen Gridzellenmittelpunkten (mittels Pythagoras)
	d <- sqrt( (lonlat.rcm[,1] - lonlat.obs[1])^2 + (lonlat.rcm[,2] - lonlat.obs[2])^2 )
	
	% 2. Identifiziere die Position mit dem geringsten räumlichen Abstand
	nearnb <- match(min(d), d)
	ifelse (nearnb %% nrows != 0, nnb[1] <- nearnb %% nrows, nnb[2] <- nearnb/nrows)
	nnb[2] <- ceiling(nearnb / nrows)
	
	% Mittels den Einträgen in 'nnb' lässt sich dann auf die entsprechende Zelle in der netCDF-Datei zugreifen.
	p.data <- ncvar_get(rcm, "pr", start = c(nnb[1], nnb[2], 1), count = c(1,1,-1))
	
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ 
	% ++++ Überprüfung, ob die richtige Position gefunden wurde ++++++
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++
	library(maptools)	# Packet muss ggf. vorher installiert werden
	data(wrld_simpl) 	# Einladen einer politischen Weltkarte
	wrl <- as(wrld_simpl,"SpatialLines") 
	
	% Plot der Gridzellen-Mittelpunkte inkl. der Ländergrenzen und des Telegrafenbergs
	plot(lonlat.rcm, cex=.4, pch=3,col="grey", asp=1,xlim=c(10,15), ylim=c(50,55))
    lines(wrl)
    points(lonlat.obs[1], lonlat.obs[2], pch=16, col="red", cex=.6)
    title(main="Location of Station in GCM-grid")
    legend("bottomleft", c("GCM-grid","Obs-station"), pch=c(3,16), col=c("grey","red"), bg="white")
	
	% Hinzufügen der identifizierten "nearest neighbour" Gridzelle
	lon.cell <- ncvar_get(rcm, "lon", start=c(nnb[1],nnb[2]), count=c(1,1))
	lat.cell <- ncvar_get(rcm, "lat", start=c(nnb[1],nnb[2]), count=c(1,1))
	points(lon.cell, lat.cell, cex=.4, pch=3,col="green")
	legend("bottomright", "nnb", pch=3, col="green", bg="white")
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++
	
   ```

## Hinweise für Python

### Tabellendaten laden

Alle Tabellendaten können wir gehabt mit `pandas.read_csv` geladen werden (Lektion 2).
Denk dran, einen Index aus Datetimeobjeken zu erstellen.

### Simulationsdaten für Potsdam aus den NetCDF-Dateien extrahieren

Öffne zunächst die NetCDF-Datei für die Referenzperiode und schau Dir die Datenstruktur
an (siehe Lektion 5). Die Herausforderung ist hier, die Indizes der Gitterzelle
zu identifizieren, in der die Station Potsdam liegt. Dafür nutzt Du die Nearest Neighbour
Methode, welche durch `scipy.spatial.KDTree` bereitgestellt wird. Dafür benötigst Du
die lat/lon-Koordinaten jedes Gitterpunktes. Hierzu müssen wir noch etwas erklären:

In den NetCDF-Dateien gibt es die Dimensionen `rlon` und `rlat`. Das sind die
Koordinaten in einem sog. rotierten Bezugssystem...das ist etwas unangenehm zu
behandeln. Zum Glück enthalten die Dateien aber auch die Eckpunkte für jede
Gitterzelle (Variablen `lon_vertices` und `lat_vertices`). Diese haben die Shapes
`(412, 424, 4)`. Wir erzeugen uns die Mittelpunkte einer jeden Gitterzelle, indem
wir über die Koordinaten der Eckpunkte mitteln (letzte Dimension der Vertizes).
Verwirrend? Nee, ganz einfach:

```
lons = refnc["lon_vertices"][:].mean(axis=2)
lats = refnc["lat_vertices"][:].mean(axis=2)
```
Jetzt hast Du lon und lat als je einen 2-D Array mit Shape `(412,424)`. Die `KDTree`-Funktion
braucht als Input aber einen 2-D Array mit dem Shape `(N,2)`, wobei `N` die Zahl
der Gitterpunkte ist. Die x-Koordinaten (lon) stehen in der ersten Spalte, die y-Koordinaten (lat)
in der zweiten. Wir wandeln `lons` und `lats` in einen solchen Array wie folgt um

```
import numpy as np
xy = np.c_[lons.ravel(), lats.ravel()]
```

Wenn Du Dir `xy` nun also als zweispaltigen Array mit den Gitterpunkten vorstellst,
gibt Dir `KDTree` den Index `ii` des Gitterpunkts zurück, der dem Telegrafenberg
(Lon: 13.062, Lat: 52.382) am Nächsten liegt:

```
from scipy.spatial import KDTree
tree = KDTree(xy)
dd, ii = tree.query([[13.062, 52.382]], k=1)
```

Wenn Du nun den Array mit den Niederschlägen mit Hilfe von `reshape` 
in die Form `(Nt, N)` bringst (`Nt` stehe für die Zahl der Zeitschritte),
kannst Du die Niederschlagszeitreihe in Potsdam extrahieren. Aber Achtung:
Die täglichen Niederschläge sind in der Einheit `g/m2/s` angegeben (SI-Einheit).
Das musst Du noch in `mm/d` umrechnen.

Nun musst Du nur noch die Datetime-Objekte für Deine Zeitreihe basteln. Schau
Dir dafür die Vertiefung der Lektion 5 nochmal an und untersuche die nc-Variable `time` genau.

Nun kannst Du Dir aus dem entstandenen Datetime-Array und dem Niederschlags-Array
Deinen DataFrame für die ersten fünf Jahr der Referenzperiode basteln.

Für die NetCDF-Datei mit der Klimaprojektion machst Du es genau so.

### Auswahl der richtigen Gitterzelle verifizieren

Um visuell zu bestätigen, dass Du die richtige Gitterzelle ausgewählt hast, gibt
es viele Wege. Wie wäre es, wenn Du einfach das Shapefile mit den Bundesländergrenzen
verwendest (Lektion 6)?

```
import geopandas
bundeslaender = gpd.read_file(".../VG250_LAN.shp")
bundeslaender = bundeslaender.to_crs("EPSG:4326")

fig, ax = plt.subplots(1,1)
ax.set_aspect("equal")
plt.plot(lons, lats, "k+", ms=1, zorder=0)
bundeslaender.plot(ax=ax, facecolor="None", edgecolor="blue")
plt.plot(lons.ravel()[ii], lats.ravel()[ii], "ro", ms=10, mfc="None", mec="red")
plt.xlim(11,14)
plt.ylim(51, 54)
```

### Zu guter Letzt

Du musst noch die DataFrames, die Du aus den NetCDF-Daten gewonnen hast,
mit den DataFrames aus den Tabellendaten verknüpfen (Stichwort: `append`).

Nun hast Du drei DataFrames mit täglichen Niederschlägen: Beobachtungen (1971-2000),
Simulation der Referenzperiode (1971-2000) und Klimaprojektion (2071-2100). Den Rest schaffst
Du spielend - lass Dich dafür von Lektion 2 inspirieren.
