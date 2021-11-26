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
a) die Güte der Niederschlagssimulationen des vom Max-Planck Institut betriebenen RCM 'REMO2015' im Vergleich zu den Messdaten, sowie  
b) die Änderungen im Niederschlag wie sie von 'REMO2015' unter Berücksichtigung des RCP4.5 Szenarios (moderate Emissionsentwicklung) projiziert werden 
analysiert und grafisch dargestellt werden.    

## Daten

Niederschlagsmessdaten, Potsdam-Telegrafenberg
Niederschlagssimulation für 1971-2000, MPI-REMO2015 0.11° räumliche Auflösung (Griddaten)
Niederschlagsprojektion für 2071-2100 (RCP4.5), MPI-REMO2015 0.11° räumliche Auflösung (Griddaten)

Die Stationsdaten (.txt-Datei) sowie die Griddaten der Klimasimulationen (.nc-Dateien) stehen auf Box.UP zu Verfügung
[Link]
Passwort: Umweltdatenverarbeitung 

## Ergebnisse

zu a)
- grafische Darstellung der Abweichung der simulierten mittleren Monatsniederschläge im Vergleich zu den Messdaten
- grafischer Vergleich der Anzahl an Tagen mit Niederschlägen >= 30 mm pro Jahr (gemessen vs. simuliert)*

zu b)
- Klimaänderungssignal im mittleren monatlichen Niederschlags
- Änderungssignal in der Anzahl an Tagen mit Niederschlägen >= 30 mm pro Jahr (z.B. Vergleich min, median, max 2071-2100 vs 1971-2000)*
 
* Probiere es gerne auch mit größeren Schwellenwerten als 30 mm / Jahr 


## Hinweise

- Änderungen und Abweichung in Prozent bzw. Tagen
- Die Gridzelle im RCM, in der die Station Telegrafenberg verortet ist, lässt sich mittels einer 'nearest neighbour' Suche identifizieren. Dafür müssen zuerst die geografischen Koordinaten des RCM-Grids in UTM Koordinaten umgewandelt werden (sp package in R).

	Beispiel in R
	-------------
	library(sp)
	library(ncdf4)
	
	rcm <- nc_open(filename)
	lon <- ncvar_get(rcm, "lon")
	lat <- ncvar_get(rcm, "lat")
	vlon <- as.vector(lon)
	vlat <- as.vector(lat)
	
	lonlat.rcm <- data.frame(lon = vlon, lat = vlat)
	lonlat.rcm <- SpatialPoints(lonlat.rcm, proj4string = CRS("+proj=longlat"))
	utm.rcm    <- spTransform(lonlat.rcm, CRS("+proj=utm +zone=33N"))
	
	utm.obs <- data.frame(lon = UTM.E, lat = UTM.N)
	utm.obs <- SpatialPoints(utm.obs, proj4string = CRS("+proj=utm +zone=33N"))
	
	# Nearest Neighbour Suche. Ergebnis: Vektor 'nnb' - Reihe (erster Eintrag) und Spalte (zweiter Eintrag) in der Koordinatenmatrix des RCM
	nnb <- c()
	
	# Estimate distance of station coordinate to every grid cell centroid (Pythagoras)
	d <- sqrt( (coordinates(utm.rcm)[,1] - coordinates(utm.obs)[1,1])^2 + (coordinates(utm.rcm)[,2] - coordinates(utm.obs)[1,2])^2 )
	nearnb <- match(min(d), d)															# match position with the smallest estimated distance
	ifelse (nearnb %% nrows != 0, nnb[1] <- nearnb %% nrows, nnb[2] <- nearnb/nrows)	# return 'rows' in GCM grid with smalles distance
	nnb[2] <- ceiling(nearnb / nrows)													# return 'cols' in GCM grid with smalles distance
	
	# Mittels den Einträgen in 'nnb' lässt sich dann auf die entsprechende Zelle in der netCDF-Datei zugreifen.
	
- UTM Koordinaten der Klimastation Telegrafenberg: 368111.274 (UTM.E); 5805315.889 (UTM.N)
