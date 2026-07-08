# Charge

## Ziel

Diese Datei beschreibt REST-Endpunkte für Gebühreninformationen.

## Grundsatz

Gebühren werden nicht im Portal berechnet.

Die Gebührenlogik bleibt in SportFM.

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| Gebühren zu Buchung anzeigen | ja |
| Gebühren zu Rechnung anzeigen | ja |
| Gebührenpositionen anzeigen | ja |
| Gebühren im Portal berechnen | nein |
| Gebührenlogik neu entwickeln | nein |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /bookings/{id}/charges | Gebühren zu einer Buchung laden |
| GET | /invoices/{id}/charges | Gebührenpositionen einer Rechnung laden |
| GET | /charges/{id} | Gebührenposition laden |

## Fachliche Abgrenzung

Charge ist nicht Invoice.

- Charge = Gebühr / Gebührenposition
- Invoice = Rechnung / Abrechnungsobjekt

## Bestehende Logik

Gebühreninformationen werden aus der bestehenden SportFM-Logik bereitgestellt.

`p_get_charges` ist als relevante bestehende Funktion zu berücksichtigen.