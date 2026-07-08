# Document

## Ziel

Diese Datei beschreibt die REST-Endpunkte für Dokumente.

## Grundsatz

Dokumente werden nicht im Portal dupliziert.

Das Portal greift über REST auf die bestehenden SportFM-Dokumente zu.

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| eigene Dokumente anzeigen | ja |
| Dokument herunterladen | ja |
| Dokumente zur Buchung anzeigen | ja |
| Dokumente zur Rechnung anzeigen | ja |
| Portal-Upload anzeigen | offen |
| Dokumentenerzeugung im Portal | nein |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /documents | eigene Dokumente laden |
| GET | /documents/{id} | Dokumentmetadaten laden |
| GET | /documents/{id}/content | Dokument herunterladen |
| GET | /bookings/{id}/documents | Dokumente zu einer Buchung |
| GET | /invoices/{id}/documents | Dokumente zu einer Rechnung |

## Berechtigungen

Ein Portalbenutzer darf nur Dokumente sehen, die seinem berechtigten SportFM-Nutzer bzw. seiner Organisation zugeordnet sind.

## Abgrenzung

Die bestehende Dokumentenlogik bleibt führend.