---
layout: default
title: Hurricane-Zugbahnen
nav_order: 2
parent: Projekte
---

# Analyse historischer Hurricane-Tracks

## Übersicht

Hurricanes gehören zu den Naturereignissen mit dem größten Schadenspotenzial. Dieses Projekt hat zum Ziel, das Hurricane-Risiko in den Vereinigten Staaten auf County-Ebene zu quantifizieren. Um dieses Ziel zu erreichen, ist es Eure Aufgabe, einen nicht-tabellarisch gespeicherten Datensatz zu Hurricane-Zugbahnen einzulesen und in einen räumlichen GIS-Vektordatensatz zu transformieren. Auf Basis einfacher Annahmen zu Durchmesser der Hurricanes, Immobilienwerten und Vulnerabilität wirst Du die mittleren jährlichen Schadenshöhen abschätzen, welche die Basis für die Berechnung von Versicherungsprämien sind. 

## Daten

- Die Daten für die Bearbeitung dieses Projekts sind auf Box.UP verfügbar ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj), Passwort: umweltdatenverarbeitung)
- HURDAT2: Die neuesten Best Track Daten (HURDAT2) für den Atlantik gibt es auf der Seite [https://www.nhc.noaa.gov/data/](https://www.nhc.noaa.gov/data/). Wir haben in dem Datensatz auf Box.UP unter `hurdat2/hurdat2-1851-2020-052921` bereits eine Datei für Euch bereitgestellt. Zusätzlich zu der Datei gibt es eine Formatbeschreibung als pdf (`hurdat2/hurdat2-format-nov2019.pdf`). Das Dokument beschreibt das Format der Datei ausführlich und hilft Euch so, das Textformat zu interpretieren. Euer Ziel ist es zunächst, die Trackdaten in ein Geodatenformat zu überführen (in R mithilfe der Pakete `sf` oder `sp`, in Python mit Hilfe von `geopandas`).

- Grenzen der US-Counties als Shapefile (`census/counties.shp`) und Zensusdaten (`census/DataSet.xlsx`): Die Attributtabelle von `counties.shp` beihaltet die Spalte FIPS (eine ID für die Counties). Diese benötigt ihr, um die räumlichen Daten mit den Zensusdaten in der Datei `DataSet.xlsx` zu verknüpfen. Aus dieser Exceldatei benötigt Ihr eigentlich nur die Spalte `HSG495212`: der "Median value of owner-occupied housing units, 2008-2012", vereinfacht gesagt der Median der Werte von Immobilienobjekten im jeweiligen County. 

## Ergebnisse

- eine Funktion, welche die HURDAT2-Daten einliest, und die mit zukünftig aktualisierten Daten ebenso funktionieren würde.
- Kartendarstellung der Hurricane-Zugbahnen
- Darstellung der Zeitreihen der Häufigkeit von Hurricanes von 1850 bis heute
- Risikoanalyse mit Kartendarstellung

## Hinweise (für R)

- Verwendet die Funktion readLines um die Daten einzulesen. Extrahiert für jedes gelistete Tiefdruckgebiet die Breiten- und Längegrade der Zugbahn und speichert sie als räumliches sf-Objekt ab (z.B. mit st_linestring(...)). Notiert zudem dabei, ob es sich bei einem der Tiefdruckgebiete auch um ein Hurricane handelt, am besten in einem data.frame. In der vierten Spalte gibt es den Eintrag HU, der diese Klassifizierung vornimmt. Kombiniert dann alle st_linestrings in eine Simple Feature Collection mittels st_sfc(...)). Fügt zum Schluss die Simple feature Collection mit dem data.frame zusammen mit Hilfe der Funktion st_sf().

- Bei der Kombination der Bundesstaaten (die ihr am besten mit der Funktion st_read einlest) und der Zensusdaten sollte euch die Funktion merge helfen.

- Um das Hurricane-Risiko zu berechnen, verwendet folgende Risikogleichung: R = H*V*A, wobei R das Risiko ($/y) ist, H die jährliche Häufigkeit von Hurricanes (1/y), V die Vulnerabilität (keine Einheit) und A die Werte ($), die der Naturgefahr ausgesetzt sind. Um H zu bestimmen, geht wie folgt vor: Nehmt einen charakteristischen Umfang eines Hurricanes an (z.B. 600 km). Erzeugt einen Buffer um die Hurricane Tracks (Achtung, hier wird der Radius benötigt). Berechnet für jedes County, wie häufig es von einem Hurricane getroffen wurde (z.B. st_intersects). Rechnet diese Werte in Häufigkeiten pro Jahr um. Um A zu bestimmen: Hier nehmen wir die Medianwerte der Immobilien im Zensusdatensatz. Um V zu bestimmen: Hier müssen wir kreativ sein, denn hier liegen uns keine Daten vor. Darum treffen wir hier einfach die Annahme, dass ein Hurricane zu Schaden am Haus führt, der 5% seines Wertes entspricht.  

## Hinweise für Python

### Verarbeitung der HURDAT2-Daten

Es gibt wohl unendlich viele Möglichkeiten, mit der HURDAT2-Datei umzugehen - dabei kannst Du Dich so richtig mit Stringmethoden austoben. Falls Du aber Schwierigkeiten hast, hier ein Rezept:

- Lies zunächst die ganze Datei mit `read` als einen String ein.
- Splitte diesen String mit dem Zeichen `\nAL`. Auf diese Weise erhältst Du eine Liste der einzelnen Sturmereignisse.
- Jetzt kommt der Trick: Du kannst nun jedes dieser Ereignisse als DateFrame "einlesen" und weiterverarbeiten. Auf folgende Weise kannst Du `pandas` vorgaukeln, 
  dass es auf eine Datei zugreift, obwohl Du direkt einen String übergibst. Sagen wir `storm` ist ein beliebiges Listeneintrag aus dem obigen Split. 

  ```
  import io
  data = io.StringIO(storm)
  df = pd.read_csv(data, sep=",", skiprows=1, header = None, dtype=str)
  ```

  Das Argument `skiprows` verwendest Du, weil jedes Ereignis einen Header beinhaltet, den Du gesondert verarbeiten musst. Aus dem Header musst Du noch das Jahr und den Namen des Ereignisses
  extrahieren. Aus dem DataFrame brauchst Du eigentlich nur die ersten sechs Spalten (Datum, Zeit, Ereignis - z.B. Landfall, Status des Sturmsystems - z.B. Hurricane, Lat, Lon). Diese musst Du
  allerdings noch ein bisschen weiterverarbeiten. Vor allem Lat und und Lon müssen noch in `float` umgewandelt werden - dafür musst Du die nervigen `N`, `W` und `E` loswerden.
- Jetzt kommt was Schönes: Sobald Du `df` zurechtgestutzt und saubergemacht hast, kannst Du den DataFrame in einen `GeoDataFrame` mit der Geometrie `LineString` umwandeln. 
  Dafür muss `df` zwei Spalten mit `lat` und `lon` als `float` enthalten:
  
  ```
  from shapely.geometry import LineString
  ls = LineString(np.array(df[["lon","lat"]]))
  df = pd.DataFrame({"name":["IRENE"], "year": 2011, "geometry":ls})
  gdf = gpd.GeoDataFrame(df, geometry="geometry", crs ="EPSG:4326")
  ```
	
- Mit `gdf` hast Du somit einen GeoDataFrame für *ein* Hurricane-Ereignis. Du kannst alle Ereignisse wie folgt hintereinanderhängen:
  
  ```
  # Leerer Container
  tracks = gpd.GeoDataFrame({}, columns=["name", "id", "geometry"], crs="EPSG:4326")
  ...
  tracks = tracks.append(gdf)
  ```
  
### Weiterer Umgang mit den Tracks

Sobald Du mit `tracks` eine GeoDataFrame hast, der alle Hurricane-Tracks enthält, kannst Du damit die tollsten Sachen machen. Zum Beispiel einen Buffer anlegen. Zuvor musst Du Dir aber 
eine sinnvolle - möglichst längentreue - Projektion für die Sog. "Continuous US" raussuchen. Nur dann ergibt ein Buffer Sinn.

  ```
  tracks = tracks.to_crs(...)
  buftracks = tracks.buffer(distance=...)
  ``` 

### Einlesen des County-Shapefiles und der Zensus-Daten

- `census/counties.shp` kannst Du einfach mit `geopandas.read_file` einlesen. Natürlich müssen sie in das gleiche CRS wie Deine `tracks` gebracht werden. 

- Um die Excel-Datei mit `pandas.read_excel` lesen zu können, musst Du `openpyxl` installieren:

  ```
  conda activate umweltdv
  conda install -c conda-forge openpyxl
  ``` 
- Für die Verknüpfung über die Spalte `FIPS` bzw. `fips` muss Du zunächst dafür sorgen, dass die entsprechenden Spalten in beiden Datensätzen den gleichen Datentyp und
  den gleichen Spaltennamen haben. 
- Anschließend kannst Du mit `pd.merge(..., ..., on="fips")` verknüpfen.

### Verschneidung der beiden Datensätze, Risikoanalyse

Jetzt kommt der zentrale inhaltliche Schritt: Du musst zählen, wie oft jedes County von einem Trackbuffer überdeckt wird. Lege dafür in Deine GeoDataFrame mit den counties (`counties`)
eine neue Spalte an (z.B. `noverlaps`). Lege nun eine Schleife über alle tracks in `buftracks` an und werte mittels `counties.geometry.intersects(trackbuf)` aus. Zähle alle Überlappungen pro County zusammen. Die eigentliche Risikoanalyse ist dann nur noch ein schlapper Einzeiler: 

`{Mittlere Zahl der Überlappungen pro Jahr} * {Median des Gebäudewerts} * 0.05`

Jetzt noch ein paar schöne Karten dazu - fertig.

 

   
