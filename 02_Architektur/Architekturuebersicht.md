# Gesamtarchitektur

## Zweck

Dieses Dokument beschreibt die Zielarchitektur der Weiterentwicklung von **SportFM** zur serviceorientierten Plattform mit REST-API, SportPortal und schrittweiser Migration der bestehenden WPF-Anwendung.

Die Architektur basiert ausschließlich auf den vorliegenden Projektinformationen aus Lastenheft, Snapshot und gelieferten ZIP-Dateien. Nicht gesicherte Punkte werden ausdrücklich als offene Punkte gekennzeichnet.

## Geltungsbereich

Dieses Dokument gilt für:

- die bestehende SportFM-Fachanwendung,
- die neue REST-API,
- das neue SportPortal,
- die schrittweise REST-Migration der WPF-Anwendung,
- die Anbindung vorhandener und zukünftiger Schnittstellen.

Nicht Bestandteil dieses Dokuments sind:

- vollständige REST-Endpunktdefinitionen,
- detaillierte Datenbankmodellierung,
- Aufwandsschätzung,
- UI-Detailkonzept,
- Testkonzept.

Diese Themen werden in eigenen Dokumenten ausgearbeitet.

## Quellenstand

| Quelle | Auswertung für dieses Dokument |
|---|---|
| `2026-07-05_Snappshot1.txt` | Projektentscheidungen, Architekturgrundsätze, technische Klarstellungen, Begriffsabgrenzungen |
| `2026-05_28_Lastenheft_SportFM.pdf` | fachlicher Auftrag, IST-/SOLL-Prozesse, Portal, Schnittstellen, Sicherheit, Betrieb |
| `GIT_IMSP.zip` | vorhandene Oracle-Tabellenstruktur für SportFM |
| `Module_SportFM.zip` | vorhandene Modul-Screenshots von SportFM |
| `Anlagen_Lastenheft.zip` | Antragsformulare, Portal-Skizzen, OZG-/Formcycle-Unterlagen |

## 1. Architekturziel

SportFM wird nicht durch das SportPortal ersetzt.

Ziel ist die Weiterentwicklung von SportFM zur zentralen Plattform für die Verwaltung kommunaler Sportstätten. Die bestehende Fachlogik bleibt erhalten und wird über eine REST-API kontrolliert bereitgestellt.

```text
SportPortal
WPF SportFM
zukünftige Clients
Fremdsysteme
    │
    ▼
REST-API SportFM
    │
    ▼
Business Services / bestehende PL/SQL-Fachlogik
    │
    ▼
Oracle 19c / RIB FM / SAP
```

## 2. Kritische Architekturfestlegung

Die REST-API ist der Kern der neuen Plattform.

Der Wizard im Portal ist ein wichtiges Modul zur Antragserfassung, aber **nicht** der Plattformkern. Er darf nicht zu einem eigenen Low-Code-System ausgebaut werden.

| Bereich | Bewertung |
|---|---|
| REST-API | Plattformkern |
| Oracle / PL/SQL | führende Fachlogik und Datenhaltung |
| SportPortal | neuer Client für Antragsteller und Portalnutzer |
| Wizard | pragmatisch konfigurierbare Antragserfassung |
| WPF SportFM | bestehender Client, schrittweise REST-Migration |

## 3. IST-Architektur

Die aktuelle Architektur basiert auf einer produktiven WPF-Fachanwendung mit direktem Oracle-Zugriff.

```text
WPF SportFM
    │
    ▼
Oracle Client / direkter Datenbankzugriff
    │
    ▼
Oracle 19c
    ├─ Tabellen
    ├─ Views
    ├─ Packages
    └─ PL/SQL-Fachlogik
    │
    ▼
RIB FM / SAP-Anbindungen
```

### 3.1 Bestand

| Bestandteil | Status |
|---|---|
| SportFM WPF | vorhanden, produktiv |
| Oracle 19c | vorhanden |
| PL/SQL-Fachlogik | vorhanden |
| RIB FM | vorhanden, führend für Stammdaten |
| SAP-Anbindung | vorhanden |
| REST-API | nicht vorhanden |
| SportPortal | neu zu entwickeln |
| Portal-Benutzerverwaltung | neu zu entwickeln |

## 4. Zielarchitektur

Die Zielarchitektur trennt die vorhandene Fachlogik von den Clients.

```text
                         SportPortal
                              │
                         WPF SportFM
                              │
                       zukünftige Clients
                              │
                              ▼
                    REST-API SportFM Plattform
                              │
              ┌───────────────┼────────────────┐
              │               │                │
        Application       Booking          Document
        Workflow          Availability     Invoice
        PortalUser        Charge           Reporting
        Authentication    Notification     Administration
              │               │                │
              └───────────────┼────────────────┘
                              ▼
                    bestehende Fachlogik
                              │
               PA_LHD_SPA / PA_LHD_SPA_OCC
                              │
                              ▼
                         Oracle 19c
                              │
                              ▼
                      RIB FM / SAP / weitere
```

## 5. Architekturprinzipien

| Kürzel | Prinzip | Festlegung |
|---|---|---|
| AR-001 | SportFM bleibt führend | SportFM bleibt fachlich führendes System für Buchung, Belegung, Dokumente, Gebühren und Rechnungen. |
| AR-002 | REST-API als Zugriffsschicht | Neue Clients greifen nicht direkt auf Oracle zu. |
| AR-003 | Keine doppelte Fachlogik | Buchungs-, Winner-, Occurrence- und Gebührenlogik werden nicht im Portal nachgebaut. |
| AR-004 | Oracle bleibt führend | Oracle 19c bleibt zentrale Datenhaltung. |
| AR-005 | PL/SQL wird weiter genutzt | Vorhandene Packages werden über die REST-API genutzt. |
| AR-006 | Portal ist Client | Das SportPortal stellt dar, erfasst Anträge und kommuniziert mit der REST-API. |
| AR-007 | WPF-Migration schrittweise | Keine Big-Bang-Ablösung der bestehenden Anwendung. |
| AR-008 | Fach-API statt Tabellen-API | Die REST-API bildet fachliche Operationen ab, keine Roh-Tabellen. |
| AR-009 | Modularer Monolith | Keine Microservice-Architektur, solange dafür keine belastbare Notwendigkeit besteht. |

## 6. Bestehende Fachlogik

Die vorhandene Fachlogik ist ein zentraler Investitionswert des Projekts.

### 6.1 Vorhandene Oracle-Strukturen

Aus `GIT_IMSP.zip` sind u. a. folgende Tabellen gesichert vorhanden:

| Bereich | Tabellen |
|---|---|
| Buchung / Belegung | `LHD_SPA_EVENTS`, `LHD_SPA_RECURRING_PATTERN`, `LHD_SPA_EVENT2UNIT`, `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER` |
| Dokumente | `LHD_SPA_DOCUMENTS`, `LHD_SPA_DOCUMENTS_EVENTS`, `LHD_SPA_DOCUMENT_TYPES`, `LHD_SPA_DOCUMENT_TEMPLATES`, `LHD_SPA_DOCUMENT_TEXTMODULES`, `LHD_SPA_DOCUMENT_NUMBERS` |
| Gebühren | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS`, `LHD_SPA_EVENTCHARGES` |
| Rechnungen | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICE_CHARGEINFOS`, `LHD_SPA_INVOICE_CI_HIST` |
| Sportstruktur | `LHD_SPA_SPORTCATEGORIES`, `LHD_SPA_SPORTGROUPS`, `LHD_SPA_SPORTSUBGROUPS`, `LHD_SPA_SPORTTYPES`, `LHD_SPA_SPORTSCOMPLEXES`, `LHD_SPA_FACILITY2COMPLEX` |
| System | `LHD_SPA_LOGGING`, `LHD_SPA_USER_SETTINGS`, `LHD_SPA_VERSION`, `LHD_SPA_CHANGEREQUESTS` |

### 6.2 Vorhandene Packages

| Package | Zweck |
|---|---|
| `PA_LHD_SPA` | bestehende Fachlogik für Wiederholungen, Stornierungen, Gebühren und weitere Buchungslogik |
| `PA_LHD_SPA_OCC` | Ermittlung der Occurrences und performante Termin-/Winner-Logik |

### 6.3 Architekturfolgen

Die REST-API darf diese Logik nicht ersetzen. Sie kapselt den Zugriff darauf.

```text
REST Controller
    │
    ▼
Application Service
    │
    ▼
Business Service
    │
    ▼
Oracle Package / View / Tabelle
```

## 7. Fachliche Kernobjekte

### 7.1 Sportstruktur

Die Sportstruktur wird wie folgt verstanden:

```text
Sportkomplex
    │
    ├── Sportanlage
    │       │
    │       ├── Teileinheit
    │       └── Teileinheit
    │
    └── Sportanlage
            └── Teileinheit
```

| Begriff | Bedeutung |
|---|---|
| Sportkomplex | organisatorische Zusammenfassung mehrerer Sportanlagen, z. B. Stadion oder Sportpark |
| Sportanlage | fachliche Anlage innerhalb eines Sportkomplexes |
| Teileinheit | kleinste buchbare Einheit |

Architekturfestelegung:

> Buchungsobjekt ist die Teileinheit. Sportkomplex und Sportanlage dienen der Strukturierung, Suche, Navigation und Auswertung.

### 7.2 Belegung

In SportFM werden belegungsrelevante Sachverhalte als Events geführt.

Dazu zählen:

- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltungen,
- Sperrungen,
- Reinigung,
- Schulbetrieb,
- GTA,
- Stornierungen.

Eine Stornierung ist fachlich kein reiner Statuswechsel, sondern eine Buchung vom Typ Stornierung.

## 8. Domänen der Zielarchitektur

| Domäne | Status | Architekturhinweis |
|---|---|---|
| Authentication | neu | Portalregistrierung, Login, 2FA, OAuth, BundID/MUK-Prüfung |
| PortalUser | neu | Portalnutzer, Organisationseinheiten, Abteilungen, Rollen, SportFM-Kontext |
| Application | neu | digitale Antragserfassung im Portal |
| Workflow | neu | Bearbeitungsstatus, Rückfragen, Historie, Zuständigkeiten |
| Booking | vorhanden | keine Neuentwicklung, REST-fähig machen |
| Availability | vorhanden/Erweiterung | Nutzung vorhandener Occurrence-/Winner-Logik |
| Facility | vorhanden/Erweiterung | REST-Zugriff auf Sportkomplexe, Sportanlagen, Teileinheiten |
| Document | vorhanden/Erweiterung | bestehende Dokumente über REST im Portal bereitstellen |
| Invoice | vorhanden/Erweiterung | bestehende Rechnungen über REST bereitstellen |
| Charge | vorhanden | Gebührenberechnung bleibt in SportFM |
| Notification | neu | E-Mail, Portalnachrichten, Rückfragen, Statusinformationen |
| Reporting | Erweiterung | vorhandene Auswertungen perspektivisch vereinheitlichen |
| Administration | neu/Erweiterung | Portal- und Fachadministration |

## 9. REST-API-Konzept

### 9.1 Grundsatz

Die REST-API ist keine CRUD-API auf Tabellen.

Nicht Ziel:

```text
GET /events
POST /events
PUT /events/{id}
```

Ziel:

```text
GET  /bookings/{id}
POST /applications/{id}/submit
POST /availability/search
GET  /documents/{id}
GET  /invoices/{id}
```

### 9.2 API-Domänen

```text
/api/v1/auth
/api/v1/organizations
/api/v1/portal-users
/api/v1/facilities
/api/v1/applications
/api/v1/workflows
/api/v1/bookings
/api/v1/calendar
/api/v1/availability
/api/v1/documents
/api/v1/invoices
/api/v1/charges
/api/v1/reports
/api/v1/admin
```

### 9.3 DTO-Grundsatz

Die API liefert keine Oracle-Tabellenstrukturen aus.

Es werden fachliche DTOs genutzt, z. B.:

- `BookingDto`,
- `BookingCalendarItemDto`,
- `AvailabilitySearchRequest`,
- `DocumentDto`,
- `InvoiceDto`,
- `PortalUserDto`,
- `OrganizationDto`,
- `ApplicationDto`.

## 10. SportPortal

Das SportPortal ist der neue Web-Client für Interessenten und registrierte Portalnutzer.

### 10.1 Aufgaben

Das Portal übernimmt:

- Registrierung und Anmeldung,
- Nutzerkonto,
- Suche nach Sportanlagen und freien Zeiten,
- Antragserfassung,
- Statusanzeige,
- Dokumentenanzeige,
- Rechnungsanzeige,
- Kommunikation mit Antragstellern.

Das Portal übernimmt nicht:

- Buchungsentscheidung,
- Winner-Berechnung,
- Gebührenberechnung,
- SAP-Buchung,
- fachliche Konfliktentscheidung.

### 10.2 Portalrubriken

Aus dem Lastenheft und Snapshot ergeben sich folgende Portalbereiche:

- Registrierung,
- Anmeldung / Nutzerkonto,
- Sportanlagensuche,
- Suche freier Zeiten,
- Anträge,
- Veranstaltungen,
- Vereine,
- Dokumente,
- Rechnungen,
- Favoriten.

## 11. Wizard / Antragserfassung

Der Wizard dient der digitalen Antragserfassung.

Kritische Festlegung:

> Der Wizard wird pragmatisch konfigurierbar, aber nicht als universelles Low-Code-System umgesetzt.

### 11.1 Muss-Funktionen

- Antragstyp auswählen,
- Schritte konfigurieren,
- Felder konfigurieren,
- Pflichtfelder,
- einfache Sichtbarkeiten,
- Uploads,
- Zwischenspeichern,
- Einreichen,
- Übergabe an REST-API,
- Anbindung an Workflow.

### 11.2 Nicht Ziel der ersten Architektur

- beliebige Berechnungslogik,
- verschachtelte Unterformulare als Standard,
- eigener Dokumentengenerator im Wizard,
- eigene Prozessengine im Wizard,
- vollständiges Low-Code-Formularsystem.

## 12. PortalUser / Organisation / SportFM-Kontext

### 12.1 Grundmodell

```text
Organisationseinheit (OE)
    ├── SportFM-Kontext
    ├── Portalnutzer
    └── Abteilungen
          ├── optional eigener SportFM-Kontext
          └── Portalnutzer
```

### 12.2 Festlegungen

- Eine OE kann mehrere Abteilungen besitzen.
- Ein Portalnutzer kann Admin einer OE sein.
- Ein Portalnutzer kann Admin oder Mitglied einer Abteilung sein.
- Eine Abteilung muss nicht automatisch ein eigener SportFM-Nutzer sein.
- Eine Abteilung kann optional einen eigenen SportFM-Kontext besitzen.
- Die Erzeugung eines eigenen SportFM-Nutzers aus einer OE oder Abteilung ist ein eigener später zu definierender Fachprozess.

### 12.3 Architekturfolge

Dokumente, Rechnungen, Anträge und Buchungen werden immer im jeweils berechtigten SportFM-Kontext angezeigt.

## 13. WPF-Migration

Die bestehende WPF-Anwendung bleibt zunächst produktiv.

Die Migration erfolgt schrittweise:

```text
Phase 1: SportPortal nutzt REST-API
Phase 2: einzelne WPF-Funktionen nutzen REST-API
Phase 3: WPF greift fachlich vollständig über REST zu
```

Keine Big-Bang-Ablösung.

## 14. Schnittstellenarchitektur

### 14.1 Bestehende oder genannte Schnittstellen

| Schnittstelle | Architekturstatus |
|---|---|
| RIB FM | bestehend, Stammdaten führend |
| SAP FI DD-IT | bestehender Prozess bleibt erhalten |
| Themenstadtplan / GDI | einzubinden bzw. zu prüfen |
| ePayBL | für Bürger/Unternehmen vorgesehen bzw. zu prüfen |
| DBOrg | bestehender Prozess bleibt erhalten |
| KIS | bestehender Prozess bleibt erhalten |
| Cardo | bestehender Prozess bleibt erhalten |
| Schuldatenbank Sachsen | bestehender Prozess bleibt erhalten, Details offen |
| Kitaportal Dresden | bestehender Prozess bleibt erhalten |
| LSBS | zu prüfen, Bereitschaft/Umsetzbarkeit offen |
| Formcycle | Schnittstelle für Onlineantragsassistenten im Lastenheft genannt |
| Sportpark-App | nicht Bestandteil dieses Projekts, aber Datenabruf/-bereitstellung muss fachlich berücksichtigt werden |

### 14.2 Architekturfestlegung

Bestehende Schnittstellen werden nicht ohne Not ersetzt.

Neue Schnittstellen werden über klar definierte Adapter oder API-Services gekapselt.

## 15. Sicherheit und Datenschutz

Die Architektur muss insbesondere folgende Anforderungen unterstützen:

- Authentifizierung,
- Autorisierung,
- Rollen- und Berechtigungskonzept,
- kontextbezogene Zugriffskontrolle,
- Protokollierung von Benutzeraktivitäten,
- Datenschutzgrundsätze,
- Lösch- und Aufbewahrungskonzept,
- Schutz von Dokumenten und Rechnungen,
- Revisionssicherheit für relevante Vorgänge.

### 15.1 Kritische Sicherheitsregel

Portalnutzer dürfen keine Dokumente, Rechnungen, Buchungen oder Anträge außerhalb ihres berechtigten SportFM-Kontextes sehen.

## 16. Betrieb und Deployment

### 16.1 Gesicherte Anforderungen

Aus dem Lastenheft ergeben sich mindestens:

- Testumgebungen für Onlineportal, Onlineantragsassistenten und RIB FM/SportFM,
- ausreichende Tests vor Produktivsetzung,
- Abnahme nach EB-IT-Vorgaben,
- Produktivstart-Unterstützung für mindestens drei Monate,
- Wartung, Support und Weiterentwicklung durch geregelte Verantwortlichkeiten.

### 16.2 Noch nicht festgelegt

| Punkt | Status |
|---|---|
| Hostingmodell SportPortal | offen |
| konkrete Serverarchitektur | offen |
| DMZ-Ausprägung | offen |
| API-Gateway | offen, derzeit nicht als gesetzt betrachten |
| Deploymentverfahren | offen |
| Monitoring-Werkzeuge | offen |

## 17. Kritische Architekturentscheidungen

### AR-010: Modularer Monolith statt Microservices

Die Architektur wird als modularer Monolith geplant.

Begründung:

- einheitliches Deployment,
- geringere Betriebskomplexität,
- bessere Beherrschbarkeit für kleines Entwicklungsteam,
- klare Domänentrennung ohne Microservice-Overhead.

Microservices werden nicht ausgeschlossen, sind aber für die erste Zielarchitektur nicht begründet.

### AR-011: Bestehende Booking-Logik nicht neu modellieren

Die Domäne Booking wird nicht neu entworfen.

Die vorhandene Logik wird dokumentiert, gekapselt und über REST bereitgestellt.

### AR-012: REST-API als Integrationsschicht

Die REST-API ist die verbindliche technische Zugriffsschicht für neue Clients.

### AR-013: Portal ohne Fachentscheidungen

Das Portal zeigt Daten an, erfasst Anträge und löst Prozesse aus. Es trifft keine fachlichen Buchungs-, Gebühren- oder Prioritätsentscheidungen.

## 18. Nicht Bestandteil der Zielarchitektur V1

Nicht als Bestandteil dieser Architektur festgelegt sind:

- Ablösung von Oracle,
- Ablösung von PL/SQL,
- Ablösung der bestehenden WPF-Anwendung,
- Neuentwicklung der Booking-/Winner-/Occurrence-Logik,
- Neuentwicklung der SAP-Integration,
- vollständige Microservice-Plattform,
- vollwertiges Low-Code-Formularsystem,
- produktive Mobile App,
- produktives Hallendisplay,
- vollständiges BI-/Statistikportal.

## 19. Offene Architekturpunkte

| ID | Thema | Status | Bemerkung |
|---|---|---|---|
| O-AR-001 | Hostingmodell | offen | erst mit IT-Infrastruktur konkretisieren |
| O-AR-002 | API intern/extern trennen | prüfen | abhängig von Sicherheits- und Betriebsmodell |
| O-AR-003 | API-Gateway | offen | derzeit nicht als gesetzt betrachten |
| O-AR-004 | BundID / MUK | prüfen | im Authentifizierungskonzept festlegen |
| O-AR-005 | ePayBL-Prozess | prüfen | besonders für Bürger/Unternehmen relevant |
| O-AR-006 | Formcycle-Rolle | prüfen | Lastenheft nennt Formcycle, Zielarchitektur muss technische Rolle klären |
| O-AR-007 | Erzeugung SportFM-Nutzer aus OE/Abteilung | offen | eigener Fachprozess erforderlich |
| O-AR-008 | Portal-Upload-Speicherort | offen | Klärung: bestehende Dokumentenverwaltung oder eigene Antrag-Anlagen-Struktur |

## 20. Architekturzusammenfassung

Die Zielarchitektur ist bewusst konservativ und investitionsschützend.

Sie baut auf den vorhandenen Stärken von SportFM auf:

- produktive Fachlogik,
- Oracle 19c,
- vorhandene PL/SQL-Packages,
- vorhandene Tabellenstruktur,
- vorhandene Dokumenten- und Rechnungslogik,
- vorhandene SAP-Integration.

Neu entstehen vor allem:

- REST-API als Plattformkern,
- SportPortal als neuer Client,
- Portal-Benutzer- und Organisationsverwaltung,
- Antragserfassung,
- Workflow,
- Benachrichtigung,
- kontextbezogene Zugriffssicherheit,
- Grundlage für spätere WPF-Migration.

## Referenzen

- `../01-Projekt/Projektuebersicht.md`
- `../02-Anforderungen/Lastenheft.md`
- `../02-Anforderungen/Pflichtenheft.md`
- `../05-Datenmodell/Datenbankmodell.md`
- `../07-API/API-Übersicht.md`
- `../10-Sicherheit/Sicherheitskonzept.md`
- `../13-Betrieb/Betriebskonzept.md`
- `../14-Entwicklung/Entwicklungsrichtlinien.md`
