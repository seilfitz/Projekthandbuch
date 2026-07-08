# Booking

## Ziel

Diese Datei beschreibt die REST-Endpunkte für die Anzeige von Buchungen.

Die bestehende Buchungslogik wird nicht neu entworfen.

## Grundsatz

Booking nutzt die vorhandene SportFM-Fachlogik.

Führend bleiben:

- Oracle-Datenmodell
- PA_LHD_SPA
- PA_LHD_SPA_OCC
- bestehende Event-, Occurrence- und Winner-Logik

## V1-Umfang

In Version 1 werden Buchungen im Portal grundsätzlich lesend bereitgestellt.

| Funktion | V1 |
|---|:---:|
| Buchungsliste anzeigen | ja |
| Buchungsdetails anzeigen | ja |
| Dokumente zur Buchung anzeigen | ja |
| Gebühren zur Buchung anzeigen | ja |
| Rechnungen zur Buchung anzeigen | ja |
| Buchung im Portal bearbeiten | nein |
| Buchungslogik neu entwickeln | nein |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /bookings | Buchungsliste für berechtigten Kontext |
| GET | /bookings/{id} | Buchungsdetails |
| GET | /bookings/{id}/documents | Dokumente zur Buchung |
| GET | /bookings/{id}/charges | Gebühren zur Buchung |
| GET | /bookings/{id}/invoices | Rechnungen zur Buchung |

## Berechtigungen

Ein Portalbenutzer darf nur Buchungen sehen, die seinem berechtigten SportFM-Nutzer bzw. seiner Organisation zugeordnet sind.

## Offene Punkte

Keine fachliche Neukonzeption der Domäne Booking.

Zu klären bleibt nur die genaue REST-taugliche Ausgabeform der bestehenden Daten.