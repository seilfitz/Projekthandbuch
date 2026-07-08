# Availability

## Ziel

Diese Datei beschreibt die REST-Endpunkte zur Suche freier Zeiten und zur Bereitstellung belegungsbezogener Informationen.

## Grundsatz

Die Suche freier Zeiten nutzt die bestehende Occurrence- und Winner-Logik.

Führend bleiben:

- PA_LHD_SPA_OCC
- LHD_SPA_OCC
- LHD_SPA_OCC_WINNER
- bestehende performante Zugriffslogik

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| freie Zeiten suchen | ja |
| belegte Zeiten anzeigen | ja |
| Filter nach Zeitraum | ja |
| Filter nach Sportanlage | ja |
| Filter nach Sportart | ja |
| Filter nach Stadtbezirk | offen |
| Algorithmus neu entwickeln | nein |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /availability | freie Zeiten suchen |
| GET | /availability/occupancy | Belegung für Zeitraum laden |
| GET | /availability/facilities/{id} | Verfügbarkeit einer Sportanlage laden |

## Abgrenzung

Die REST-API berechnet keine eigene Belegungslogik.

Sie stellt die vorhandene SportFM-Logik portalgeeignet bereit.

## Offene Punkte

- genaue Filter für V1 festlegen
- Ausgabeformat für Trefferliste festlegen
- Performance-Ziele konkretisieren