---
layout: default
title: Klimaprojektionen
nav_order: 4
parent: Projekte
---

# Analyse und Darstellung von Klimaprojektionen (Niederschlag) für Potsdam

## Übersicht

Dynamische Regionale Klimamodelle (RCMs) simulieren die physikalischen Prozesse der Atmosphäre für einen begrenzten räumlichen Ausschnitt. Dabei werden sie zum einen dafür genutzt, um das Klima (nicht das Wetter!) der jüngeren Vergangenheit nachzubilden.  Zum anderen kann man mit ihnen, basierend auf Emissionszenarien, das Klima der Zukunft simulieren. 
 
In diesem Projekt sollen für den Standort Potsdam (Telegrafenberg)
 
a) die Güte der Niederschlagssimulationen des vom Max-Planck Institut betriebenen RCM 'REMO2009' im Vergleich zu den Messdaten, sowie  
b) die Änderungen im Niederschlag wie sie von 'REMO2009' unter Berücksichtigung des RCP4.5 Szenarios (moderate Emissionsentwicklung) projiziert werden 
 
analysiert und grafisch dargestellt werden.    

## Daten

- Niederschlagsmessdaten, Potsdam-Telegrafenberg
- Niederschlagssimulation für 1971-2000, MPI-REMO2009 0.11° räumliche Auflösung (Griddaten)
- Niederschlagsprojektion für 2071-2100 (RCP4.5), MPI-REMO2015 0.11° räumliche Auflösung (Griddaten)

Die Stationsdaten (.txt-Datei) sowie die Griddaten der Klimasimulationen (.nc-Dateien) stehen auf Box.UP zu Verfügung
https://boxup.uni-potsdam.de/s/pbSK8DLTBsNLJqS
Passwort: Umweltdatenverarbeitung

Beachte: Die RCM Daten werden immer in Blöcken für fünf Jahre bereitgestellt. Pro Klimaperiode (30 Jahre) müsstest du also auf sechs nc-Dateiein zegreifen.
Aufgrund der Größe der nc-Dateien, sind diese aber nur für die jeweils ersten fünf Jahre der beiden Zeiträume (also 1971-1975 und 2071-2075) unter dem o.g. Link bereitgestellt. 
Die Daten für die übrigen Jahre (1976-2000 und 2076-2100) findest du als Tabellen (csv-Dateien) für die Gridzelle, in der sich die Messstation befindet ebenfalls unter dem o.g. Link.
Die Originaldaten (nc-Dateien) der RCMs wurden hier heruntergeladen: https://cds.climate.copernicus.eu/cdsapp#!/dataset/projections-cordex-domains-single-levels   

## Ergebnisse

zu a)
- grafische Darstellung der mittleren Monatsniederschläge (gemessen und simuliert)
- grafische Darstellung der Abweichung der simulierten mittleren Monatsniederschläge im Vergleich zu den Messdaten
- grafischer Vergleich der Anzahl an Tagen mit Niederschlägen >= 30 mm pro Jahr (gemessen vs. simuliert)*

zu b)
- Klimaänderungssignal im mittleren monatlichen Niederschlags
- Änderungssignal in der Anzahl an Tagen mit Niederschlägen >= 30 mm pro Jahr (z.B. Vergleich min, median, max 2071-2100 vs 1971-2000)*
 
Änderungen und Abweichungen in Prozent bzw. Tagen
* Probiere es gerne auch mit größeren Schwellenwerten als 30 mm / Jahr 


## Hinweise / Ablauf
- Einlesen der Stationsdaten (siehe Lektion 3)
- Einlesen und Zugreifen auf die Klimasimulationsdaten für die jeweils ersten fünf Jahre (NetCDF, siehe Lektion 6)
- Erstellen von Dataframes für die Klimasimulationen (Lektion3)
- Anbinden der Tabellen aus den csv-Dateien (Daten für die übrigen je 25 Jahre) an die erstellten Dataframes (Lektion 3).
- Berechnung der Monatsmittel (Funktion aggregate(); Lektion 3)
- Berechnung der Anzahl an Tagen mit Niederschlägen größergleich Schwellenwert (z.B. über regelbasierte Teilmengen; Lektion 3)
- Berechnung der Abweichungen und Änderungen
- Visualiersung der Ergebnisse (Lektionen 2 und 3)

- Die größte Herausforderung im Zusammenhang mit den nc-Dateien ist die Identifizierung der Gridzelle im RCM, in der die Station Telegrafenberg verortet ist. Diese Gridzelle lässt sich mittels einer 'nearest neighbour' Suche identifizieren. Das folgende Scriptbeispiel für R zeigt den Ablauf: 
- Die Koordinaten der Klimastation Telegrafenberg lauten: 13.062 (longitude); 52.382 (latitude)

   ```
	% Beispiel in R
	% -------------
	library(ncdf4)
	
	rcm <- nc_open(filename)
	lon <- ncvar_get(rcm, "lon")
	lat <- ncvar_get(rcm, "lat")
	vlon <- as.vector(lon)
	vlat <- as.vector(lat)
	
	lonlat.rcm <- data.frame(lon = vlon, lat = vlat)
	lonlat.obs <- c(13.062, 52.382)
	
	% Nearest Neighbour Suche. Ergebnis: Vektor 'nnb' - Reihe (erster Eintrag) und Spalte (zweiter Eintrag) in der Koordinatenmatrix des RCM
	nnb <- c()
	
	% 1. Berechnung der Distanz zwischen der Stationskoordinate und allen Gridzellenmittelpunkten (mittels Pythagoras)
	d <- sqrt( (lonlat.rcm[,1] - lonlat.obs[1])^2 + (lonlat.rcm[,2] - lonlat.obs[2])^2 )
	
	% 2. Identifiziere die Position mit dem geringsten räumlichen Abstand
	nearnb <- match(min(d), d)
	ifelse (nearnb %% nrows != 0, nnb[1] <- nearnb %% nrows, nnb[2] <- nearnb/nrows)
	nnb[2] <- ceiling(nearnb / nrows)
	
	% Mittels den Einträgen in 'nnb' lässt sich dann auf die entsprechende Zelle in der netCDF-Datei zugreifen.
	p.data <- ncvar_get(rcm, "pr", start = c(nnb[1], nnb[2], 1), count = c(1,1,-1))
	
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ 
	% ++++ Überprüfung, ob die richtige Position gefunden wurde ++++++
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++
	library(maptools)	# Packet muss ggf. vorher installiert werden
	data(wrld_simpl) 	# Einladen einer politischen Weltkarte
	wrl <- as(wrld_simpl,"SpatialLines") 
	
	% Plot der Gridzellen-Mittelpunkte inkl. der Ländergrenzen und des Telegrafenbergs
	plot(lonlat.rcm, cex=.4, pch=3,col="grey", asp=1,xlim=c(10,15), ylim=c(50,55))
    lines(wrl)
    points(lonlat.obs[1], lonlat.obs[2], pch=16, col="red", cex=.6)
    title(main="Location of Station in GCM-grid")
    legend("bottomleft", c("GCM-grid","Obs-station"), pch=c(3,16), col=c("grey","red"), bg="white")
	
	% Hinzufügen der identifizierten "nearest neighbour" Gridzelle
	lon.cell <- ncvar_get(rcm, "lon", start=c(nnb[1],nnb[2]), count=c(1,1))
	lat.cell <- ncvar_get(rcm, "lat", start=c(nnb[1],nnb[2]), count=c(1,1))
	points(lon.cell, lat.cell, cex=.4, pch=3,col="green")
	legend("bottomright", "nnb", pch=3, col="green", bg="white")
	% ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++ ++++
	
   ```
