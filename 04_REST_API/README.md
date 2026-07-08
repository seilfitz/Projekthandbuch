# 04 REST-API

Dieses Kapitel beschreibt die REST-API der SportFM-Plattform.

Die REST-API ist der fachliche Zugriffspunkt für das SportPortal, die schrittweise Migration der bestehenden WPF-Anwendung und spätere Clients.

## Grundsatz

Die REST-API ist keine Tabellen-API.

Sie bildet fachliche Domänen und fachliche Anwendungsfälle ab.

```text
Client
  ↓
REST-API
  ↓
Business Services
  ↓
PL/SQL-Packages
  ↓
Oracle
```

## Struktur

| Datei / Ordner | Inhalt |
|---|---|
| `API_Uebersicht.md` | Ziel, Rolle und Einordnung der REST-API |
| `Authentifizierung.md` | Anmeldung, Token, 2FA, OAuth, Claims |
| `Berechtigungen.md` | Zugriffsschutz, Rollen, Kontext, Mandantentrennung |
| `Versionierung.md` | API-Versionen und Kompatibilitätsregeln |
| `Fehlerformat.md` | Einheitliches Fehlerformat |
| `DTOs.md` | Grundsätze für Datenübertragungsobjekte |
| `Endpunkte.md` | Übersicht der fachlichen Endpunktgruppen |
| `Endpunkte/` | Detailbeschreibung je fachlicher Domäne |

## Fachliche Endpunktgruppen

- Booking
- Availability
- Application
- Document
- Invoice
- Charge
- ReferenceData

## Abgrenzung

Die bestehende SportFM-Fachlogik wird nicht neu entworfen.

Insbesondere bleiben folgende Bestandteile führend:

- bestehendes Oracle-Datenmodell
- `PA_LHD_SPA`
- `PA_LHD_SPA_OCC`
- bestehende Gebühren-, Belegungs- und Winner-Logik

Die REST-API kapselt diese Logik für neue Clients.