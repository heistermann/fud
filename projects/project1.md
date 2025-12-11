---
layout: default
title: Starkniederschlag (R+Python)
nav_order: 1
parent: Projekte
---

# Radargestützte Niederschlagsrekonstruktion für das Ereignis im Juli 2021

## Übersicht

Ziel des Projekts ist die Darstellung des Niederschlags für das Hochwasserereignis
im Juli 2021 im Westen Deutschlands (Nordrhein-Westfalen und Rheinland-Pfalz).
Grundlage dafür ist das deutschlandweite Radarkomposit des Deutschen Wetterdienstes,
welches stündliche Niederschlagshöhen auf einem Gitter von 1x1 km für ganz Deutschland
bereitstellt. Zusätzlich zu einer räumlichen Darstellung des Niederschlags sollen
für ausgewählte Punkte und Flächen Zeitreihen der kumulativen Niederschlagssumme
dargestellt werden. Die räumlichen Darstellungen sollen mit administrativen Grenzen
sowie Hauptgewässern ergänzt werden.

## Daten 

- Die erforderlichen Daten sind allesamt frei verfügbar. Wir haben sie jedoch der
  Einfachheit halber für Euch als Paket zusammengestellt. Das Datenpaket ist unter
  Box.UP als zip-Datei verfügbar ([Link](https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj),
  Passwort: umweltdatenverarbeitung). Die Strukur der zip-Datei ist folgende:
  - `rw/...`: stündliche Niederschlagsraster als ESRI ASCII Grids (Quelle: 
     DWD Opendata Server, [Link](https://opendata.dwd.de/climate_environment/CDC/grids_germany/hourly/radolan/recent/asc/))
  - `shapefiles/ahr/`: Einzugsgebiet der Ahr bis Pegel Altenahr (freundlicherweise
     von Matthias Kemtner und Sergiy Vorogushyn, beide GFZ, bereitgestellt)
  - `shapefiles/bundeslaender/`: Grenzen der Bundesländer (Quelle: Bundesamt für
     Kartographie und Geodäsie, [Link](https://gdz.bkg.bund.de/index.php/default/digitale-geodaten/verwaltungsgebiete/verwaltungsgebiete-1-250-000-ebenen-stand-01-01-vg250-ebenen-01-01.html))
  - `shapefiles/osm/`: zwei Shapefiles, die über OpenStreetMaps (OSM) verfügbar sind und
    Fließgewässer in NRW und RLP darstellen: `osm_rivers_nrw_rlp.shp` stellt nur
    die Gewässer der Klasse "river" dar, `osm_waterways_nrw_rlp.shp` stellt alle
    Gewässer aus dem OSM-Layer "waterways" dar. Der Datensatz wurde aus Einzeldatensätzen
    zusammengestellt, die über die Geofabrik zur Verfügung stehen (Quelle: Geofabrik, 
    [Link](https://download.geofabrik.de/europe/germany.html))


## Zu erzielende Ergebnisse / Arbeitsschritte

- Einlesen der Niederschlagsdaten
- Kartendarstellung der gesamten Niederschlagssumme vom 12. bis 16. Juli 2021
- Kartendarstellung der täglichen Niederschlagshöhen vom 12. bis 16. Juli 2021
- Die Kartendarstellungen sollen jeweils die Grenzen der Bundesländer, die
  Hauptfließgewässer (osm_rivers_nrw_rlp.shp), das Einzugsgebiet der Ahr bis Pegel
  Altenahr sowie die Lage der Städte Hagen, Erftstadt und Bad Neuenahr-Ahrweiler
  enthalten. Die Kartendarstellung soll auf das Kernereignis in NRW und RLP zoomen.
- stündliche Zeitreihen der kumulativen Niederschlagshöhe für sollen für die Städte Hagen, 
  Erftstadt und Bad Neuenahr-Ahrweiler dargestellt werden. 
- Außerdem soll für das Einzugsgebiet der Ahr bis Pegel Altenahr der mittlere Gebietsniederschlag als 
  stündliche Zeitreihe dargestellt werden.


## Vorschläge zur Vorgehensweise in R
### Einlesen der Niederschlagsdaten

- Mit `list.files()` eine List aller Dateipfade zu den Niederschlagsgittern erstellen (Lektion 5)
- Sortiere diese Liste, um sicherzustellen, dass die zeitliche Reihenfolge stimmt (`sort()`)

- Probiere zunächst, ein einzelnes Niederschlagsgitter einzulesen (mit `raster()`, Lektion 6)
- Die Gitter enthalten den Niederschlag in der Einheit Zehntelmillimeter
  (1/10 mm) - bitte in mm umrechnen! 

- Mit `stack()` kannst Du deckungsgleiche Gitter übereinanderstapeln, um sie später gemeinsam zu behandeln.
  
- Erstelle nun ein Schleife über alle Dateipfade, lies jede einzelne Datei ein. (`stack()` akzeptiert auch die gesamte Dateiliste und stapelt diese. Seltsamerweise läuft dies aber deutlich langsamer als in der Schleife.)

- Das Wichtigste ist nun geschafft: Du hast die Daten in einem `stack`. 

- Die erforderlichen Vektordaten liest Du ein, wie es in Lektion 6 gelernt hast.

### Räumliche Bezugssysteme, übrige Geodaten

- Um eine Karte zu erstellen, musst Du alle darzustellenden Datensätze in ein
  räumliches Bezugssystem (CRS) bringen. Du solltest die Niederschlagsdaten in 
  ihrem CRS belassen und die übrigen Geodaten (Bundesländer, ...) in diese CRS
  umprojizieren. Leider ist das CRS der RADOLAN-Daten sehr ungewöhnlich. Darum geben
  wir Dir hier an, wie Du die korrekte Projektion setzt:

   ```
		radolanproj ="+proj=stere +lat_0=90 +lat_ts=90 +lon_0=10 +k=0.93301270189 +x_0=0 +y_0=0 +a=6370040 +b=6370040 +to_meter=1 +no_defs"
		crs(ein_geobjekt) = radolanproj #Festlegen einer Projektion für ein Geoobjekt
   ```
	Geodaten, die in einer anderen Projektion vorliegen, musst Du umprojizieren (siehe Lektion 6).

- Die Koordinaten für die Städte musst Du selbst rausfinden. Derartige Punkt-Geodaten lassen sich mittels `st_as_sf()` in R erzeugen. Auch diese müssen dann in das RADOLAN-CRS umprojiziert werden.
	
	Ob die Projektionen stimmen, kannst Du im nächsten Schritt visuell überprüfen.

### Karten erstellen

- Für die Kartendarstellung des Niederschlags kannst Du Dich daran orientieren,
  was wir in Lektion 6 für die Geländehöhe gemacht haben: Nutze entweder einfach `plot()` (folgendene Layer mit (`plot(..., add=TRUE)` oder die fortgeschrittenen Funktionen von `tmap`.
  Falls Du die Plots nebeneinander ausrichten willst, brauchst Du im ersten Fall ` par(mfrow=...) `, im zweiten `tmap_arrange()`, 
- Für die Darstellung der Niederschlagssummen der einzelnen Tage musst Du aus dem
  Gesamtarray die passenden Zeitscheiben auswählen. Die Funktion `subset()` könnte für den Rasterstack behilflich sein.

### Zeitreihen

- Für die Zeitreihendarstellung musst Du nur die entsprechenden Werte für jeden
  Zeitschritt aus dem 3D-Gitter extrahieren. Das geht mit `extract()`. Es kann mit Punkten und Polygonen extrahiert werden. Für letzteres ist das Argument `fun` hilfreich.
- Eine kumulative Summe berechnest Du mit `cumsum`.
- Die Zeitstempel kannst Du mittels `names(DeinRasterstack)` extrahieren, wenn Du danach diese Zeichenketten umwandelst, wie in Lektion 2 geschehen.


## Vorschläge zur Vorgehensweise in Python
### Einlesen der Niederschlagsdaten

- Mit `glob.glob` eine List aller Dateipfade zu den Niederschlagsgittern erstellen (Lektion 5)
- Sortiere diese Liste, um sicherzustellen, dass die zeitliche Reihenfolge stimmt (`sorted`)
- Erstelle zunächst einen 3-D `ndarray` (sagen wir mal ), der aus Nullen besteht, aber genau den
  `shape` hat, um Deine Niederschlagsdaten aufzunehmen (`(Zahl der Zeitschritte, 900, 900)`)
- Probiere zunächste, ein einzelnes Niederschlagsgitter einzulesen (mit `rasterio`, Lektion 6)
- Achtung: Die Gitter enthalten den Niederschlag in der Einheit Zehntelmillimeter
  (1/10 mm) - bitte in mm umrechnen! Vorher aber Fehlwerte maskieren - der Wert
  `-1` ist ein Fehlwertflag.
- Erstelle nun ein Schleife über alle Dateipfade, lies jede einzelne Datei ein
  und übertrage das eingelesene Gitter (2D) in den 3D-Array.
- Das Wichtigste ist nun geschafft: Du hast die Daten in einem `ndarray`. Eine
  Gesamtereignissumme kannst Du jetzt bereits ganz einfach erstellen...

### Räumliche Bezugssysteme, übrige Geodaten

- Um eine Karte zu erstellen, musst Du alle darzustellenden Datensätze in ein
  räumliches Bezugssystem (CRS) bringen. Du solltest die Niederschlagsdaten in 
  ihrem CRS belassen und die übrigen Geodaten (Bundesländer, ...) in diese CRS
  umprojizieren. Leider ist das CRS der RADOLAN-Daten sehr ungewöhnlich. Darum geben
  wir Dir hier an, wie Du ein `crs`-Objekt mit dem RADOLAN-CRS erstellen kannst.

   ```
   # WKT String
   radolanwkt = """PROJCS["Radolan Projection",
       GEOGCS["Radolan Coordinate System",
           DATUM["Radolan_Kugel",
               SPHEROID["Erdkugel",6370040,0]],
           PRIMEM["Greenwich",0,
               AUTHORITY["EPSG","8901"]],
           UNIT["degree",0.0174532925199433,
               AUTHORITY["EPSG","9122"]]],
       PROJECTION["Polar_Stereographic"],
       PARAMETER["latitude_of_origin",90],
       PARAMETER["central_meridian",10],
       PARAMETER["scale_factor",0.933012701892],
       PARAMETER["false_easting",0],
       PARAMETER["false_northing",0],
       UNIT["kilometre",1,
           AUTHORITY["EPSG","9036"]],
       AXIS["Easting",SOUTH],
       AXIS["Northing",SOUTH]]
   """
   import pyproj
   radolancrs = pyproj.CRS.from_wkt(radolanwkt)
   ```
- Du kannst nun wie gewohnt die Shapefiles mit `geopandas` einlesen und umprojizieren
  (Lektion 6).

### Karten erstellen

- Für die Kartendarstellung des Niederschlags kannst Du Dich daran orientieren,
  was wir in Lektion 6 für die Geländehöhe gemacht haben: Nutze `plt.imshow`
  zusammen mit dem Extent-Argument und plotte dann die Vektordaten darüber.
- Für die Darstellung der Niederschlagssummen der einzelnen Tage musst Du aus dem
  Gesamtarray die passenden Zeitscheiben auswählen. Dafür gibt es unterschiedliche
  Lösungen - das schaffst Du bestimmt.
- Die Koordinaten für die Städte musst Du selbst rausfinden und dann in das RADOLAN-CRS
  umprojizieren.

### Zeitreihen

- Für die Zeitreihendarstellung musst Du nur die entsprechenden Werte für jeden
  Zeitschritt aus dem 3D-Gitter extrahieren. Für das einfache Beispiel der Zeitreihe
  für eine bestimmte Gitterzelle `i,j` plottest Du also einfach `plt.plot(dtimes, x[:,i,j], ...)`.
- Zum Glück bietet `rasterio` eine sehr einfach Möglichkeit, die Zeilen- und Spaltenindizes
  aus den Raumkoordinaten zu ermitteln: Nutze für ein beliebiges `rasterio`-Objekt `rio`
  die Methode `rio.index(...)`.  
- Eine kumulative Summe berechnest Du mit `numpy.cumsum`
- Für die Ermittlung des Gebietsniederschlags bis zum Pegel Altenahr wird es etwas
  komplizierter (beachte: *Diese Aufgabe ist optional.*). Du musst zunächst eine
  Maske erstellen, mit der Du die Gitterzellen auswählen kannst, die innerhalb
  des Einzugsgebiets liegen. Dafür gehst Du z.B. wie folgt vor:
  
  ```
  rasterio.mask as riom
  tmp = rasterio.open("data/RW/RW-20210714/RW_20210714-2350.asc", crs=radolancrs)
  mask, transform = riom.mask(tmp, [cat.geometry[0]])
  mask = ~mask.astype("bool")
  ``` 
  
  `cat` ist hier der in `radolancrs` umprojizierte GeoDataFrame des Einzugsgebiets.

## Weitere Quellen

- BPB ([Link](https://www.bpb.de/politik/hintergrund-aktuell/337277/jahrhunderthochwasser-2021-in-deutschland))
- DWD ([Link](https://www.dwd.de/DE/leistungen/besondereereignisse/niederschlag/20210721_bericht_starkniederschlaege_tief_bernd.pdf?__blob=publicationFile&v=6))
- Uni Potsdam, NatRiskChange ([Link](https://www.uni-potsdam.de/fileadmin/projects/natriskchange/Taskforces/Flut2021_StatementThiekenEtAl.pdf))
- Wikipedia ([Link](https://de.wikipedia.org/wiki/Hochwasser_in_West-_und_Mitteleuropa_2021))