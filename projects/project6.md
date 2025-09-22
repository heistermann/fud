---
layout: default
title: Bodenfeuchte
nav_order: 6
parent: Projekte
---

# Räumliche und zeitliche Bodenfeuchtedynamik im Einzugsgebiet des Wüstebachs

## Übersicht

Das Einzugsgebiet des Wüstebachs liegt in der Eifel und ist ein sogenanntes
TERENO-Gebiet. [TERENO](https://www.tereno.net) steht für **Ter**restrial **En**vironmental **O**bservatories
und zeichnet Gebiete aus, in denen ein langfristig angelegtes Umweltmonitoring
stattfindet, um Prozesse des globalen Wandels zu untersuchen. Das Monitoring
im Wüstebachgebiet umfasst u.a. ein dichtes Messnetz von 150 Sensoren (SoilNet),
welche in Tiefen von 5, 20 und 50 cm die Bodenfeuchte erfassen. Auf diese Weise
lässt sich sowohl die horizontale wie die vertikale Heterogenität dieser wichtigen
Größe untersuchen. 


## Daten

- Stündliche SoilNet-Messungen des volumetrischen Bodenwassergehalts von 2009 bis 2021 (5, 20 und 50 cm Tiefe) an 150 Messpunkten 
(auch "Knoten" genannt). *Achtung:* Die Daten wurden einer Plausibilitätskontrolle unterzogen.
Unplausible Daten wurden mit dem Wert -1 multipliziert, so dass unplausible Werte einerseits
leicht herausgefiltert werden können, andererseits aber der usprüngliche Messwert rekonstruiert werden kann.
- DEM (Raster)
- Einzugsgebietsgrenzen (Shapefile)
- Wüstebach (Shapefile)

Die Zeitreihendaten stehen auf dem [TERENO-Datenportal](https://ddp.tereno.net/ddp/) zur Verfügung,
wurden aber zur Nutzung in diesem Kurs bereits heruntergeladen, vorprozessiert und unter Box.UP
bereitgestellt ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj), Passwort: umweltdatenverarbeitung).

## Ergebnisse

- Ermittle für jeden Monat und jede Messtiefe von 2009 bis 2021 den prozentualen  Anteil verfügbarer und plausibler Daten. Stelle dies als Zeitreihendiagramm dar.
- Ermittle für jeden Messknoten und für jede Tiefe den Anteil verfügbarer und plausibler Daten über den *gesamten Messzeitraum*. Ermittle auch den Mittelwert dieses Anteils über alle drei Messtiefen. Sortiere die Knoten absteigend nach diesem Mittelwert. Wieviele der 150 Knoten haben eine mittlere Verfügbarkeit von über 75 Prozent?
- Stelle nun den monatlichen Mittelwert für die Knoten mit über 75 % Verfügbarkeit als Zeitreihe von 2009 bis 2021 dar (5, 20 und 50 cm Tiefe). Füge darunter einen zusätzlichen Subplot mit dem prozentualen Anteil verfügbarer Daten über die Zeit dar. Erörtere kurz die Bodenfeuchtedynamik in den drei Tiefen, auch unter Berücksichtigung der Datenverfügbarkeit.
- Berechne nun die mittlere Bodenfeuchte für jeden Knoten und jede Tiefe für einen selbst gewählten Zeitraum (analog zum vorherigen Schritt nur für Knoten mit min. 75% Verfügbarkeit im gewählten Zeitraum). Stelle diese berechnete mittlere Bodenfeuchte für alle Knoten als Karte für jede Tiefe
  dar, in der an der Position jedes Knotens die entsprechende Bodenfeuchte farbkodiert ist. Nutze als Kartenhintergrund die Einzugsgebietsgrenzen und den Verlauf des Wüstebachs, optional auch die Höhenlinien aus dem DEM. Achte darauf, für jede der drei Karten die gleiche Farbskalierung zu nutzen. Erörtere kurz die räumliche Verteilung.

## Hinweise

### R

- Die SoilNet-Daten liegen in einer zip-Datei. Anstatt dieses manuell auszupacken, könnt Ihr die Daten auch direkt aus der zip-Datei lesen. Nehmen wir an, in der zip-Datei `the.zip` liegt eine Datei `the.csv`, dann geht das so:
`read.table(unz("the.zip", "the.csv"), header=TRUE)`

- Die Handhabung der Daten der verschiedenen Tiefen ist einfacher, wenn Ihr diese in *einem* Dataframe zusammenfasst. Ihr könnt dies durch spaltenweises Zusammenfügen erreichen, praktischer ist aber das zeilenweise aneinanderhängen und dabei eine Spalte für die Messtiefe anzufügen. Dafür müsst Ihr beachten, dass die Spaltennamen in den Dateien der unterschiedlichen Tiefen sind auch unterschiedlich sind. Sie können mit `names(df)` angepasst werden, dann klappt danach auch das aneinanderhängen. 

- Ihr könnt gültige Werte (d.h. Werte, die nicht `NA` sind) in einem Vektor `x` mit Hilfe von `sum(!is.na(x))` zählen. Mit `apply()` lässt sich das dann auch spalten- oder zeilenweise auf Dataframes anwenden.

- Während sich `apply()` eignet, um spalten- oder zeilenweise zu aggregieren, kann `aggregate()` dies nur zeilweise, aber dafür lässt sich dabei nach unterschiedlichen Kriterien gruppieren.

- Den Monat einer Datumsangabe kann man wie in Lektion "Mastering Geodata" erklärt extrahieren. `floor_date()`, auch aus dem Paket `lubridate` könnte weiterhin nützlich sein, um ein Datum auf einen vollen Monatsbeginn abzurunden.

- Die Koordinaten aus einer Tabelle lassen sich in ein Geoobjekt mit `st_as_sf()` (Paket `sf`) umwandeln. Wenn diese vorher mit Spalten zu den Bodenfeuchtewerten ergänzt werden (z.B. mit `merge()`), lassen sich mit dem Paket `tmap` (siehe Lektion "Mastering Geodata") Karten erzeugen. Die Farbe der Punkte regelt man in diesem Fall über
````
tm_shape(dasPunkteObjekt") +   
   tm_symbols(col = "SpaltenameBodenfeuchte", breaks = breaks)
````
Steht statt "SpaltenameBodenfeuchte" ein Vektor mit mehreren Spaltennamen, wird automatisch ein Multipanel-Plot erzeugt. Wenn Ihr `breaks` vorher als Grenzen für die Farblegende definiert, ist diese auch in den verschiedenen Panels konsistent.

- `rasterToContour` aus dem Paket `raster` erzeugt Höhenlinien aus einer Rasterkarte - nützlich, wenn man das DEM lieber etwas dezenter darstellen möchte.

### Python

- Die SoilNet-Daten liegen in einer zip-Datei. Anstatt dieses manuell auszupacken, könnt Ihr die Daten auch direkt aus der zip-Datei lesen. Dazu gibt es das Paket `zipfile`. Nehmen wir an in der zip-Datei `the.zip` liegt eine Datei `the.csv`, dann:

```
import zipfile
zipf = zipfile.ZipFile("the.zip", "r")
f = zipf.open("the.csv")
df = pd.read_csv(f, ...)
f.close()
```
- Ihr könnt gültige Werte  (Werte, die nicht `NaN` oder `NA` sind) in einem `DataFrame` mit Hilfe der Methode `count` zählen. `count` lässt sich auch entlang von `axis` ausführen und mit `resample` kombinieren.

```
# Zählt gültige Werte für jede Spalte
df.count()
# Zählt gültige Werte für jede Zeile
df.count(axis=1)
# Zählt gültige Werte für jede Spalte und jeden Monat (setzt datetime-Index voraus)
df.resample("1M").count()
```

- Achtung, die Spaltenbezeichner sind für die Daten aus unterschiedlichen Tiefen auch unterschiedlich. Wenn der DataFrame df5 die Daten aus der Tiefe 5 cm enthält und der DataFrame df20 aus 20 cm Tiefe, könnt Ihr Spaltennamen aus df5 also nicht zur Auswahl von Daten aus df20 verwenden. Wenn für alle drei Tiefen die jeweils gleichen Spalten auswählen wollt, empfiehlt sich daher die Nutzung von `.iloc`. Beispiel: `df.iloc[:,[1,3]]` wählt aus `df` alle Zeilen und die 2 und 3 Spalte aus. 

- Multiplanel-Plots, also Diagrammme mit mehreren Subplots, erstellt man am einfachsten mit `plt.subplots`. Die `axes` (subplots), welche dadurch zurückgegeben werden, kann man mit `plt.sca()` aktivieren.

```
fig, ax = plt.subplots(2,1,figsize=(8,5))
ax = ax.ravel()
# Aktiviere erste Zeile:
plt.sca(ax[0])
plt.plot(...)
# Aktiviere zweite Zeile:
plt.sca(ax[1])
plt.plot(...)
```

- Mit Hilfe der Funktion `plt.scatter` könnt Ihr Punkte entsprechend eines Wertes einfärben. Beispiel: Ihr habt eine DataFrame df mit den Spalten `x`, `y` und `wert` (`x` und `y` seien kartesische Koordinaten). Dann färbt Ihr die Punkte wie folgt ein: `plt.scatter(df.x, df.y, c=df.wert)`. Mit dem Schlüsselwort `cmap` könnt Ihr noch eine Colormap auswählen. Mit den Schlüsselwörtern `vmin` und `vmax` könnt Ihr eine einheitliche Farbskalierung erzwingen - das ist wichtig für die Vergleichbarkeit von Teilabbildungen, die die gleiche Variable ,mit unterschiedlichen Wertebereichen zeigen.
