# API Übersicht

## Ziel dieses Kapitels

Dieses Kapitel beschreibt die Rolle, Grundsätze und fachliche Einordnung der REST-API innerhalb der SportFM-Plattform.

Die Detailbeschreibung einzelner Endpunkte erfolgt in den Dateien unter `04_REST_API/Endpunkte/`.

## Rolle der REST-API

Die REST-API ist der zentrale fachliche Zugriffspunkt auf SportFM.

Sie verbindet neue und bestehende Clients mit der vorhandenen SportFM-Fachlogik.

```text
SportPortal
WPF SportFM
weitere Clients
      ↓
REST-API SportFM
      ↓
Business Services
      ↓
PL/SQL-Packages
      ↓
Oracle 19c
```

## Grundsatz

Die REST-API ist keine Tabellen-API.

Sie veröffentlicht keine internen Tabellenstrukturen, sondern fachliche Funktionen und lesende Sichten auf bestehende Fachobjekte.

Nicht Ziel:

```text
Tabellen direkt veröffentlichen
bestehende Buchungslogik neu entwickeln
Gebührenlogik im Portal nachbauen
Winner-Logik im REST-Service neu berechnen
```

Ziel:

```text
bestehende SportFM-Logik kapseln
Portal und WPF einheitlich anbinden
Zugriff absichern
Daten portalgeeignet bereitstellen
Fachlogik zentral halten
```

## Führende Bestandteile

Die REST-API baut auf dem bestehenden produktiven System auf.

Führend bleiben:

| Bereich | Führend |
|---|---|
| Buchungslogik | SportFM / PL/SQL |
| Occurrences | PA_LHD_SPA_OCC |
| Winner-Logik | PA_LHD_SPA_OCC |
| Gebühren | SportFM / p_get_charges |
| Rechnungen | SportFM / SAP-Integration |
| Dokumente | bestehendes Dokumentenmodell |
| Stammdaten | RIB FM / SportFM |

## Abgrenzung bestehende und neue Logik

| Bereich | Einordnung |
|---|---|
| Booking | bestehende Fachlogik, REST-fähig machen |
| Availability | bestehende Occurrence-/Winner-Logik, REST-fähig machen |
| Charge | bestehende Gebührenlogik, lesend bereitstellen |
| Invoice | bestehende Rechnungen, lesend bereitstellen |
| Document | bestehende Dokumente, lesend bereitstellen |
| Application | neue Portal-Domäne |
| Workflow | neue Portal-/Plattform-Domäne |
| Authentication | neue Plattform-Domäne |
| Berechtigungen | neue Plattform-Domäne |

## Fachliche Endpunktgruppen

| Gruppe | Zweck |
|---|---|
| Authentication | Anmeldung, Token, 2FA, OAuth |
| Application | Anträge, Entwürfe, Einreichung |
| Booking | Buchungen anzeigen |
| Availability | freie Zeiten und Belegung anzeigen |
| Document | Dokumente anzeigen und herunterladen |
| Invoice | Rechnungen anzeigen |
| Charge | Gebühreninformationen anzeigen |
| ReferenceData | Auswahldaten und Stammdaten |

## Nicht-Ziele der V1

Für Version 1 gilt ausdrücklich:

- keine Neuentwicklung der bestehenden Buchungslogik
- keine neue Gebührenberechnung im Portal
- keine parallele SAP-Logik
- keine direkte Tabellen-API
- keine vollständige Ablösung der WPF-Anwendung
- keine tiefgreifende Datenmigration des bestehenden SportFM-Bestands

## V1-Schwerpunkt

Version 1 konzentriert sich auf:

| Schwerpunkt | Beschreibung |
|---|---|
| Portalzugriff | SportPortal konsumiert REST-API |
| Lesender Zugriff | Buchungen, Dokumente, Rechnungen, Gebühren |
| Antragssystem | neue Anträge über Wizard und Workflow |
| Sicherheit | Authentifizierung, Berechtigungen, Kontextprüfung |
| Plattformbasis | einheitliche Fehler, DTOs, Versionierung, Logging |

## Prinzip der Fachverantwortung

| Funktion | Verantwortlich |
|---|---|
| Antrag erfassen | Portal / Application-Service |
| Antrag prüfen | Workflow / SportFM-Sachbearbeitung |
| Buchungen ermitteln | SportFM |
| freie Zeiten ermitteln | PA_LHD_SPA_OCC |
| Gebühren ermitteln | SportFM |
| Rechnung erzeugen | SportFM / SAP-Prozess |
| Dokument anzeigen | REST-API mit Berechtigungsprüfung |

## Bezug zu anderen Kapiteln

| Kapitel | Inhalt |
|---|---|
| `02_Architektur` | Zielarchitektur und Plattformprinzipien |
| `03_Domaenen` | fachliche Domänenbeschreibung |
| `04_REST_API` | API-Grundsätze und Endpunkte |
| `05_Datenmodell` | Oracle-Tabellen, Packages, Datenmodell |

## Offene Punkte

| ID | Frage |
|---|---|
| OF-API-001 | finale Festlegung der V1-Endpunkte je Domäne |
| OF-API-002 | genaue DTO-Struktur je Endpunkt |
| OF-API-003 | Performance-Ziele je Endpunktgruppe |
| OF-API-004 | Umfang der WPF-Migration in V1 |

## Leitentscheidung

Die REST-API dient dem strategischen Ausbau von SportFM zur Plattform.

Sie schützt die bestehende Fachlogik, macht sie für neue Clients nutzbar und verhindert parallele Implementierungen im Portal.