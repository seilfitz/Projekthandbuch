# Berechtigungen

## Ziel

Dieses Kapitel beschreibt den Zugriffsschutz der REST-API.

Die REST-API muss sicherstellen, dass Portalnutzer ausschließlich auf Daten zugreifen können, für die sie berechtigt sind.

## Grundprinzipien

| Nr. | Prinzip |
|---:|---|
| BP-001 | Jeder Zugriff erfolgt authentifiziert, soweit der Endpunkt nicht ausdrücklich öffentlich ist. |
| BP-002 | Jeder geschützte Zugriff wird gegen Benutzer, Rolle und Kontext geprüft. |
| BP-003 | Portalnutzer sehen nur Daten ihrer eigenen Organisation bzw. ihres eigenen SportFM-Nutzers. |
| BP-004 | Direkter Zugriff auf fremde Dokumente, Rechnungen, Buchungen oder Anträge ist ausgeschlossen. |
| BP-005 | Berechtigungslogik liegt nicht im Portal, sondern in der REST-API bzw. Plattform. |

## Kontextmodell

Ein Portalbenutzer kann in einem oder mehreren fachlichen Kontexten handeln.

Beispiele:

```text
Portalbenutzer
  ↓
Organisation / SportFM-Nutzer
  ↓
Rolle
  ↓
Berechtigte Funktionen
```

## Zu prüfende Zugriffsebenen

| Ebene | Beschreibung |
|---|---|
| Benutzer | Ist der Benutzer angemeldet? |
| Rolle | Darf die Rolle die Funktion ausführen? |
| Kontext | Handelt der Benutzer für die richtige Organisation? |
| Fachobjekt | Gehört das Objekt zum berechtigten SportFM-Nutzer? |
| Funktion | Ist Lesen, Erstellen, Ändern oder Einreichen erlaubt? |

## Besonders schützenswerte Objekte

- Anträge
- Buchungen
- Dokumente
- Rechnungen
- Benutzerdaten
- Organisationsdaten
- Uploads

## Abgrenzung

Das Portal darf Berechtigungen zur Benutzerführung auswerten.

Die verbindliche Berechtigungsentscheidung trifft immer die REST-API.