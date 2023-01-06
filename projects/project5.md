---
layout: default
title: Grundwasser
nav_order: 5
parent: Projekte
---

# Grundwassersituation in Potsdam und Umgebung

## Übersicht

Die vergangenen fünf Jahre haben eine ungewöhnliche Häufung heißer und trockener Sommer in Brandenburg
gezeigt, welche das Grundwasser einem starken Stress ausgesetzt haben. Teils konnte die 
winterliche Grundwasserneubildung die Verluste nicht ausgleichen. Aber ist dies allein ein
Prozess der letzten fünf Jahre?

In diesem Projekt untersuchen wir mit Hilfe von Grundwasserbeobachtungen an amtlichen Pegeln die
Grundwassserdynamik in Potsdam und Umgebung im Laufe der letzten 20 Jahre.


## Daten

- Beobachtungen des Grundwasserstands an ausgewählten amtlichen Pegeln (LfU), Beobachtungszeitraum und Häufigkeit sind variabel
- Beobachtungen von Niederschlag, Lufttemperatur und Luftfeuchtigkeit am Standort Potsdam-Telegrafenberg (DWD)

Die Daten stehen auf der [Auskunftplattform Wasser](https://apw.brandenburg.de) zur Verfügung, wurden jedoch bereits
zur Nutzung in diesem Kurs heruntergeladen und unter Box.UP bereitgestellt ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj), Passwort: umweltdatenverarbeitung).

## Ergebnisse

- Lies zunächst die Pegeldaten aus dem Verzeichnis `data/gw/obs` ein. Jede zip-Datei repräsentiert einen Pegel und enthält die eigentlichen Messungen in einer `.csv`-Datei und die Metadaten des Pegels in einer Datei namens `Metadaten.csv`. Lies außerdem die Datei mit den Geokoordinaten der Pegel ein (`data/gw/obs/pegel.csv`). Nutze die sechsstellige Pegel-ID, um die Messwerte in Bezug zur Position des Pegels zu setzen.

- Erstelle eine Tabelle bzw. einen DataFrame, der folgende Informationen enthält: Pegel-ID, Länge, Breite, Geländehöhe, Startdatum der Zeitreihe, Enddatum der Zeitreihe. Welches sind die zehn Pegel mit den längsten Messreihen?

- Ermittle für jeden Pegel (soweit möglich) den Mittelwert von 2001-2010, den Mittelwert von 2020-2021 (einschließlich) sowie die Differenz beider Werte (im Folgenden `delta` genannt).
  
- Erstelle einen Multipanelplot mit einem Subplot pro Pegel (Empfehlung: 9x6). Jeder Subplot soll folgende Elemente und Informationen enthalten:
   - Zeitreihe des Pegelstandes relativ zum Mittelwert 2001-2010
   - Wie zuvor, aber geglättet mit Moving Window der Größe 1000 Tage
   - `delta` als Text
   - Name des Pegels (ggf. gekürzt) als Text

- Erstelle eine Karte des Gebiets, in welcher die Position jedes Pegels dargestellt und der Wert für `delta`
farblich codiert ist. Stelle dazu folgende Klassen dar: `delta` < -75 cm, -75 <= `delta` < -50, -50 <= `delta` < -25, -25 <= `delta`.
Nutze als Kartenhintergrund folgende Layer:

   - Gewässerflächen (`data/geo/gewaesser.shp`)
   - Gemeindegrenzen (`data/geo/gemeinden.shp`)
   - Siedlungsflächen (`data/geo/urban.shp`)
   - Höhenlinien, abgeleitet aus DEM (`data/geo/dem25_epsg25832.tif`)

- Stelle `delta` in Beziehung zur mittleren Höhe des Grundwasserstandes. Lässt sich ein Zusammenhang erkennen und ggf. erklären? Welcher Pegel fällt bzgl. des mittleren Pegelstandes besonders aus der Reihe? Warum? Nutze ggf. den mittleren Wasserstand (MW) des [Havelpegels in Potsdam](https://www.pegelonline.wsv.de/gast/stammdaten?pegelnr=580412) für eine Einordnung der Pegelhöhen.

## Hinweise

### Python

- Die eigentlichen Pegeldaten liegen in zip-Dateien verpackt. Anstatt diese alle auszupacken, könnt Ihr die Daten direkt aus der zip-Datei lesen. Dazu gibt es das Paket `zipfile`. Nehmen wir an in der zip-Datei `the.zip` liege eine Datei `the.csv`, dann:

```
import zipfile
zipf = zipfile.ZipFile("the.zip", "r")
f = zipf.open("the.csv")
df = pd.read_csv(f, ...)
f.close()
```
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

