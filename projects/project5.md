---
layout: default
title: Grundwasser (R+Python)
nav_order: 5
parent: Projekte
---

# Grundwassersituation in Potsdam und Umgebung

## Übersicht

Seit 2018 gab es eine ungewöhnliche Häufung heißer und trockener Sommer in Brandenburg,
welche das Grundwasser einem starken Stress ausgesetzt haben. Teils konnte die 
winterliche Grundwasserneubildung die Verluste nicht ausgleichen.

In diesem Projekt untersuchen wir mit Hilfe von Grundwasserbeobachtungen an amtlichen Pegeln 
des LfU Brandenburg die Grundwassserdynamik in Potsdam und Umgebung im Laufe der letzten 20 Jahre.


## Daten

Die Daten stehen auf der [Auskunftplattform Wasser](https://apw.brandenburg.de) zur Verfügung, wurden jedoch bereits
zur Nutzung in diesem Kurs heruntergeladen und unter Box.UP bereitgestellt ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj), Passwort: umweltdatenverarbeitung).
Da der Prozess des Downloads auf der APW etwas mühsam ist, wurden der Datensatz seit 2021 nicht mehr aktualisiert. Die Daten wurden
nach dem Download auch schon einer gewissen Bereinigung unterzogen. Die ursprünglich heruntergeladenen Daten
findest Du unter `gw/obs` als zip-Dateien. Du arbeitest aber mit den GW-Daten unter `gw/obs_bereinigt`.

- `pegel.csv`: Übersichtstabelle der Pegel mit `id`, `name`, `stockwerk` (GW-Stockwerk), `lat` (geogr. Breite), `lon` (geogr. Länge).
  Mit `id` kannst Du den Pegel mit den Dateinamen unter `gw/obs_bereinigt` verknüpfen.
- `gw/obs_bereinigt` enthält unter `metadata` zusätzliche Metadaten der Pegel. Von diesen benötiogst Du die Geländehöhe sowie den
  Beginn und das Ende der Zeitreihe. Das Verzeichnis `data` enthält die eigentlichen Zeitreihen der Pegelstände (Achtung beim Dezimaltrennzeichen).
- `geo`: Unterschiedliche Geodatensätze aus OpenStreetMaps sowie ein DEM. 



## Ergebnisse

- Erstelle eine Tabelle bzw. einen DataFrame, der folgende Informationen enthält: Pegel-ID, Länge, Breite, Geländehöhe, Startdatum der Zeitreihe, Enddatum der Zeitreihe. Welches sind die zehn Pegel mit den längsten Messreihen?

- Ermittle für jeden Pegel (soweit möglich) den Mittelwert von 2001-2010, den Mittelwert von 2020-2021 (einschließlich) sowie die Differenz beider Werte (im Folgenden `delta` genannt).
  
- Erstelle einen Multipanelplot mit einem Subplot pro Pegel (Empfehlung: 9x6). Jeder Subplot soll folgende Elemente und Informationen enthalten:
   - Zeitreihe des Pegelstandes relativ zum Mittelwert 2001-2010
   - Wie zuvor, aber geglättet mit Moving Window der Länge 1000 Tage
   - `delta` als Text
   - Name des Pegels (ggf. gekürzt) als Text

- Erstelle eine Karte des Gebiets, in welcher die Position jedes Pegels dargestellt und der Wert für `delta`
farblich codiert ist. Stelle dazu folgende Klassen dar: `delta` < -75 cm, -75 <= `delta` < -50, -50 <= `delta` < -25, -25 <= `delta`.
Nutze als Kartenhintergrund folgende Layer:

   - Gewässerflächen (`geo/gewaesser.shp`)
   - Gemeindegrenzen (`geo/gemeinden.shp`)
   - Siedlungsflächen (`geo/urban.shp`)
   - Höhenlinien, abgeleitet aus DEM (`geo/dem25_epsg25832.tif`)

- Stelle `delta` in Beziehung zur mittleren Höhe des Grundwasserstandes. Lässt sich ein Zusammenhang erkennen und ggf. erklären? Welcher Pegel fällt bzgl. des mittleren Pegelstandes besonders aus der Reihe? Warum? Nutze ggf. den mittleren Wasserstand (MW) des [Havelpegels in Potsdam](https://www.pegelonline.wsv.de/gast/stammdaten?pegelnr=580412) für eine Einordnung der Pegelhöhen.

## Hinweise


### Python

- Ihr müsste eine Abbildung mit sehr viele Subplots erstellen. Dazu könnt Ihr folgenden Ansatz verwenden:

```
# sharex und sharey sorgen dafür, dass die x- und y-Achsen automatisch gleich skaliert werden
# (zwecks Vergleichbarkeit)
fig, ax = plt.subplots(9,6, figsize=(12,15), sharex=True, sharey=True)
ax = ax.ravel(order="F")
# locs könnte der Name Eures DateFrames mit den Stammdaten der Pegel sein
# locs.index die Pegel-ID...
for i, key in enumerate(locs.index):
   # Aktiviert den Subplot i
   plt.sca(ax[i])
   # und nun geht das Geplotte los
   ...
# Macht die Zeitachse nochmal schick
fig.autofmt_xdate()
```

- Das Plotten von Höhenlinien in der Karte ist nicht ganz einfach. Darum hier die
Anleitung, wie das DEM zu lesen und daraus die Höhenlinien zu generieren sind:

```
with rasterio.open('data/geo/dem25_epsg25832.tif') as src:
    dem = src.read(1)
    dem[dem==-9999] = np.nan
    height = dem.shape[0]
    width = dem.shape[1]
    cols, rows = np.meshgrid(np.arange(width), np.arange(height))
    demx, demy = rasterio.transform.xy(src.transform, rows, cols)
```

Und dann in Eurem Kartenplot:

```
fig, ax = plt.subplots(1,1,figsize=(10,10))
...
plt.contour(demx, demy, dem, colors="brown", linewidths=0.75)
...
```

### R

- Ich hatte beim Einlesen der Metadaten und beim Zusammenführen zu einem Dataframe Probleme mit den Datentypen. Schaut euch in der Hilfe zu `read.table()` das Argument `colClasses` an. Tipp: Datentypen lassen sich nach dem Einlesen auch umformatieren, z.B. `xy` seien Zahlen als Zeichenkette, dann `as.numeric(xy)` zum Umwandeln.

- Zum Plotten der vielen Subplots: Schaut euch für sog. Multipannel-Plots (also eine Abbildungen mit vielen Subplots) einmal den Parameter `par(mfrow = c(,))` an. Der Parameter `par(mar = c(,))` ist auch nützlich bei der kosmetischen Gestaltung der Abbildung. Besipiel:

```
% 49 Plots angeordnet in 7 x 7:
par(mfrow = c(7,7))

% Ränder der Subplots (testet gerne auch andere Kombinationen, um zu verstehen, was passiert):
par(mar = c(2,2,1,.1))
```
