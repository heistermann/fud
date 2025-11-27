---
layout: default
title: Schnee
nav_order: 7
parent: Projekte
---

# NUR FÜR PYTHON: Wasseräquivalent in der Schneedecke von 1950 bis heute - Vergleich von ERA5-Land und Beobachtungen des DWD

## Übersicht

ERA5 ist eine der weltweit bedeutendsten Reanalysen des Klimasystems. Eine Reanalyse kombiniert Modell und Beobachtungen, um so eine möglichst konsistente Rekonstruktion unterschiedlicher Variablen in Atmosphäre, Landoberfläche und Ozeanen zu erstellen. ERA5-Land fokussiert dabei auf die Landoberfläche und erreicht dabei eine räumliche Auflösung von etwa neun Kilometern.

Auch wenn das Ziel einer Reanalyse die konsistente Abbildung von Beobachtung und Physik anstrebt, bedeutet dies nicht, dass die Reanalyse alle Beobachtungen zuverlässig reproduziert. Dies ist aus einer Vielzahl von Gründen auch gar nicht das Ziel bzw. auch gar nicht möglich. Umso wichtiger ist es, sich für die jeweils betrachtete Klimavariable einer Region ein Bild über die Unsicherheit der Reanalyse zu verschaffen.

Im vorliegenden Projekt setzt Du Dich mit der ERA5-Land Rekonstruktion des Wasseräquivalents der Schneedecke (engl. **snow water equivalent, SWE**, in mm) auseinander. Dabei sollst Du einerseits die langfristige Änderung des SWE gemäß ERA5-Land untersuchen. Andererseits sollst Du das SWE aus ERA5-Land mit Beobachtungen des Deutschen Wetterdienstes (DWD) vergleichen, um die Unsicherheit der Reanalyse zu quantifizieren.


## Daten

- `era5-swe-airt.nc`: NetCDF-Datei mit monatlichen Mittelwerten des SWE und der Lufttemperatur für Europa (1951 bis heute) auf Basis von ERA5-Land. Die Variable `sd` enthält das SWE in der Einheit m. Empfehlung: Umrechnung in mm.
- `dwd/wa/Wa_Tageswerte_Beschreibung_Stationen.txt`: Stationsübersicht des DWD für Messstationen aus dem WA-Kollektiv (für **W**asser**a**equivalent). Enthält u.a. die Stations-ID sowie Standortkoordinaten und Stationshöhe.
- `dwd/BESCHREIBUNG_WA.pdf`: Beschreibung der eigentlichen Daten 
- `dwd/wa/`: In diesem Verzeichnis liegen die tägliche Messungen des DWD aus dem "WA"-Kollektiv (>1000 Stationen). Jede zip-Datei entspricht einem Stationsdatensatz (Namenschema: `tageswerte_Wa_{ID}_{Startdatum}_{Enddatum}_hist.zip`). Die Daten wurden vom Opendata-Repository des DWD heruntergeladen ([Link](https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/daily/water_equiv/)) - siehe auch die oben beschriebenen Dateien `Wa_Tageswerte_Beschreibung_Stationen.txt` (Stationsübersicht) und `BESCHREIBUNG_WA.pdf` (Beschreibung der Daten). Innerhalb jeder zip-Datei ist jeweils vor allem die Datei `produkt_waequi_tag_{...}.txt` relevant. Die Schwierigkeit mit diesem Datensatz: Messungen des SWE (Spalte `WASH_6`) sind nur ab einer Mindesthöhe der Schneedecke (typischweise 5 cm) sinnvoll durchführbar. Falls keine Messungen durchgeführt wurden, wird ein NoData-Wert angegeben. NoData kann in diesem Fall also zwei Dinge bedeuten: (a) Messungen wurden im Zeitraum grundsätzlich nicht durchgeführt; (b) Messungen wurden aufgrund zu geringmächtiger Schneedecke nicht durchgeführt. Dieser Sachverhalt erschwert die Abschätzung des mittleren SWE über längere Zeiträume (z.B. einen Monat). Um das obenstehende Problem zu beheben, sollt Ihr ferner die Beobachtungen der Schneehöhe (Parameter `SH_TAG`) berücksichtigen, welche durchgehend verfügbar sind. Weitere Hinweise unten. 
- `shapefiles/VG250_LAN.shp`: Shapefile mit Bundesländergrenzen (zur Visualisierung).

## Arbeitsschritte und zu erzielende Ergebnisse

- Lies die Datei `dwd/wa/Wa_Tageswerte_Beschreibung_Stationen.txt` ein und erzeuge daraus eine GeoDataFrame namens `walocs`. Nutze die Stations-ID als Zeilenindex.
- Schreibe eine Funktion, die für eine beliebige Station mit `ID` die entsprechenden Beobachtungen aus `dwd/wa/` in einen DataFrame einliest und diesen zurückgibt. 
- Wende nun die Funktion auf alle IDs in `walocs` an. Speichere die DataFrames in einem `dict` namens `wadata` ab, in welchem Du die Stations-ID als `key` nutzt. So kannst Du jederzeit darauf zugreifen. *Achtung*: Nicht für jede ID liegen Daten vor. Damit musst Du umgehen. Füge `walocs` eine Spalte namens `hasdata` hinzu und setze den Wert auf `False`, wenn keine Daten vorliegen (sonst `True`). Erzeuge dann einen neuen DataFrame (`walocs2`), der nur noch Einträge hat, für die `hasdata` `True` ist.
- "Trimme" die einzelnen DataFrames in `wadata`. Das soll heißen: Entferne alle Zeilen vor dem ersten und nach dem letzten gültigen Wert von `WASH_6`. 
- Berechne für jede Station eine vollständige Zeitreihe des SWE. Die Herausforderung dabei ist oben bereits beschrieben: `WASH_6` (also der SWE) wird nur gemessen, wenn eine Mindesthöhe der Schneedecke erreicht ist. Die Schneehöhe `SH_TAG` hingegen ist relativ lückenlos vorhanden. Dies kannst Du Dir zunutze machen: Berechne zunächst die Schneedichte (also Wasseräquivalent durch Höhe, nennen wir diese Variable einmal `dens` und fügen dafür eine neue Spalte hinzu). `dens` kannst Du interpolieren, also mit Hilfe von `pandas` die Lücken füllen, die durch fehlende Werte von `WASH_6` entstehen (`df["ipdens"] = df[["dens"]].interpolate(method='nearest')`). Indem Du nun `ipdens` mit `SH_TAG` multiplizierst, erhältst Du eine vollständige Reihe des SWE (neue Spalte `swe`).
- Erstelle nun einen neuen DataFrame namens `wadaily`, dessen Spalten die `swe`-Reihen der einzelnen Stationen enthalten (Datum als Zeilenindex, Stations-IDs als Spaltenindex). `wadaily` führt also die `swe`-Spalten aus den DataFrames zusammen, die im Dictionary `wadata` enthalten sind.
- Erzeuge durch Mittelwertsbildung aus `wadaily` einen DataFrame auf monatlicher Auflösung (`wamonthly`).
- Lies den ERA5-Land-Datensatz ein und extrahiere für jede Station aus `walocs2` den SWE-Wert aus dem ERA5-Land-Datensatz). Speichere die Ergebnisse in einem DataFrame `era5`, in dem jede Station aus `walocs2` durch eine Spalte repräsentiert wird. Als Zeilenindex nutzt Du die monatlichen Zeitstempel aus dem NetCDF-Datensatz.
- Ermittle nun für jede Station die folgenden Kenngrößen und füge die Ergebnisse `walocs2` hinzu:
   - den mittleren prozentualen Fehler (mean percentage error, MPE) der monatlichen Mittelwerte aus ERA5-Land (aus dem Vergleich von `wamonthly` und `era5`).
   - Welcher Monat hat im Mittel den höchsten SWE-Wert (auf Basis von ERA5-Land)?
   - SWE-Trend der ERA5-Land-Daten von 1950 bis 2024 für den Monat Februar. Nutze als Einheit für den Trend bitte mm/Dekade.
- Stelle die drei obigen Werte als Histogramm und als Karte dar. 
- Warum ist es sinnvoller, den Trend aus ERA5-Land zu berechnen als aus den Stationsmessungen? 

## Hinweise

### Python

#### Datei aus zip-Archiv lesen

```
import zipfile

# zip_path ist der Pfad zum zip-Archiv einer Station
with zipfile.ZipFile(zip_path, 'r') as z:
    # Erst ermitteln wir den Namen der Datei mit dem Datenprodukt
    fname = ""
    for f in z.namelist():
        if "produkt" in f:
            fname = f    

    # Dann lesen wir dies in einen DataFrame
    with z.open(fname) as f:
        weq = pd.read_csv(f, sep=";", na_values=-999.)
    ...
```

#### Zeitreihe für einen Standort aus einem NetCDF lesen (hier: NetCDF mit ERA5-Land-Daten)

```
import xarray as xr

ds = xr.open_dataset(".../era5-swe-airt.nc")
# lat und lon sind die Standortkoordinaten einer Station
# siehe dwd/wa/Wa_Tageswerte_Beschreibung_Stationen.txt
swe = (1e3*ds.sel(latitude=lat, longitude=lon, method="nearest")["sd"])
...

```