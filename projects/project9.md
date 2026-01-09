---
layout: default
title: CO2-Bilanz (nur Python)
nav_order: 9
parent: Projekte
---

# **NUR FÜR PYTHON** Kohlenstoffbilanzierung an Standorten des ICOS-Netzwerks

## Übersicht

ICOS steht für "Integrated Carbon Observation System" und umfasst mehr als 170 Messstationen in Europa. In diesem Projekt konzentrieren wir uns auf die sogenannten "Ecosystem Observations". An den betreffenden Stationen werden insbesondere vertikale Flüsse von Energie, Wasser und Kohlendioxid mittels so genannter Eddy-Flux-Türme erfasst. Ziel dieser Aufgabe ist die Darstellung von Kohlenstoffflüssen und der CO~2~-Konzentration auf unterschiedlichen Zeitskalen. Ferner soll das Stationsmessnetz mit einer Karte der Klimazonen nach Koeppen abgeglichen werden.

## Daten
- `icos/FLUXES`: enthält für jeden ICOS Standort des Kollektivs "Ecosystem Observations" eine zip-Datei, die nach dem Land und dem Standortkürzel benannt ist. So enthält z.B. die Datei `ICOSETC_BE-Bra_FLUXES_L2.zip` die Daten für den Standort "Brasschaat" (Bra) in Belgien (BE). Jede dieser zip-Dateien enthält wiederum nur eine einzige csv-Datei mit den Eddy-Flux-Messdaten an der jeweiligen Station. Die Variablen sind in `icos/Fluxes_Variables.csv` beschrieben.
- `icos/METEO`: genauso wie `icos/FLUXES`, aber mit meteorologischen Messdaten. Die Variablen sind in `icos/Meteo_Variables.csv` beschrieben.
- `icos/stations.csv`: Enthält Metadaten der ICOS-Messstationen.
- `koeppen/koppen_geiger_0p00833333.tif`: Rasterdatensatz der Klimazonen nach Koeppen-Geiger für die Klimanormale 1991-2020. Jede Integer einer Gitterzelle kodiert eine Klimazone (siehe `legend.txt`).
- `koeppen/legend.txt`: Legende für den Koeppen-Datensatz. Ordnet den Integern aus dem Koeppen-Gitter die Klimazone zu (Hauptklimate und Unterklimate).
- `shapefiles/europe_countries.shp`: Shapefile mit den europäischen Ländern (für die Visualisierung).

## Zu erzielende Ergebnisse / Arbeitsschritte

- Ermittle aus der Tabelle `stations.csv` zunächst für jede Station eine ID, die sich aus Länder- und Stationskürzel zusammensetzt. Das wäre z.B. "BE-Bra" für obiges Beispiel. Dazu musst Du die Spalte `Id` verarbeiten und mit Stringoperationen die eigentliche ID extrahieren. Ferner sollst Du aus der Spalte `Position` den Längen- und Breitengrad jeweils als `float` isolieren. Erstelle dann einen DataFrame mit folgenden Spalten: `Name`, `Country`, `Lon`, `Lat`, `Site type` , `Elevation_above_sea` und `Labeling date`. Der Zeilenindex soll der Stations-ID entsprechen, den Du selbst gebastelt hast. Mit diesem DataFrame (wie nennen in einfach mal `stations`) arbeitest Du in der Folge weiter.
- Schreibe nun eine Funktion, die für eine beliebige `FLUXES`-Datei eines Standorts die `FLUXES` als DataFrame extrahiert und zurückgibt. Aus der Datei `ICOSETC_BE-Bra_FLUXES_L2.zip` müsste also `ICOSETC_BE-Bra_FLUXES_L2.csv` in einen DataFrame gelesen werden. Du sollst aber nicht alle Variablen (Spalten) zurückgeben, sondern nur "TIMESTAMP_START", "TIMESTAMP_END", "CO2", "LE" und "NEE". Sollte ein Standort hingegen nicht alle diese Variablen enthalten oder es gar keine Datei dafür geben, dann gib `None` zurück. Die Bedeutung der Variablen steht in der Datei `Fluxes_Variables.csv`, die in jedem Archiv enthalten ist. Füge in das Notebook eine kurze Erläuterung der Variablen und ihrer Einheiten hinzu.
- Wende nun Deine Funktion auf alle Standorte aus `stations` an. Speichere die jeweiligen DataFrames so ab, dass Du über ihre ID zugreifen kannst (in Python z.B. in einem dictionary). Füge dem DataFrame `stations` eine weitere Spalte "hasfluxes" hinzu und setze für eine Station den Wert auf `False`, wenn die Funktion `None` zurückgibt.
- Gehe nun genauso für das Auslesen von Daten aus dem Verzeichnis `METEO` vor. Hier sollst Du nur folgende Variablen auslesen: "LW_IN", "LW_OUT", "SW_IN" und "SW_OUT". Die Variablen sind in `Fluxes_Variables.csv` beschrieben.
- Füge dem DataFrame `stations` eine neue Spalte namens `tstart` hinzu, welche den Zeitstempel mit der ersten gültigen Messung an dieser Station enthält.
- Berechne für jede Station den Anteil fehlender Daten (NaN) für die Variable `NEE` für den Zeitraum nach tstart. Speichere das Ergebnis in `stations` in der Spalte `missingNEE`.
- Interpoliere für jede Station die Variable `NEE` mit `.interpolate` und füge das Ergebnis in einer neuen Spalte `NEEip` dem DataFrame hinzu.
- Berechne für jede Station, an der `missingNEE` < 0.5 ist, die Netto-CO~2~-Emission (oder Sequestrierung) im Zeitraum "2021-01-01" bis "2024-12-31" (Summe über `NEEip`). Füge den Wert dem DataFrame `stations` als Spalte `netNEE` hinzu (für alle anderen Stationen setze `netNEE` auf `numpy.nan`).
- Erstelle eine Karte Europas, in welcher Du den Wert von `netNEE` für jede Station farblich kodierst (mit horizontalem Colorbar als Legende). Stelle zur räumlichen Verortung das Shapefile mit den europäischen Ländern mit dar. Inwiefern sticht die Station "SE-Nor" hervor? Versuche zu recherchieren, was ist dort passiert ist. Zeige eine Zeitreihe von `NEE` für diese Station.
- Schreibe eine Funktion, welche für eine beliebige Station eine zweispaltige Abbildung erstellt:
   - linke Spalte: mittlere Tagesgänge (stündliche Mittelwerte) von `NEEip` sowie der kurzwelligen Einstrahlung `SW_IN` für Januar und Juli
   - mittlere Spalte: mittlere Jahresgänge (monatliche Mittelwerte) der gleichen Variablen
   - rechte Spalte: Zeitreihe der CO2-Konzentration (Tagesmittelwerte)
- Erweitere Deine Funktion so, dass für eine Liste von `n` Stations-IDs eine Abbildung mit zwei Spalten und `n` Zeilen erstellt wird (eine Zeile pro Station). Erstelle auf dieser Grundlage eine Abbildung mit fünf Stationen aus unterschiedlichen Klimazonen und/oder Ökosystemtypen. Nutze möglichst lange Zeitreihen. Verzeichne in den Zeilen jeweils auch die Standortattribute `Site type` sowie die Klimazone nach Köppen.
   

## Hinweise

### Python

#### Wie liest man eine Datei aus einem zip-Archiv in einen DataFrame ein?

```
import zipfile

zippath = "foo/bar.zip"
whichfile = "myfile.csv"
with zipfile.ZipFile(zippath, 'r') as z:
   if not whichfile in z.namelist():
      print("File %s no found in archive %s. Only found:" % (myfile, zippath))
         for fname in z.namelist():
            print(fname)
    else:
       with z.open(whichfile) as f:
           df = pd.read_csv(f, ...)
```
