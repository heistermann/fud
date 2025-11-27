---
layout: default
title: Niederschlagstrends (nur Python)
nav_order: 10
parent: Projekte
---

# **NUR FÜR PYTHON** Niederschlag von 1950 bis heute - Vergleich von ERA5-Land und Beobachtungen des DWD

## Übersicht

ERA5 ist eine der weltweit bedeutendsten Reanalysen des Klimasystems. Eine Reanalyse kombiniert Modell und Beobachtungen, um so eine möglichst konsistente Rekonstruktion unterschiedlicher Variablen in Atmosphäre, Landoberfläche und Ozeanen zu erstellen. ERA5-Land fokussiert dabei auf die Landoberfläche und erreicht dabei eine räumliche Auflösung von etwa neun Kilometern.

Auch wenn das Ziel einer Reanalyse die konsistente Abbildung von Beobachtung und Physik anstrebt, bedeutet dies nicht, dass die Reanalyse alle Beobachtungen zuverlässig reproduziert. Dies ist aus einer Vielzahl von Gründen auch gar nicht das Ziel bzw. auch gar nicht möglich. Umso wichtiger ist es, sich für die jeweils betrachtete Klimavariable einer Region ein Bild über die Unsicherheit der Reanalyse zu verschaffen.

Im vorliegenden Projekt setzt Du Dich mit der ERA5-Land Rekonstruktion der jährlichen Niederschlagssumme auseinander. Dabei sollst Du einerseits die Unterschiede zwischen Stationmessung und Reanalyse darstellen, andererseits die langfristige Änderung des Jahresniederschlags untersuchen. 

## Daten

- `era5-prec.nc`: NetCDF-Datei mit monatlichen Mittelwerten der täglichen Niederschlagssumme für Europa (1951 bis heute) auf Basis von ERA5-Land
- `dwd/rr/RR_Monatswerte_Beschreibung_Stationen.txt`: Stationsübersicht des DWD für Messstationen aus dem RR-Kollektiv (Niederschlag). Enthält u.a. die Stations-ID sowie Standortkoordinaten und Stationshöhe.
- `dwd/rr/BESCHREIBUNG_RR.pdf`: Beschreibung der eigentlichen Daten 
- `dwd/rr/`: In diesem Verzeichnis liegen die tägliche Messungen des DWD aus dem "RR"-Kollektiv (>1000 Stationen). Jede zip-Datei entspricht einem Stationsdatensatz (Namenschema: `tageswerte_RR_{ID}_{Startdatum}_{Enddatum}_hist.zip`). 
- `shapefiles/VG250_LAN.shp`: Shapefile mit Bundesländergrenzen (zur Visualisierung).

## Arbeitsschritte und zu erzielende Ergebnisse

- Lies die Datei `dwd/rr/RR_Tageswerte_Beschreibung_Stationen.txt` ein und erzeuge daraus eine GeoDataFrame namens `locs`. Nutze die Stations-ID als Zeilenindex.
- Entferne alle Stationen (Zeilen) aus `locs`, für die das Attribut `bis` vor dem Jahr 2010 und das Attribut `von` nach 1960 liegt. 
- Schreibe eine Funktion, die für eine beliebige Station mit `ID` die entsprechenden Beobachtungen aus `dwd/rr/` in einen DataFrame einliest und diesen zurückgibt.  
- Wende nun die Funktion auf alle IDs in `locs` an. Speichere die resultierenden DataFrames in einem `dict` namens `rrdata` ab, in welchem Du die Stations-ID als `key` nutzt. So kannst Du jederzeit darauf zugreifen. *Achtung*: Nicht für jede ID liegen Daten vor. Damit musst Du umgehen. Füge `locs` eine Spalte namens `hasdata` hinzu und setze den Wert auf `False`, wenn entweder keine entsprechende Datei in `dwd/rr/` gefunden wird **oder** wenn die Station nach 1950 für die Variable `MO_RR` ausschließlich Fehlwerte enthält (sonst `True`).
- Übertrage nun die Zeitreihen für `MO_RR` aus `rrdata` in einen DataFrame namens `rr`. In `rr` entspricht der Zeilenindex den Monaten von "1950-01-01" bis "2024-12-01" (Zeitstempel entspricht jeweils dem Monatsersten), die Spalten entsprechen den Stations-IDs. Jetzt hast Du alle Beobachtungen schön in einem DataFrame.
- Ermittle für jede Station aus `locs` die Zeitreihe der monatlichen Niederschlagssummen aus dem nächstgelegenen ERA5-Land-Pixel. Die monatliche Niederschlagssumme (in mm) ergibt sich aus dem Produkt der Variable `tp` mal der Zahl der Tage des betreffenden Monats mal 1000 (m zu mm). Organisiere die Daten in einem DataFrame namens `era5`, der die exakt gleiche Struktur hat wie `rr`.
- Erzeuge aus `rr` und `era5` jeweils DataFrames mit den Jahressummen des Niederschlags von 1950 bis 2024 und nenne diese `rry` bzw. `era5y`. *Achtung*: Wenn für ein Jahr nicht Werte für alle Monate vorliegen, dann soll die Jahressumme NaN sein. In Python erreicht man dies mit `DataFrame.resample("YS").sum(min_count=12)`. Schau mal nach: Für welche Stationen wird in ERA5-Land gar kein gültiger Wert gefunden? Woran liegt das?
- Entferne alle Stationen aus `locs`, für die die Zahl gültiger Jahresniederschläge in `rry` oder `era5y` kleiner als 60 ist.
- Ermittle nun für jede Station aus `locs` die folgenden Maße und füge diese in einer entsprechenden Spalte in `locs` hinzu:
   - mittlerer Jahresniederschlag gemäß `rry` (`meanprec`, in mm)
   - mittlerer prozentualer Fehler (`MPE`, in %) des Jahresniederschlags von ERA5-Land. Achtung: Für jede Station kannst Du nur diejenigen Jahre für die Berechnung des MPE heranziehen, für die sowohl `rry` als auch `era5y` einen gültigen Wert haben (also nicht NaN).
   - Zeitlicher Trend des Jahresniederschlags zwischen 1950 und 2024 azs `rry`. Der Trend `trendrr` soll in % Änderung pro Dekade ggb. 1950 angegeben werden. Wenn Deine Regression also einen slope (Änderung in mm/Jahr) und einen intercept (bei 1950) ergibt, dann ergibt sich der Trend in %/Dekade als 100*slope*10/intercept.
   - Berechne den gleichen Trend nun für die ERA5-Reihen an den Stationen (`trendera5`)
- Stelle `meanprec`, `mpe`, `trendrr` und `trendera5` als Histogramme (in einer Abbildung) und als Karten (in einer Abbildung) dar. Nutze für die Kartenabbildung das Shapefile mit den Bundesländergrenzen.  
 Gütemaße aus einem Vergleich mit den ERA5-Land-Daten: den mittleren prozentualen Fehler (mean percentage error, MPE) und den mittleren absoluten prozentualen Fehler (mean absolute percentage error, MAPE). Stelle die Fehler aller Stationen als Histogramm dar. Füge die Werte als Spalte in `walocs2` hinzu.
- Ermittle für jede Station den SWE-Trend der ERA5-Land-Daten von 1980 bis 2024 - einmal den Trend für den Monat Februar und einmal den Trend des Mittelwerts der Monate Dezember bis Februar. Nutze als Einheit für den Trend bitte mm/Dekade. Füge beide Trendwerte als Spalte in `walocs2` hinzu und stelle sie für jede Station auf einer Karte dar.
- Warum ist es sinnvoller, den Trend aus ERA5-Land zu berechnen als aus den Stationsmessungen? 

## Hinweise

### Python

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

ds = xr.open_dataset(".../era5-prec.nc")
# lat und lon sind die Standortkoordinaten einer Station
# siehe dwd/rr/RR_Monatswerte_Beschreibung_Stationen.txt
tp = (1e3*ds.sel(latitude=lat, longitude=lon, method="nearest")["tp"])
tp = pd.DataFrame(index=tp["valid_time"], columns=["tp"])
days = np.array([date.days_in_month for date in df.index])
tp["prec"] = df[var] * days
```
