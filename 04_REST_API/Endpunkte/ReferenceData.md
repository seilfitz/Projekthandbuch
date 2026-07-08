# ReferenceData

## Ziel

Diese Datei beschreibt REST-Endpunkte für Auswahldaten und Stammdaten, die von Portal und REST-Clients benötigt werden.

## Grundsatz

ReferenceData stellt vorhandene Daten lesend bereit.

Es erfolgt keine Neudefinition der fachlichen Stammdaten.

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| Sportanlagen laden | ja |
| Sportarten laden | ja |
| Belegungsarten laden | ja |
| Dokumenttypen laden | offen |
| Stadtbezirke laden | offen |
| Gebührenarten laden | offen |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /reference/facilities | Sportanlagen laden |
| GET | /reference/sporttypes | Sportarten laden |
| GET | /reference/eventtypes | Belegungsarten laden |
| GET | /reference/documenttypes | Dokumenttypen laden |

## Abgrenzung Facility und Booking

Facility beschreibt Sportanlagen, Sportkomplexe und Teileinheiten.

Booking beschreibt Buchungen und Belegungen.

Diese Domänen werden nicht vermischt.

## Datenquelle

Die Daten stammen aus dem bestehenden SportFM-Datenbestand.