---
description: QGIS to view curve di livello dal file tiff
---

# Coordinate system

## Importa file tiff

Raster -> curve di livello

Le curve di livello sono spesso temporanee, e se non salvate vanno ricaricate ad ogni apertura del programma

## Aggiungi sistema di riferimento ISERNIA

Da TAF Italia, importare i punti di riferimento del comune di Isernia&#x20;

e nella tabella cercare

ISERNIA TESTA CHIODO MNIATO (rigo 8357).

Otterremo la zona di interesse:

41.668361N, 14.213607E su Google Maps per ritrovarla

## Sistemi di riferimento di coordinate

Sostanzialmente 3 sistemi di riferimento:

1. WGS 84 / EPSG 32633 UTM zone 33. Sistema di riferimento "proiettato" in metri. E' il sistema della mappa con i voxel letta da Geant4;
2. WGS 84 / EPSG 4326: Sistema di riferimento globale "più usato". E' quello presentato da Google MAPS. QGIS presenta di default questo sistema e quello precedente quando importiamo le curve di livello
3. Planimetria Tufo Colonoco: sistema usato nel file DWG fornitomi. Non conosco ancora i dettagli

## Importare DWG/DXF

Dal menu iniziale, importare DWG/DXF. Il sistema li porterà molto lontano a causa dei diversi sistemi di riferimento.

Una prima approssimazione per avvicinarlo è usare il sistema "Trasla" di QGIS.

I vettori traslati sono solitamente temporanei. Possono essere salvati però in un file esterno, per recuperarli facilmente.

I layer sono separati in innumerevoli layer, le etichette sono localizzate in **punti\_quo**. Per vedere le etichette dopo la traslazione fare tasto destro sul layer->Proprietà->Mostra Etichette -> Text.

Unire i layer durante l'importazione consente di traslarli tutti insieme, ma le etichette non sono più visibili.

## GeoReferenzazione

Oltre alla semplice traslazione di un dato offset (dx, dy, dz), è presente in modo nativo in QGIS dal menu Layer.

Si caricano i vettori dal file DXF della planimetria, e si possono selezionare alcuni punti di riferimento dal disegno. Quindi si forniscono le misure nel sistema di riferimento in uscita (per esempio, WGS 84 / EPSG 4326) e il sistema salverà la trasformazione effettuata. Possono essere anche salvati i punti di riferimento, attivati o disattivati dall'utente.

La stessa trasformazione va fatta per il vettore delle etichette e il vettore del disegno.
