# Invoice

## Ziel

Diese Datei beschreibt die REST-Endpunkte für Rechnungen.

## Grundsatz

Rechnungen werden nicht im Portal erzeugt.

Das Portal zeigt bestehende SportFM-Rechnungen und zugehörige Dokumente an.

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| eigene Rechnungen anzeigen | ja |
| Rechnungsdetails anzeigen | ja |
| Rechnungsdokument herunterladen | ja |
| Zahlungsstatus anzeigen | offen |
| Rechnung im Portal erzeugen | nein |
| SAP-Logik neu entwickeln | nein |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /invoices | eigene Rechnungen laden |
| GET | /invoices/{id} | Rechnungsdetails laden |
| GET | /invoices/{id}/documents | Rechnungsdokumente laden |
| GET | /invoices/{id}/charges | Gebührenpositionen zur Rechnung laden |

## Abgrenzung Charge und Invoice

Charge beschreibt Gebühren bzw. Gebührenpositionen.

Invoice beschreibt die Rechnung als Abrechnungsobjekt.

Die Domänen werden nicht vermischt.

## SAP

SAP bleibt bestehender Integrationspunkt.

Die REST-API stellt nur portalrelevante Informationen bereit.