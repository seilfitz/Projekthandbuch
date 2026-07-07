# Datenmodell Übersicht

| Feld | Wert |
|---|---|
| Kapitel | 05 – Datenmodell |
| Dokument | Datenmodell Übersicht |
| Status | Konsolidierter Arbeitsstand, Teil 2 |
| Typ | Fachliches Übersichts- und Referenzdokument |
| Priorität | Sehr hoch |
| Leitquellen | `Quellen/2026-07-05_Snapshot1.txt`, Lastenheft, Oracle-DDLs, Kapitel `03_Domaenen`, Architekturkapitel, fachliche Korrekturen Facility ↔ Booking und Charge ↔ Invoice |

---

## 1 Zweck

Dieses Dokument beschreibt die **fachliche Gesamtstruktur des SportFM-Datenmodells**.

Es ist der Einstieg in Kapitel `05_Datenmodell` und verbindet die bereits ausgearbeiteten Domänen mit dem vorhandenen Oracle-Bestand, den zentralen PL/SQL-Packages und der späteren REST- und Umsetzungssicht.

Die Übersicht beantwortet insbesondere folgende Fragen:

- Welche fachlichen Datenobjekte existieren im SportFM-Zielbild?
- Welche Domäne ist für welches Datenobjekt verantwortlich?
- Welche Oracle-Tabellen gehören fachlich zu welcher Domäne?
- Welche PL/SQL-Packages bleiben führend?
- Welche Datenbereiche werden über REST freigelegt?
- Welche Datenbereiche sind Bestandsdomänen und dürfen nicht neu modelliert werden?

Dieses Dokument beschreibt bewusst **nur das fachliche Datenmodell**.

Technische Details wie Trigger, Indizes, Sequenzen, Dirty-Tabellen, technische Hilfsstrukturen, Optimizer-Details oder vollständige Spaltenkataloge werden nicht hier, sondern in den nachfolgenden Dokumenten `Oracle_Datenmodell.md`, `Tabellen.md` und `Packages.md` behandelt.

---

## 2 Einordnung im Projekthandbuch

Kapitel `05_Datenmodell` liegt fachlich zwischen Domänenmodell und REST-API.

```text
03_Domaenen
  ↓
05_Datenmodell
  ↓
04_REST_API
  ↓
06_Arbeitspakete
  ↓
07_Kalkulation
```

Die Domänendokumente beschreiben die fachliche Verantwortung.

Das Datenmodell beschreibt, welche fachlichen Objekte und Bestandsdaten diese Verantwortung tragen.

Die REST-API beschreibt anschließend, wie diese Objekte kontrolliert verfügbar gemacht werden.

---

## 3 Geltungsbereich

Diese Übersicht umfasst:

- fachliche Business Objects,
- fachliche Datenbereiche,
- Domänenverantwortung,
- zentrale Oracle-Tabellen je Domäne,
- zentrale PL/SQL-Packages je Domäne,
- zentrale fachliche Abhängigkeiten,
- Abgrenzung zwischen Bestandsdaten und neuen Portaldaten,
- Auswirkungen auf REST, Migration und Umsetzung.

Diese Übersicht umfasst nicht:

- vollständige DDL-Dokumentation,
- vollständige Spaltenlisten aller Tabellen,
- technische Schlüsseldefinitionen im Detail,
- Trigger,
- Indizes,
- Sequenzen,
- Partitionierung,
- technische Batch- und Dirty-Queue-Details,
- physische Performanceoptimierung.

---

## 4 Modellierungsgrundsätze

### 4.1 Oracle bleibt führend

SportFM besitzt einen fachlich relevanten Oracle-Bestand.

Dieser Bestand bleibt führend für die zentralen Bestandsdomänen:

- Booking,
- Facility,
- Availability,
- Charge,
- Invoice,
- Document.

Für diese Domänen wird keine zweite fachliche Datenhaltung aufgebaut.

---

### 4.2 PL/SQL-Bestand bleibt führend

Bestehende PL/SQL-Logik wird nicht durch neue .NET-Logik ersetzt.

Verbindlich führend sind insbesondere:

| Package / Funktion | Fachliche Verantwortung |
|---|---|
| `PA_LHD_SPA` | zentrale bestehende SportFM-Logik, u. a. Buchung, Gebühren, Stornierung und weitere Bestandslogik |
| `PA_LHD_SPA_OCC` | Occurrence- und Winner-Logik, performante Termin- und Belegungszugriffe |
| `PA_LHD_SPA.p_get_charges` | Gebührenberechnung |

Die neue REST- und Serviceschicht kapselt diese Logik, ersetzt sie aber nicht.

---

### 4.3 Fachliches Datenmodell statt technisches Schema

Dieses Dokument modelliert fachliche Zusammenhänge.

Beispiel:

```text
BookingEvent
  ↓
EventUnitAssignment
  ↓
Occurrence
  ↓
OccurrenceWinner
```

Das ist keine vollständige physische Oracle-Struktur, sondern eine fachliche Sicht auf den Bestand.

---

### 4.4 Genau eine fachliche Verantwortung

Jedes zentrale Business Object besitzt genau eine fachlich verantwortliche Domäne.

| Business Object | Verantwortliche Domäne |
|---|---|
| `Application` | Application |
| `WorkflowTask` | Workflow |
| `Facility` | Facility |
| `FacilityUnit` | Facility |
| `BookingEvent` | Booking |
| `SportType` | Booking |
| `Charge` | Charge |
| `Invoice` | Invoice |
| `Document` | Document |
| `Upload` | Upload |
| `PortalUser` | PortalUser |
| `Context` | Context |

---

### 4.5 Keine direkte Tabellen-API

REST-Endpunkte bilden keine Oracle-Tabellen direkt ab.

REST arbeitet fachlich über DTOs, Services und domänenbezogene Berechtigungen.

Beispiel:

Nicht gewünscht:

```text
GET /api/lhd_spa_events/{id}
```

Gewünscht:

```text
GET /api/v1/bookings/{id}
```

---

### 4.6 Keine Doppelmodellierung

Fachliche Daten werden nicht in mehreren Domänen doppelt modelliert.

Verbindliche Abgrenzungen:

| Thema | Verantwortliche Domäne | Nicht verantwortlich |
|---|---|---|
| Sportanlage | Facility | Booking |
| Teileinheit | Facility | Booking |
| Sportart | Booking | Facility |
| Sportgruppe | Booking | Facility |
| Sportuntergruppe | Booking | Facility |
| Sportkategorie | Booking | Facility |
| Gebühren | Charge | Invoice |
| Rechnung | Invoice | Charge |
| Dateiannahme | Upload | Document |
| dauerhaftes Dokument | Document | Upload |
| Login / Token | Authentication | PortalUser |
| fachliches Profil | PortalUser | Authentication |
| Sichtbarkeitsraum | Context | Organisation |

---

## 5 Fachliche Gesamtübersicht

Das fachliche Datenmodell gliedert sich in mehrere Datenbereiche.

```text
Benutzer / Organisation / Kontext
  ↓
Antrag / Wizard / Workflow / Upload
  ↓
Facility / Availability / Booking
  ↓
Charge / Invoice / Document
  ↓
Notification / Dashboard / Administration
```

### 5.1 Benutzer, Organisation und Kontext

Dieser Bereich bildet Identität, fachliches Profil, Organisationen, Mitgliedschaften und den aktiven fachlichen Arbeitsraum ab.

| Domäne | Fachliche Datenobjekte |
|---|---|
| Authentication | technische Identität, Login, Token, Sperrstatus |
| PortalUser | Portalprofil, Kontaktdaten, Einstellungen, Favoriten, Zustimmungen |
| Organisation | Organisation, Abteilung, Mitgliedschaft, Organisationsrolle |
| Context | fachlicher Arbeitskontext, sichtbare Organisation / Abteilung / Rolle |

### 5.2 Antrag und Bearbeitung

Dieser Bereich bildet neue Portalprozesse für Antragstellung und Bearbeitung ab.

| Domäne | Fachliche Datenobjekte |
|---|---|
| Application | Antrag, Antragsteller, Antragspayload, Antragsstatus |
| Wizard | Wizarddefinition, Schritt, Feld, Validierung, Pflichtanlage |
| Workflow | Vorgang, Aufgabe, Rückfrage, Entscheidung, Statusübergang |
| Upload | Upload, Datei, Uploadstatus, Uploadzuordnung |

### 5.3 Sportstätten, Belegung und Buchung

Dieser Bereich bildet den fachlich wichtigsten Bestandskern von SportFM ab.

| Domäne | Fachliche Datenobjekte |
|---|---|
| Facility | Sportkomplex, Sportanlage, Teileinheit, FacilityGroup |
| Availability | Verfügbarkeitsabfrage, freies Zeitfenster, Konflikt, Kalenderansicht |
| Booking | Event, Buchung, Eventtyp, Eventklasse, Sportart, Sportgruppe, Wiederholungsmuster, Occurrence, Winner |

### 5.4 Gebühren, Rechnungen und Dokumente

Dieser Bereich bildet Gebühreninformationen, Rechnungen und Dokumente ab.

| Domäne | Fachliche Datenobjekte |
|---|---|
| Charge | Charge, ChargeType, ChargeGroup, EventCharge, InvoiceChargeInfo |
| Invoice | Rechnung, Rechnungsstatus, Zahlstatus, SAP-Status, Rechnungsdokumentbezug |
| Document | Dokument, Dokumenttyp, Dokumentstatus, Dokumentnummer, PDF-Inhalt, Vorlage, Textbaustein |

### 5.5 Querschnittsdaten

Dieser Bereich aggregiert oder unterstützt Fachprozesse.

| Domäne | Fachliche Datenobjekte |
|---|---|
| Notification | Portalnachricht, Empfänger, MailQueue, Versandstatus, Vorlage |
| Dashboard | Dashboardbereich, Aufgabenübersicht, Antragsübersicht, Dokument-/Rechnungsübersicht |
| Administration | Systemparameter, AdminAudit, Konfiguration, administrative Sichten |

---

## 6 Fachliche Datenlandkarte

```mermaid
graph TD
    AUTH[Authentication]
    PU[PortalUser]
    ORG[Organisation]
    CTX[Context]

    APP[Application]
    WIZ[Wizard]
    WFL[Workflow]
    UPL[Upload]

    FAC[Facility]
    AVL[Availability]
    BOOK[Booking]

    CHG[Charge]
    INV[Invoice]
    DOC[Document]

    NOT[Notification]
    DSH[Dashboard]
    ADM[Administration]

    AUTH --> PU
    PU --> ORG
    ORG --> CTX
    CTX --> APP
    CTX --> BOOK
    CTX --> DOC
    CTX --> INV

    APP --> WIZ
    APP --> WFL
    APP --> UPL
    APP --> FAC
    APP --> CHG

    FAC --> AVL
    AVL --> BOOK
    BOOK --> CHG
    BOOK --> DOC
    BOOK --> INV

    CHG --> INV
    INV --> DOC
    UPL --> DOC

    WFL --> NOT
    DOC --> NOT
    INV --> NOT
    APP --> NOT

    DSH --> APP
    DSH --> WFL
    DSH --> NOT
    DSH --> DOC
    DSH --> INV

    ADM --> AUTH
    ADM --> ORG
    ADM --> CTX
```

---

## 7 Domänen → Business Objects

| Domäne | Primäre Business Objects | Charakter |
|---|---|---|
| Authentication | Identity, Login, Token, AccountLock | neue Portaldomäne |
| PortalUser | PortalUser, Profile, Contact, Preference, Favorite, Consent | neue Portaldomäne |
| Organisation | Organisation, Department, Membership, OrganisationRole | neue / erweiterte Portaldomäne |
| Context | Context, ContextScope, ContextRole | neue Portaldomäne |
| Application | Application, ApplicationDraft, ApplicationPayload, ApplicationStatus | neue Portaldomäne |
| Wizard | WizardDefinition, WizardStep, WizardField, WizardValidation | neue Portaldomäne |
| Workflow | WorkflowInstance, WorkflowTask, Query, Decision, Transition | neue / erweiterte Portaldomäne |
| Upload | Upload, UploadFile, UploadCategory, UploadAssignment | neue Plattformdomäne |
| Facility | SportsComplex, Facility, FacilityUnit, FacilityGroup | Bestandsdomäne |
| Availability | AvailabilityQuery, AvailabilityResult, TimeSlot, Conflict | Bestands-/Lesedomäne |
| Booking | BookingEvent, EventType, EventClass, RecurringPattern, Occurrence, OccurrenceWinner, SportType, SportGroup, SportSubGroup, SportCategory | Bestandsdomäne |
| Charge | Charge, ChargeType, ChargeGroup, ChargeFacilityGroupAssignment, EventCharge, InvoiceChargeInfo | Bestandsdomäne |
| Invoice | Invoice, InvoiceStatus, PaymentStatus, SapStatus, InvoiceDocument | Bestandsdomäne |
| Document | Document, DocumentType, DocumentState, DocumentTemplate, DocumentTextModule, DocumentContent | Bestandsdomäne |
| Notification | Notification, PortalMessage, Recipient, MailQueueItem, DeliveryStatus | neue Querschnittsdomäne |
| Dashboard | DashboardDto, DashboardSection, DashboardTaskItem | Aggregationsdomäne |
| Administration | SystemParameter, ConfigurationItem, AdminAuditEntry, AdminDashboardItem | Verwaltungsdomäne |

---

## 8 Wichtige fachliche Abgrenzungen

### 8.1 Facility ↔ Booking

Facility beschreibt Sportstätten.

Booking beschreibt die Nutzung dieser Sportstätten.

| Objekt | Domäne |
|---|---|
| Sportkomplex | Facility |
| Sportanlage | Facility |
| Teileinheit | Facility |
| FacilityGroup | Facility |
| Event / Buchung | Booking |
| Eventtyp | Booking |
| Sportart | Booking |
| Sportgruppe | Booking |
| Sportuntergruppe | Booking |
| Sportkategorie | Booking |

Diese Abgrenzung ist verbindlich, weil `LHD_SPA_EVENTS` die Sportreferenzen `ID_SPORTTYPE`, `ID_SPORTGROUP` und `ID_SPORTSUBGROUP` am Event führt.

---

### 8.2 Charge ↔ Invoice

Charge beschreibt Gebühren und Gebühreninformationen.

Invoice beschreibt Rechnungen und Zahlstatus.

| Objekt | Domäne |
|---|---|
| Charge | Charge |
| ChargeType | Charge |
| ChargeGroup | Charge |
| EventCharge | Charge |
| InvoiceChargeInfo | Charge / Rechnungsbezug, fachlich Gebühreninformation |
| Invoice | Invoice |
| PaymentStatus | Invoice |
| SapStatus | Invoice |
| Rechnungs-PDF | Document mit Bezug zu Invoice |

Die Gebührenberechnung erfolgt über `PA_LHD_SPA.p_get_charges` und wird nicht in Invoice oder Portal neu umgesetzt.

---

### 8.3 Upload ↔ Document

Upload nimmt Dateien entgegen, prüft sie und ordnet sie fachlich zu.

Document verwaltet Dokumente dauerhaft.

| Objekt | Domäne |
|---|---|
| Upload | Upload |
| UploadFile | Upload |
| UploadValidationResult | Upload |
| UploadAssignment | Upload |
| Document | Document |
| DocumentContent | Document |
| DocumentType | Document |
| DocumentTemplate | Document |

---

## 9 Ergebnis dieses Übersichtsabschnitts

Die fachliche Datenmodellierung ist domänenorientiert aufgebaut.

Die zentralen Bestandsbereiche bleiben in Oracle und PL/SQL führend.

Neue Portaldomänen ergänzen den Bestand, ersetzen ihn aber nicht.

---

## 10 Domänen → Oracle-Tabellen

Die folgende Matrix beschreibt die fachliche Zuordnung der zentralen Oracle-Tabellen zu den Domänen.

Sie ist keine vollständige Tabelleninventur. Vollständige Details folgen in `Tabellen.md`.

| Domäne | Zentrale Oracle-Tabellen | Fachliche Bedeutung |
|---|---|---|
| Booking | `LHD_SPA_EVENTS`, `LHD_SPA_EVENTS_HIST`, `LHD_SPA_EVENTTYPES`, `LHD_SPA_EVENTCLASSES`, `LHD_SPA_EVENT2UNIT`, `LHD_SPA_BOOKING_NUMBERS`, `LHD_SPA_RECURRING_PATTERN` | Event, Buchung, Eventtyp, Eventklasse, Teileinheitszuordnung, Buchungsnummer, Wiederholung |
| Booking | `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER`, `LHD_SPA_OCC_DAY_COVERAGE`, `LHD_SPA_OCC_EVENT_DRT`, `LHD_SPA_OCC_WINNER_DRT` | konkrete Vorkommen, gültige Belegung, Aktualisierungs- und technische Dirty-Strukturen |
| Booking | `LHD_SPA_SPORTCATEGORIES`, `LHD_SPA_SPORTGROUPS`, `LHD_SPA_SPORTSUBGROUPS`, `LHD_SPA_SPORTTYPES` | Sportreferenzdaten am Event / an der Buchung |
| Facility | `LHD_SPA_SPORTSCOMPLEXES`, `LHD_SPA_FACILITY2COMPLEX`, `LHD_SPA_FACILITYGROUPS` | Sportkomplexe, Zuordnung Sportanlage zu Komplex, Sportanlagengruppen |
| Facility | `LHD_SPA_EVENTS`, `LHD_SPA_EVENT2UNIT`, `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER` | Facility-Bezug über `ID_SPA`, `SPA_NR`, `UNIT_ID`; fachliche Hauptverantwortung für Events bleibt Booking |
| Availability | `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER`, `LHD_SPA_OCC_DAY_COVERAGE` | Grundlage für freie Zeiten, Kalender und Konfliktprüfung |
| Charge | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS`, `LHD_SPA_CHARGE2FACILITYGROUP` | Gebührenstammdaten, Typen, Gruppen und FacilityGroup-Zuordnung |
| Charge | `LHD_SPA_EVENTCHARGES`, `LHD_SPA_EVENTCHARGES_HIST`, `LHD_SPA_INVOICE_CHARGEINFOS`, `LHD_SPA_INVOICE_CHARGEINFOS_2025` | Buchungsgebühren, Historie und rechnungsrelevante Gebühreninformationen |
| Invoice | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICES_HIST`, `LHD_SPA_INVOICE_CHARGEINFOS` | Rechnungen, Historie, Anzeige von Rechnungspositionen über ChargeInfos |
| Document | `LHD_SPA_DOCUMENTS`, `LHD_SPA_DOCUMENTS_EVENTS`, `LHD_SPA_DOCUMENT_NUMBERS`, `LHD_SPA_DOCUMENT_TEMPLATES`, `LHD_SPA_DOCUMENT_TEXTMODULES`, `LHD_SPA_DOCUMENT_TYPES` | Dokumente, Event-Zuordnung, Nummernkreise, Vorlagen, Textbausteine, Dokumenttypen |
| Upload | neue / zu prüfende Upload-Tabellen laut Domäne Upload | technische Dateiannahme, Validierung, Zuordnung; finale Persistenz abhängig von Speicherstrategie |
| Application | neue / zu definierende Antragstabellen laut Domäne Application | Onlineanträge, Entwürfe, Payload, Status, Zuordnungen |
| Wizard | neue / zu definierende Wizard-Konfigurationstabellen laut Domäne Wizard | Schritt-, Feld-, Validierungs- und Pflichtanlagenkonfiguration |
| Workflow | neue / zu definierende Workflow-Tabellen laut Domäne Workflow | Vorgänge, Aufgaben, Rückfragen, Statusübergänge |
| Authentication | neue / zu definierende Identity-/Auth-Tabellen laut Domäne Authentication | technische Portalidentität, Login, Token, Sperre |
| PortalUser | neue / zu definierende PortalUser-Tabellen laut Domäne PortalUser | fachliches Benutzerprofil, Präferenzen, Zustimmungen |
| Organisation | neue / zu definierende Organisations- und Mitgliedschaftstabellen laut Domäne Organisation | Organisationen, Abteilungen, Mitgliedschaften, Rollenbezug |
| Context | neue / abgeleitete Kontextstrukturen laut Domäne Context | aktiver Sichtbarkeits- und Arbeitskontext |
| Notification | neue / zu definierende Notification-Tabellen laut Domäne Notification | Portalnachrichten, Empfänger, MailQueue, Versandstatus |
| Dashboard | keine eigene fachliche Persistenz V1 | reine Aggregation aus anderen Domänen |
| Administration | neue / zu prüfende Konfigurations- und Auditstrukturen | administrative Einstellungen, Protokollierung, Verwaltungssichten |

---

## 11 Domänen → PL/SQL-Packages

Die folgende Matrix beschreibt die zentrale Package-Verantwortung aus fachlicher Sicht.

| Domäne | Package / Funktion | Bedeutung |
|---|---|---|
| Booking | `PA_LHD_SPA` | bestehende Buchungs-, Wiederholungs-, Stornierungs- und weitere SportFM-Bestandslogik |
| Booking | `PA_LHD_SPA_OCC` | Occurrence- und Winner-Ermittlung, performante Termin- und Belegungszugriffe |
| Availability | `PA_LHD_SPA_OCC` | führende Grundlage für Belegung und freie Zeiten |
| Charge | `PA_LHD_SPA.p_get_charges` | führende Gebührenberechnung |
| Charge | `PA_LHD_SPA` | bestehende Gebühren- und Buchungslogik im Bestand |
| Invoice | bestehende Rechnungs-/SAP-Packages, noch final zu identifizieren | Anzeige und Status bleiben Bestand; keine SAP-Neuimplementierung |
| Document | bestehende Dokumentenpackages, noch final zu identifizieren | Dokumentliste, Details, Download und Nummernkreis über Bestand oder Kapselung |
| Facility | bestehende Facility-/Stammdatenpackages, noch final zu identifizieren | Sportstätten, Teileinheiten, Sportkomplexe, FacilityGroups |
| Application | neue REST-/Service-Schicht, kein bestätigtes Bestandspackage | Antrag ist neue Portaldomäne |
| Wizard | neue REST-/Service-Schicht, kein bestätigtes Bestandspackage | Wizard-Konfiguration ist neue Portaldomäne |
| Workflow | neue REST-/Service-Schicht, kein bestätigtes Bestandspackage | Workflow ist neue / erweiterte Portaldomäne |
| Upload | neue REST-/Service-Schicht, kein bestätigtes Bestandspackage | Upload ist neue Plattformdomäne |
| Authentication | neue Auth-Schicht | technische Identität und Zugriff |
| PortalUser | neue REST-/Service-Schicht | fachliches Benutzerprofil |
| Organisation | neue REST-/Service-Schicht | Organisationen und Mitgliedschaften |
| Context | neue REST-/Service-Schicht | Sichtbarkeits- und Arbeitskontext |
| Notification | neue REST-/Service-Schicht | Portalnachrichten und MailQueue |
| Dashboard | kein eigenes Package V1 | Aggregation vorhandener Services |
| Administration | neue / zu prüfende Admin-Kapselungen | Verwaltung und Konfiguration |

### 11.1 Zielprinzip für Package-Nutzung

Die .NET-Schicht ruft Packages über Repository- oder Gateway-Klassen auf.

Die Domänenlogik entscheidet fachlich, **welche** Operation benötigt wird.

Das Repository / Gateway kapselt, **wie** Oracle oder PL/SQL aufgerufen wird.

```text
REST Controller
  ↓
Application / Domain Service
  ↓
Repository / Oracle Gateway
  ↓
PL/SQL Package / Oracle Tabelle
```

---

## 12 Domänen → REST-Verantwortung

Die REST-Schicht bildet fachliche Operationen ab und trennt konsequent zwischen Bestandsfreilegung und neuen Portalfunktionen.

| Domäne | REST-Verantwortung | Charakter |
|---|---|---|
| Authentication | Login, Registrierung, Token, Passwortprozesse | neu |
| PortalUser | Profil, Einstellungen, Favoriten, Zustimmungen | neu |
| Organisation | Organisationen, Mitgliedschaften, Rollenbezug | neu / erweitert |
| Context | verfügbare Kontexte, aktiver Kontext | neu |
| Application | Anträge, Entwürfe, Einreichung, Status | neu |
| Wizard | Wizarddefinitionen, Schritte, Felder, Validierungen | neu |
| Workflow | Aufgaben, Rückfragen, Entscheidungen, Status | neu / erweitert |
| Upload | Datei hochladen, prüfen, zuordnen | neu |
| Facility | Sportanlagen, Teileinheiten, Sportkomplexe, FacilityGroups | lesende Bestandsfreilegung |
| Availability | freie Zeiten, Kalender, Konflikte | lesende / prüfende Bestandsfreilegung |
| Booking | Buchungen, Occurrences, Kalender, Sportreferenzen | lesende Bestandsfreilegung V1 |
| Charge | Gebührenstammdaten, EventCharges, InvoiceChargeInfos, Gebührenhinweis | lesende Bestandsfreilegung / Vorschau |
| Invoice | Rechnungsliste, Details, Zahlstatus, PDF-Bezug | lesende Bestandsfreilegung |
| Document | Dokumentliste, Detail, Download, Dokumenttypen | lesende Bestandsfreilegung / Upload-Übernahme |
| Notification | Nachrichten, ungelesene Anzahl, MailQueue, Präferenzen | neu |
| Dashboard | aggregierte Startseite | neu, keine eigene Persistenz |
| Administration | administrative Sichten, Konfiguration, Audit | neu / erweitert |

---

## 13 Fachliche Identitäten und Schlüsselfelder

Diese Übersicht nennt nur fachlich relevante Identitäten. Die vollständige technische Schlüsselbeschreibung folgt in `Tabellen.md`.

| Bereich | Fachliche Identität | Quelle / Bezug |
|---|---|---|
| Booking | `ID_EVENT` | zentrale Event-/Buchungsidentität in `LHD_SPA_EVENTS` |
| Booking | `BOOKING_NUMBER` | fachliche Buchungsnummer |
| Booking | `ID_BOOKING`, `BOOKING_COUNTER`, `YEAR` | Buchungsnummern- und Jahresbezug |
| Booking | `EVENT_ID` | Occurrence-Bezug in `LHD_SPA_OCC` |
| Booking | `WINNER_ID` / `OCC_ID` | Winner-/Occurrence-Bezug |
| Facility | `ID_SPA`, `SPA_NR` | Sportanlagenbezug aus Event / Occurrence |
| Facility | `ID_UNIT`, `UNIT_ID` | Teileinheitsbezug aus Event2Unit / Occurrence |
| Facility | `ID_FACILITYGROUP` | Sportanlagengruppe / Gebührenbezug |
| Charge | `ID_CHARGE` | Gebührenstammsatz |
| Charge | `ID_CHARGETYPE` | Gebührentyp |
| Charge | `ID_CHARGEGROUP` | Gebührengruppe |
| Charge | `ID_EVENT` + `ID_CHARGE` | EventCharge-Bezug |
| Invoice | `ID_INVOICE` | Rechnung und Rechnungsbezug |
| Document | `ID_DOCUMENT_TYPE`, `DOCUMENT_NUMBER` | Dokumenttyp und Dokumentnummer |
| Document | `ID_INVOICE` | Dokumentbezug zur Rechnung |
| Application | `applicationId` | neue Portalidentität, final im Datenmodell zu definieren |
| Workflow | `workflowInstanceId`, `taskId` | neue Portalidentitäten, final im Datenmodell zu definieren |
| Upload | `uploadId` | neue Portalidentität, final im Datenmodell zu definieren |
| PortalUser | `portalUserId`, `identityId` | neue Portalidentität und technische Identitätsverknüpfung |
| Context | `contextId` | neue / abgeleitete Kontextidentität |

---

## 14 Fachliche Referenzdaten

Referenzdaten werden fachlich einer verantwortlichen Domäne zugeordnet.

| Referenzdaten | Verantwortliche Domäne | Oracle-Bezug |
|---|---|---|
| Eventtypen | Booking | `LHD_SPA_EVENTTYPES` |
| Eventklassen | Booking | `LHD_SPA_EVENTCLASSES` |
| Sportarten | Booking | `LHD_SPA_SPORTTYPES` |
| Sportgruppen | Booking | `LHD_SPA_SPORTGROUPS` |
| Sportuntergruppen | Booking | `LHD_SPA_SPORTSUBGROUPS` |
| Sportkategorien | Booking | `LHD_SPA_SPORTCATEGORIES` |
| FacilityGroups | Facility / Charge-Bezug | `LHD_SPA_FACILITYGROUPS` |
| ChargeTypes | Charge | `LHD_SPA_CHARGETYPES` |
| ChargeGroups | Charge | `LHD_SPA_CHARGEGROUPS` |
| DocumentTypes | Document | `LHD_SPA_DOCUMENT_TYPES` |
| WizardSteps / Fields | Wizard | neue / zu definierende Portalreferenzdaten |
| NotificationCategories | Notification | neue / zu definierende Portalreferenzdaten |
| Rollen / Rechte | Authentication / Organisation / Context / Administration | neue / zu definierende Portalreferenzdaten |

---

## 15 Auswirkungen auf nachfolgende Kapitel

### 15.1 Auswirkungen auf `Oracle_Datenmodell.md`

`Oracle_Datenmodell.md` muss die hier genannten fachlichen Datenbereiche technisch vertiefen.

Dort sind insbesondere zu dokumentieren:

- Tabellenbereiche,
- technische Hilfstabellen,
- Historien,
- Dirty-Tabellen,
- Trigger, falls fachlich relevant,
- Sequenzen und Nummernkreise,
- Views, falls vorhanden,
- technische Abhängigkeiten.

### 15.2 Auswirkungen auf `Tabellen.md`

`Tabellen.md` muss jede relevante Tabelle mit mindestens folgenden Angaben dokumentieren:

- verantwortliche Domäne,
- fachlicher Zweck,
- wichtigste Schlüssel,
- wichtigste Attribute,
- Beziehung zu anderen Tabellen,
- verwendende REST-Funktionen,
- verwendende Packages,
- Migrationsrelevanz.

### 15.3 Auswirkungen auf `Packages.md`

`Packages.md` muss mindestens folgende Bestandslogik vertiefen:

- `PA_LHD_SPA`,
- `PA_LHD_SPA_OCC`,
- `PA_LHD_SPA.p_get_charges`,
- identifizierte Dokumentenpackages,
- identifizierte Rechnungs-/SAP-Packages,
- identifizierte Facility-/Stammdatenpackages.

### 15.4 Auswirkungen auf `04_REST_API`

Die REST-API muss die fachlichen Domänen abbilden und darf keine Tabellen-API werden.

Die REST-Struktur muss deshalb den Domänen folgen:

```text
/api/v1/bookings
/api/v1/facilities
/api/v1/charges
/api/v1/invoices
/api/v1/documents
/api/v1/applications
/api/v1/workflow
```

### 15.5 Auswirkungen auf `06_Arbeitspakete` und `07_Kalkulation`

Die Arbeitspakete und die Kalkulation müssen zwischen folgenden Aufwandsarten unterscheiden:

| Art | Bedeutung |
|---|---|
| Bestandsfreilegung | Oracle-/PLSQL-Bestand fachlich über REST verfügbar machen |
| Neuentwicklung | neue Portal- und Plattformfunktionen entwickeln |
| Integration | Domänen verbinden, z. B. Booking ↔ Charge ↔ Invoice |
| Migration | WPF und Bestand schrittweise an REST und Portal anbinden |
| Tests | Regression gegen Bestand und neue Portalfunktionen |

---

## 16 Ergebnis dieses Bearbeitungsstands

Mit diesem Abschnitt sind die zentralen Mapping-Sichten ergänzt:

- Domäne → Oracle-Tabellen,
- Domäne → PL/SQL-Packages,
- Domäne → REST-Verantwortung,
- fachliche Identitäten,
- Referenzdaten,
- Auswirkungen auf die Folgedokumente.

Die noch zu ergänzenden Abschnitte dieser Datei sind:

- Datenflüsse,
- Lebenszyklen,
- Traceability-Matrix,
- Risiken,
- offene Punkte,
- Änderungsauswirkungen,
- Abschlussbewertung.
