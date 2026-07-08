# Domäne Application

| Feld | Wert |
|---|---|
| Kapitel | 03 – Domänen |
| Dokument | Application |
| Status | Konsolidierter Arbeitsstand |
| Typ | Neuentwicklung |
| Priorität | Sehr hoch |
| Leitquellen | `Quellen/2026-07-05_Snapshot1.txt`, `Quellen/2026-05_28_Lastenheft_SportFM.pdf` |

---

## 1 Zweck

Die Domäne **Application** beschreibt die digitale Antragstellung im SportFM-Portal.

Sie umfasst den fachlichen Lebenszyklus eines Nutzungsantrags von der Anliegensklärung über die Erfassung, Validierung, Speicherung als Entwurf und Einreichung bis zur Übergabe an Workflow und SportFM.

Application ist eine neue Plattformdomäne.

---

## 2 Projektbewertung

| Bereich | Bestand | Erweiterung | Neuentwicklung | Bewertung |
|---|:---:|:---:|:---:|---|
| Oracle |  | x |  | Erweiterung erforderlich |
| PL/SQL |  | x |  | Erweiterung / Kapselung erforderlich |
| REST |  |  | x | neue fachliche API |
| DTO |  |  | x | neue Vertragsobjekte |
| Portal |  |  | x | neue Seiten und Komponenten |
| Workflow |  | x |  | Anbindung an Workflow erforderlich |
| Wizard |  | x |  | Wizard als Eingabeoberfläche |
| Tests |  |  | x | neue Tests erforderlich |

---

## 3 Abgrenzung

### 3.1 Verantwortlich

Application ist verantwortlich für:

- Anliegensklärung,
- Auswahl des Antragstyps,
- Anlegen eines Antrags,
- Speichern von Entwürfen,
- Bearbeitung von Entwürfen,
- Zuordnung von Anlagen,
- fachliche Vorvalidierung,
- Einreichung,
- Start des Workflows,
- Anzeige des Antragsstatus,
- Historisierung der Antragsschritte.

### 3.2 Nicht verantwortlich

Application ist nicht verantwortlich für:

- Buchungslogik,
- Belegungsplanlogik,
- Gebührenberechnung,
- Rechnungserstellung,
- Dokumentengenerierung,
- technische Dateispeicherung,
- Benutzerverwaltung,
- Rollenverwaltung,
- Bearbeitung von Vorgängen, die laut Lastenheft nicht über SportFM abgewickelt werden.

Diese Verantwortlichkeiten verbleiben in den jeweils bestehenden oder eigenen Domänen.

---

## 4 Einordnung in die Plattform

```text
Portal
  ↓
Dashboard
  ↓
Application
  ↓
Workflow
  ↓
SportFM-Fachlogik
  ↓
Booking / Document / Charge / Invoice
```

Application ist die führende Domäne für die Antragserstellung, aber nicht für die spätere fachliche Buchung.

---

## 5 Fachlicher Ablauf

### 5.1 Ablaufübersicht

```text
Portalnutzer meldet sich an
  ↓
SportFM-Kontext auswählen
  ↓
Anliegensklärung auswählen
  ↓
Antragstyp bestimmen
  ↓
Antrag als Entwurf anlegen
  ↓
Wizard-Daten erfassen
  ↓
Anlagen hochladen
  ↓
Antrag validieren
  ↓
Antrag einreichen
  ↓
Workflow starten
  ↓
Antrag im SportFM-Arbeitskorb bearbeiten
```

### 5.2 Anliegensklärung

Vor der eigentlichen Antragstellung wird eine Anliegensklärung durchgeführt.

Aus den Quellen ergeben sich mindestens folgende Anliegen:

- Training / Übungsbelegung,
- Wettkämpfe und Veranstaltungen,
- Schulbetrieb,
- Einzelnutzung,
- Mehrfachnutzung.

Die finale fachliche Liste der Anliegen ist im Pflichtenheft zu bestätigen.

### 5.3 Antragstypen V1

Die Antragstypen orientieren sich an den im Lastenheft und den Anlagen genannten Nutzungsanträgen, soweit diese über SportFM abgewickelt werden.

Für V1 sind insbesondere relevant:

- Trainingszeiten auf Sportanlagen ohne Schulsportanlagen,
- Wettkämpfe und Veranstaltungen auf Sportanlagen ohne Schulsportanlagen,
- Trainingszeiten im Eissport- und Ballspielzentrum,
- Wettkämpfe und Veranstaltungen im Eissport- und Ballspielzentrum,
- Trainingszeiten auf Schulsportanlagen,
- Wettkämpfe und Veranstaltungen auf Schulsportanlagen,
- Einzelveranstaltungen auf Sportanlagen einschließlich Schulsportanlagen.

### 5.4 Nicht Bestandteil der Application-Domäne

Der **Antrag auf Anmietung Dritter** wird nicht als Application-V1-Antragstyp geführt.

Begründung:

- Nach unserer fachlichen Korrektur wird dieser Antrag nicht über SportFM bearbeitet.
- Er darf deshalb nicht als SportFM-Application-Antragstyp kalkuliert oder technisch vorgesehen werden.
- Falls später eine separate externe Antragsstrecke erforderlich wird, ist diese als eigene Klärung außerhalb der aktuellen SportFM-Application-Domäne zu behandeln.

---

## 6 Statusmodell

Application verwaltet den Status der Antragstellung bis zur Übergabe an Workflow.

| Status | Bedeutung | Bearbeitung durch Portalnutzer |
|---|---|:---:|
| `DRAFT` | Entwurf | ja |
| `VALIDATION_FAILED` | Validierung fehlgeschlagen | ja |
| `READY_TO_SUBMIT` | bereit zur Einreichung | ja |
| `SUBMITTED` | eingereicht | nein |
| `IN_WORKFLOW` | in Bearbeitung | nein |
| `QUERY` | Rückfrage | eingeschränkt |
| `WITHDRAWN` | zurückgezogen | nein |
| `CLOSED` | abgeschlossen | nein |

Hinweis: Die technische Codierung der Statuswerte ist im Datenmodell final festzulegen. Dieses Kapitel beschreibt die fachliche Bedeutung.

---

## 7 Business Objects

| Objekt | Zweck | Persistenz |
|---|---|---|
| `Application` | fachlicher Antrag | neue oder erweiterte Persistenz |
| `ApplicationType` | Antragstyp | Konfiguration |
| `ApplicationDraft` | Entwurfsdaten | neue Persistenz |
| `ApplicationPayload` | strukturierte Antragsdaten | neue Persistenz |
| `ApplicationAttachment` | Verweis auf Anlagen | Upload / Document |
| `ApplicationSubmission` | Einreichvorgang | neue Persistenz |
| `ApplicationHistory` | Historie | neue Persistenz / Audit |
| `ApplicationValidationResult` | Validierungsergebnis | transient / optional persistiert |

---

## 8 Fachliche Regeln

| ID | Regel |
|---|---|
| APP-BR-001 | Ohne aktiven SportFM-Kontext darf kein Antrag angelegt werden. |
| APP-BR-002 | Ohne Anliegensklärung darf kein Antragstyp ausgewählt werden. |
| APP-BR-003 | Ein Antrag beginnt immer als Entwurf. |
| APP-BR-004 | Entwürfe starten keinen Workflow. |
| APP-BR-005 | Nur vollständige und gültige Anträge dürfen eingereicht werden. |
| APP-BR-006 | Nach Einreichung ist eine Bearbeitung durch den Portalnutzer nur noch über definierte Rückfrageprozesse zulässig. |
| APP-BR-007 | Anlagen werden nicht in Application gespeichert, sondern über Upload / Document referenziert. |
| APP-BR-008 | Application erzeugt keine Buchung. |
| APP-BR-009 | Application löst beim Einreichen den Workflow aus. |
| APP-BR-010 | Jede Statusänderung wird historisiert. |
| APP-BR-011 | Antragstypen, die nicht über SportFM bearbeitet werden, sind nicht Bestandteil der Application-Domäne. |

---

## 9 REST-API

Die verbindliche REST-Ausarbeitung erfolgt in `04_REST_API/Endpunkte/Application.md` und `04_REST_API/Endpunkte.md`.

| ID | Methode | Pfad | Zweck |
|---|---|---|---|
| APP-API-001 | `GET` | `/applications/types` | verfügbare Antragstypen lesen |
| APP-API-002 | `POST` | `/applications` | Antrag als Entwurf anlegen |
| APP-API-003 | `GET` | `/applications/{id}` | Antrag lesen |
| APP-API-004 | `GET` | `/applications` | eigene Anträge lesen |
| APP-API-005 | `PUT` | `/applications/{id}` | Entwurf speichern |
| APP-API-006 | `POST` | `/applications/{id}/files` | Anlage zuordnen |
| APP-API-007 | `POST` | `/applications/{id}/submit` | Antrag einreichen |
| APP-API-008 | `GET` | `/applications/{id}/history` | Historie lesen |

REST-Endpunkte bilden fachliche Aktionen ab. Es entsteht keine Tabellen-CRUD-API.

---

## 10 DTOs

Die verbindliche DTO-Grundstruktur wird in `04_REST_API/DTOs.md` beschrieben.

Für Application sind insbesondere relevant:

- `AntragTypDto`,
- `AntragListeDto`,
- `AntragDetailDto`,
- `AntragSpeichernAnfrageDto`,
- `AntragEinreichenErgebnisDto`.

Die DTOs bilden keine Oracle-Tabellen ab, sondern fachliche Sichtweisen.

---

## 11 Services

### 11.1 ApplicationService

Verantwortung:

- Antrag anlegen,
- Antrag laden,
- Entwurf speichern,
- Antrag zurückziehen, soweit zulässig,
- Status prüfen,
- Historie schreiben.

Nicht verantwortlich für:

- Dateispeicherung,
- Workflowausführung,
- Buchung,
- Rechnung.

### 11.2 ApplicationValidationService

Verantwortung:

- Pflichtfeldprüfung,
- fachliche Konsistenzprüfung,
- Prüfung Kontext,
- Prüfung Antragstyp,
- Prüfung Anlagenvollständigkeit.

### 11.3 ApplicationSubmissionService

Verantwortung:

- Gesamtvalidierung,
- Statuswechsel zur Einreichung,
- Workflowstart,
- Audit / Historie,
- Notification auslösen.

### 11.4 ApplicationAttachmentService

Verantwortung:

- Upload-Domäne aufrufen,
- Anlage fachlich zuordnen,
- Entfernen aus Entwurf erlauben,
- keine physische Dateiverwaltung.

---

## 12 Repository

Das ApplicationRepository ist für Persistenzzugriffe zuständig.

Aufgaben:

- Antrag speichern,
- Antrag lesen,
- Antragsliste lesen,
- Payload speichern,
- Historie speichern,
- Anlagenzuordnung speichern.

Das Repository enthält keine Geschäftslogik.

---

## 13 Oracle und PL/SQL

Application erweitert das bestehende SportFM-System, ersetzt aber keine bestehende Buchungs-, Gebühren-, Rechnungs- oder Dokumentenlogik.

Aus den vorhandenen Quellen ergeben sich relevante bestehende Bereiche:

| Bereich | bestehende Quellen / Objekte |
|---|---|
| Buchung | `LHD_SPA_BOOKING_NUMBERS`, `LHD_SPA_OCC*`, `PA_LHD_SPA`, `PA_LHD_SPA_OCC` |
| Dokumente | `LHD_SPA_DOCUMENTS*`, `LHD_SPA_DOCUMENT_TYPES`, `LHD_SPA_DOCUMENT_TEMPLATES` |
| Gebühren | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS`, `PA_LHD_SPA.p_get_charges` |
| Rechnungen | `LHD_SPA_INVOICES*`, `LHD_SPA_INVOICE_CHARGEINFOS*` |
| Logging | `LHD_SPA_LOGGING` |

### 13.1 Neue / zu prüfende Application-Persistenz

Die Quellen belegen kein vorhandenes vollständiges Datenmodell für digitale Antragsentwürfe. Daher sind neue oder erweiterte Persistenzobjekte zu prüfen:

| Objekt | Zweck | Status |
|---|---|---|
| `LHD_SPA_PORT_APPLICATION` | Antragskopf | vorgesehen |
| `LHD_SPA_PORT_APPLICATION_DATA` | strukturierte Antragsdaten | vorgesehen |
| `LHD_SPA_PORT_APPLICATION_FILE` | Anlagenzuordnung | vorgesehen |
| `LHD_SPA_PORT_APPLICATION_HISTORY` | Historie | vorgesehen |

Die genaue Tabellenstruktur wird in `05_Datenmodell` dokumentiert.

### 13.2 Package-Zuordnung

| Package | Zweck | Status |
|---|---|---|
| `PA_LHD_SPA_PORT_APPLICATION` | Application-Funktionen | Zielstruktur, zu spezifizieren |
| `PA_LHD_SPA_PORT_WORKFLOW` | Workflowstart / Status | abhängig von Workflow-Domäne |
| bestehende SportFM-Packages | Buchung, Dokumente, Rechnungen, Gebühren | weiterverwenden |

---

## 14 Blazor-Frontend

| Seite | Route | Zweck |
|---|---|---|
| Antragstyp auswählen | `/application/new` | Auswahl Anliegen / Antragstyp |
| Antrag bearbeiten | `/applications/{id}/wizard` | Wizard-geführte Erfassung |
| Eigene Anträge | `/applications` | Liste eigener Anträge |
| Antrag anzeigen | `/applications/{id}` | Detailansicht |
| Antragshistorie | `/applications/{id}/history` | Historie / Protokoll |

---

## 15 Berechtigungen

| Berechtigung | Zweck |
|---|---|
| `Application.Read` | Antrag lesen |
| `Application.Create` | Antrag anlegen |
| `Application.UpdateDraft` | Entwurf bearbeiten |
| `Application.DeleteDraft` | Entwurf löschen |
| `Application.Submit` | Antrag einreichen |
| `Application.Withdraw` | Antrag zurückziehen |
| `Application.History.Read` | Historie lesen |

Berechtigungen sind immer kontextbezogen zu prüfen.

---

## 16 Validierungen

| ID | Validierung | Ebene |
|---|---|---|
| APP-VAL-001 | aktiver Kontext vorhanden | Application |
| APP-VAL-002 | Benutzer darf Antrag im Kontext erstellen | Application |
| APP-VAL-003 | Antragstyp aktiv | Application |
| APP-VAL-004 | Antragstyp gehört zum SportFM-V1-Umfang | Application |
| APP-VAL-005 | Pflichtfelder vollständig | Wizard / Application |
| APP-VAL-006 | Pflichtanlagen vorhanden | Application / Upload |
| APP-VAL-007 | Datenschutz / Zustimmung bestätigt | Application |
| APP-VAL-008 | Antrag im einreichbaren Status | Application |
| APP-VAL-009 | Workflowstart möglich | Workflow |

---

## 17 Testfälle

| Testfall | Beschreibung |
|---|---|
| TF-APP-001 | Antragstypen laden |
| TF-APP-002 | Antrag als Entwurf anlegen |
| TF-APP-003 | Entwurf speichern |
| TF-APP-004 | Pflichtfeldvalidierung schlägt fehl |
| TF-APP-005 | Pflichtanlagen fehlen |
| TF-APP-006 | Antrag erfolgreich einreichen |
| TF-APP-007 | Workflow wird gestartet |
| TF-APP-008 | Antrag nach Einreichung nicht mehr als Entwurf änderbar |
| TF-APP-009 | Zugriff auf fremden Kontext wird verhindert |
| TF-APP-010 | Historie wird geschrieben |
| TF-APP-011 | Antragstyp außerhalb SportFM-V1 wird nicht angeboten |

---

## 18 Arbeitspakete

| AP | Titel | Inhalt |
|---|---|---|
| AP-APP-001 | Domänenmodell | Business Objects, Status, Regeln |
| AP-APP-002 | Oracle-Konzept | Tabellenprüfung, neue Tabellen, Package-Zuordnung |
| AP-APP-003 | REST | Controller, DTOs, Fehlerformat |
| AP-APP-004 | Services | ApplicationService, ValidationService, SubmissionService |
| AP-APP-005 | Repository | Oracle-Zugriff, Mapping |
| AP-APP-006 | Wizard-Anbindung | WizardHost, Payload, Validierung |
| AP-APP-007 | Upload-Anbindung | Anlagen, Pflichtanlagen, Entfernen im Entwurf |
| AP-APP-008 | Workflow-Anbindung | Submit, Workflowstart, Rückfrage |
| AP-APP-009 | Portal | Seiten, Komponenten, UX |
| AP-APP-010 | Tests | Unit-, Integrations- und UI-Tests |

---

## 19 Risiken

| Risiko | Bewertung | Maßnahme |
|---|---|---|
| Antragstypen nicht final geklärt | hoch | fachliche Klärung vor Kalkulation |
| Pflichtanlagen je Antragstyp unklar | hoch | Anlagenmatrix erstellen |
| Workflow-Regeln ändern sich | hoch | Workflow separat spezifizieren |
| bestehende SportFM-Übergabe unklar | hoch | Integrationsschnittstelle konkretisieren |
| Portal enthält versehentlich Fachlogik | hoch | REST- und Servicegrenzen strikt einhalten |
| Oracle-Zuordnung unvollständig | hoch | SQL-Quellen auswerten |
| nicht zuständige Antragstypen werden versehentlich kalkuliert | hoch | Antragstypenliste gegen Lastenheft und fachliche Korrekturen prüfen |

---

## 20 Offene Punkte

| ID | Offener Punkt | Relevanz |
|---|---|---|
| OP-APP-001 | finale Liste der Antragstypen V1 | hoch |
| OP-APP-002 | Zuordnung Antragstyp zu Wizard-Definition | hoch |
| OP-APP-003 | Pflichtanlagen je Antragstyp | hoch |
| OP-APP-004 | finale Statusliste im Zusammenspiel mit Workflow | hoch |
| OP-APP-005 | technische Übergabe an bestehenden SportFM-Arbeitskorb | hoch |
| OP-APP-006 | Löschregel für Entwürfe | mittel |
| OP-APP-007 | genaue Oracle-Tabellenstruktur Application | hoch |
| OP-APP-008 | finales Package-Konzept | hoch |

---

## 21 Traceability-Matrix

| Quelle | Funktion | REST | Service | UI | Test | AP |
|---|---|---|---|---|---|---|
| Lastenheft Onlineantrag | Antrag anlegen | APP-API-002 | ApplicationService | Antrag bearbeiten | TF-APP-002 | AP-APP-003/004 |
| Lastenheft Upload | Anlage hochladen | APP-API-006 | AttachmentService | Anlagenübersicht | TF-APP-005 | AP-APP-007 |
| Snapshot REST-Grundsätze | Einreichen | APP-API-007 | SubmissionService | Einreichen | TF-APP-006/007 | AP-APP-008 |
| Lastenheft Arbeitskorb | Übergabe Workflow/SportFM | intern | SubmissionService | Statusanzeige | TF-APP-007 | AP-APP-008 |
| Sicherheitsanforderungen | Kontextprüfung | alle | ApplicationService | alle Seiten | TF-APP-009 | AP-APP-004/010 |
| fachliche Korrektur | nicht über SportFM bearbeitete Anträge ausschließen | Antragstypenfilter | ApplicationValidationService | Antragstypauswahl | TF-APP-011 | AP-APP-001/003/010 |

---

## 22 Änderungsauswirkungen

Änderungen an `Application.md` wirken sich aus auf:

- `03_Domaenen/Workflow.md`,
- `03_Domaenen/Wizard.md`,
- `03_Domaenen/Upload.md`,
- `03_Domaenen/Document.md`,
- `04_REST_API/Endpunkte.md`,
- `04_REST_API/DTOs.md`,
- `05_Datenmodell/Tabellen.md`,
- `05_Datenmodell/Packages.md`,
- `06_Arbeitspakete/Arbeitspaketliste.md`,
- `07_Kalkulation/Aufwandsschaetzung.md`,
- `09_Testkonzept/Testfaelle.md`,
- `12_Offene_Punkte/Offene_Punkte.md`.

---

## 23 Ergebnis

Die Domäne Application ist als neue Plattformdomäne fachlich spezifiziert.

Die Korrektur ist berücksichtigt: Der **Antrag auf Anmietung Dritter** ist kein SportFM-Application-V1-Antragstyp.

Die konkrete Kalkulation bleibt abhängig von:

- finaler Antragstypenliste,
- finaler Wizard-Struktur,
- finaler Anlagenmatrix,
- bestätigtem Workflowmodell,
- bestätigter Oracle-Zuordnung.