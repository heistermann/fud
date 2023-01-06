---
layout: default
title: Schneehöhen
nav_order: 3
parent: Projekte
---

# Rekonstruktion einer Zeitreihe von Schneehöhen aus Fotos einer Wildtierkamera

## Übersicht

Am Versuchsstandort Marquardt (10 km nördlich von Potsdam) wurde im Winter 2020/21 im Rahmen des Cosmic-Sense-Projektes ein Schneemonitoring betrieben. Dafür dienten Messstäbe, die in regelmäßigen Abständen von einer Wildtierkamera aufgenommen wurden.

Aufgabe ist es, Anhand der Länge der aus dem Schnee ragenden Stäbe auf den Fotos sowie deren Zeitstempel eine Zeitreihe der Schneehöhe zu ermitteln und diese mit den Aufzeichnungen eines Ultraschall-Schneepegels zu vergleichen.

## Daten
Gegeben sind die Fotodaten der Wildtierkamera an Standort 02. An diesem Standort wurden 9 Stäbe beobachtet. Die Auswertung soll nur exemplarisch nur für *einen* Stab an diesem Standort erfolgen.

Die Daten liegen hier:
https://boxup.uni-potsdam.de/s/WgoamrJjWBt6KAj
Passwort: umweltdatenverarbeitung

	fotos.zip: Archiv der Fotos
	temp_precip_height.json: Beschreibung der Aufzeichnungen des Ultraschall-Schneepegels
	temp_precip_height.txt : Aufzeichnungen des Ultraschall-Schneepegels

## Ergebnisse
- examplarische Darstellung von Bildern (kein Schnee, viel Schnee)
- grafischer Vergleich<sup>1</sup> der Schneehöhe an einem Messstab mit der Zeitreihe aus der Ultraschallmessung<sup>2</sup>
- kurze Beurteilung der Ergebnisse

## Hinweise
- Die Auswertung soll halbautomatisch erfolgen. Dafür sollen alle Fotos des Verzeichnisses automatisch eingeladen <sup>3</sup> und dargestellt<sup>7</sup> werden und die Lage der Spitze (einmalig, konstant) und des Fußpunkts des Stabs (variabel mit der Schneehöhe) durch Klicken<sup>4</sup> des Nutzers bestimmt werden. 
- Die Zeitpunkte der Fotoaufnahmen lassen sich aus dem Dateidatum oder (sicherer) deren EXIF-Informationen<sup>5</sup> bestimmen.
- Die installierten Stäbe ragten 50 cm aus dem Boden. 5 cm vor der Spitze befindet sich eine weitere (rote) Referenzmarkierung, die ebenso genutzt werden kann.
- Die Erfassung soll vor allem für Phasen starker Änderung erfolgen; in Phasen gleicher Schneehöhe kann zwischen weiter auseinanderliegenden Aufnahmen interpoliert<sup>6</sup> werden (muss aber nicht).
- Es ist evtl. sinnvoll, die Plots der Fotos auf den auszuwertenden Stab zu zoomen, um die Genauigkeit zu erhöhen.
- Tipp: Die Koordinaten aufzunehmen ist natürlich etwas mühsam. Tipp: Speichere die aufgezeichneten Koordinaten als csv-Datei ab<sup>7</sup> . Dann kannst Du sie immer wieder
  laden, wenn Du weiterarbeitest.
- Die Aufzeichnungen des automatischen Schneepegels sind als Vergleich einzuladen<sup>2</sup> und Zeitstempel umwandeln<sup>9</sup> 

## Detailtipps für die Umsetzung in R
- <sup>1</sup> Lektion 3, Darstellung mit `plot()`
- <sup>2</sup> Lektion 3, `read.table()`
- <sup>3</sup> Lektion 5, `list.files()`, Lektion 4, `for()`
- <sup>4</sup> Funktion `locator()`
- <sup>5</sup> `file.info()` oder package `exiftool`
- <sup>6</sup> Funktion `approx()` 
- <sup>7</sup> Funktion `write_table(...)`
- <sup>8</sup> Lektion 5, ´load.image()´
- <sup>9</sup> ´strptime()´ und ´as.POSIXct()´

## Detailtipps für die Umsetzung in Python
- <sup>1</sup> Bilder lesen mit `matplotlib.image.imread(...)`, plotten mit `plt.imshow(...)`
- <sup>2</sup> Lektion 3, `pandas.read_csv()`
- <sup>3</sup> Lektion 5, `glob.glob()`, Lektion 4, `for`
- <sup>4</sup> Das ist in Python leider ein bisschen komplizierter...siehe unten.
- <sup>5</sup> In Python kannst Du den Zeitstempel einer Datei mittels `os.path.getmtime()` ermitteln. Der Zeitstempel kann dann mittels `pd.to_datetime(..., unit="s")` in ein `datetime`-Objekt umgewandelt werden.
- <sup>6</sup> Funktion `scipy.interpolate.interp1d()`
- <sup>7</sup> Funktion `pandas.to_csv(...)`

### Aufnahme von Abbildungskoordinaten in Python

Hier ist ein Beispiel, wie Du Punktkoordinaten aufnehmen kannst.

```
import matplotlib.pyplot as plt
import numpy as np
plt.ioff()
fig, ax = plt.subplots(1,1, figsize=(5,5))
myobj = plt.plot([2], [3], "k+", ms=15)
plt.xlim(0,10)
plt.ylim(0,10)
plt.title("Klicke auf den Punkt!")

coords = []

def onclick(event):
    ix, iy = event.xdata, event.ydata
    print("x = %d, y = %d" % (ix, iy) )

    global coords
    coords.append((ix, iy))

    return coords

for i in range(2,5):
    myobj[0].set_data((np.array([i]), np.array([i+1])))
    plt.draw()
    cid = fig.canvas.mpl_connect('button_press_event', onclick)
    # Du hast 5 Sekunden
    plt.pause(5)
    fig.canvas.mpl_disconnect(cid)
plt.close()
print(coords)
```


