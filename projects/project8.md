---
layout: default
title: Energiebilanz (nur Python)
nav_order: 8
parent: Projekte
---

# **NUR FÜR PYTHON** Energiebilanzierung an Standorten des ICOS-Netzwerks

## Übersicht

ICOS steht für "Integrated Carbon Observation System" und umfasst mehr als 170 Messstationen in Europa. In diesem Projekt konzentrieren wir uns auf die sogenannten "Ecosystem Observations". An den betreffenden Stationen werden insbesondere vertikale Flüsse von Energie, Wasser und Kohlendioxid mittels so genannter Eddy-Flux-Türme erfasst. Ziel dieser Aufgabe ist die Darstellung charakteristischer Tages- und Jahresgänge der Energieebilanz (Strahlung, latente und fühlbare Wärmeflüsse) sowie die Ermittlung des [Bowen-Ratios](https://en.wikipedia.org/wiki/Bowen_ratio). Ferner soll das Stationsmessnetz mit einer Karte der Klimazonen nach Koeppen abgeglichen werden. 

## Daten
- `icos/FLUXES`: enthält für jeden ICOS Standort des Kollektivs "Ecosystem Observations" eine zip-Datei, die nach dem Land und dem Standortkürzel benannt ist. So enthält z.B. die Datei `ICOSETC_BE-Bra_FLUXES_L2.zip` die Daten für den Standort "Brasschaat" (Bra) in Belgien (BE). Jede dieser zip-Dateien enthält wiederum nur eine einzige csv-Datei mit den Eddy-Flux-Messdaten an der jeweiligen Station. Die Variablen sind in `icos/Fluxes_Variables.csv` beschrieben.
- `icos/METEO`: genauso wie `icos/FLUXES`, aber mit meteorologischen Messdaten. Die Variablen sind in `icos/Meteo_Variables.csv` beschrieben.
- `icos/stations.csv`: Enthält Metadaten der ICOS-Messstationen.
- `koeppen/koppen_geiger_0p00833333.tif`: Rasterdatensatz der Klimazonen nach Koeppen-Geiger für die Klimanormale 1991-2020. Jede Integer einer Gitterzelle kodiert eine Klimazone (siehe `legend.txt`).
- `koeppen/legend.txt`: Legende für den Koeppen-Datensatz. Ordnet den Integern aus dem Koeppen-Gitter die Klimazone zu (Hauptklimate und Unterklimate).
- `shapefiles/europe_countries.shp`: Shapefile mit den europäischen Ländern (für die Visualisierung).

## Zu erzielende Ergebnisse / Arbeitsschritte

- Ermittle aus der Tabelle `stations.csv` zunächst für jede Station eine ID, die sich aus Länder- und Stationskürzel zusammensetzt. Das wäre z.B. "BE-Bra" für obiges Beispiel. Dazu musst Du die Spalte `Id` verarbeiten und mit Stringoperationen die eigentliche ID extrahieren. Ferner sollst Du aus der Spalte `Position` den Längen- und Breitengrad jeweils als `float` isolieren. Erstelle dann einen DataFrame mit folgenden Spalten: `Name`, `Country`, `Lon`, `Lat`, `Site type` , `Elevation_above_sea` und `Labeling date`. Der Zeilenindex soll der Stations-ID entsprechen, den Du selbst gebastelt hast. Mit diesem DataFrame (wie nennen in einfach mal `stations`) arbeitest Du in der Folge weiter.
- Lies die Datei `koeppen/koppen_geiger_0p00833333.tif` sowie die zugehörige Legende (`koeppen/legend.txt`) ein. Ermittele für jeden Standort die zugehörige Gitterzelle und ordne auf diese Weise jedem Standort die Koeppen-Klimazone zu. Füge diese Information als zusätzliche Spalte `koeppen` Deinem `stations`-DataFrame hinzu.
- Schreibe nun eine Funktion, die für eine beliebige `FLUXES`-Datei eines Standorts die `FLUXES` als DataFrame extrahiert und zurückgibt. Aus der Datei `ICOSETC_BE-Bra_FLUXES_L2.zip` müsste also `ICOSETC_BE-Bra_FLUXES_L2.csv` in einen DataFrame gelesen werden. Du sollst aber nicht alle Variablen (Spalten) zurückgeben, sondern nur "TIMESTAMP_START", "TIMESTAMP_END", "H" und "LE". Sollte ein Standort hingegen nicht alle diese Variablen enthalten oder es gar keine Datei dafür geben, dann gib None zurück. Die Bedeutung der Variablen steht in der Datei `Fluxes_Variables.csv`, die in jedem Archiv enthalten ist. Füge in das Notebook eine kurze Erläuterung der Variablen und ihrer Einheiten hinzu.
- Wende nun Deine Funktion auf alle Standorte aus `stations` an. Speichere die jeweiligen DataFrames so ab, dass Du über ihre ID zugreifen kannst (in Python z.B. in einem dictionary, in R z.B. in einer Liste). Füge dem DataFrame `stations` eine weitere Spalte "hasfluxes" hinzu und setze für eine Station den Wert auf `False`, wenn die Funktion `None` zurückgibt.
- Gehe nun genauso für das Auslesen von Daten aus dem Verzeichnis `METEO` vor. Hier sollst Du nur folgende Variablen auslesen: "LW_IN", "LW_OUT", "SW_IN" und "SW_OUT". Die Variablen sind in `Fluxes_Variables.csv` beschrieben.
- Berechne nun das mittlere Bowen-Ratio für jede Station und füge den Wert dem DataFrame `stations` als Spalte `bowen` hinzu. Was ist das Problem in Bezug auf die Vergleichbarkeit der Werte für die unterschiedlichen Stationen?
- Erstelle eine Karte Europas, in welcher Du den Wert von `bowen` für jede Station farblich kodierst (mit horizontalem Colorbar als Legende). Nutze die Koeppen-Klimazonen als Hintergrund und stelle in einem vertikalen Colorbar eine Legende für die Koeppen-Zonen dar.
- Schreibe eine Funktion, welche für eine beliebige Station eine zweispaltige Abbildung erstellt:
   - linke Spalte: mittlere Tagesgänge (stündliche Mittelwerte) des fühlbaren und latenten Wärmeflusses sowie der Nettostrahlung für Januar und Juli
   - rechte Spalte: mittlere Jahresgänge (monatliche Mittelwerte) des fühlbaren (H) und latenten Wärmeflusses (LE) sowie der Nettostrahlung. Stelle auch die Summe der Kurven für H und LE dar und vergleiche diese mit der Nettostrahlung. Was fällt auf?
   - Welche Flussgröße der Energieebilanz an der Erdoberfäche ist in dieser Betrachtung noch nicht enthalten?
- Erweitere Deine Funktion so, dass für eine Liste von `n` Stations-IDs eine Abbildung mit zwei Spalten und `n` Zeilen erstellt wird (eine Zeile pro Station). Erstelle auf dieser Grundlage eine Abbildung mit fünf Stationen aus unterschiedlichen Klimazonen. Verzeichne in den Zeilen jeweils auch die Standortattribute `Site type` sowie die Klimazone nach Köppen.
   

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
