# Tabellen

> Status: Arbeitsstand für das Projekthandbuch SportFM  
> Kapitel: `05_Datenmodell/Tabellen.md`  
> Grundlage: Snapshot vom 05.07.2026, Lastenheft SportFM, Oracle-DDLs aus `GIT_IMSP.zip`, Domänenkapitel, `Packages.md` und fachliche Klärungen.

---

## 1. Ziel des Kapitels

Dieses Kapitel beschreibt die für das Portalprojekt relevanten Tabellen des bestehenden SportFM-Datenmodells und die neu zu ergänzenden Portal-Tabellen.

Ziel ist nicht, eine vollständige physische Oracle-DDL zu ersetzen. Ziel ist eine fachliche und technische Arbeitsgrundlage für:

- Pflichtenheft,
- technisches Datenmodell,
- REST-API,
- PL/SQL-Packages,
- Arbeitspakete,
- Aufwandsschätzung,
- Migration,
- Tests.

Das Kapitel beantwortet insbesondere:

1. Welche bestehenden Tabellen bleiben führend?
2. Welche neuen Tabellen benötigt das Portal?
3. Welche Tabelle gehört zu welcher Domäne?
4. Welches Package ist fachlich für welche Tabelle verantwortlich?
5. Welche Beziehungen zum bestehenden SportFM-Modell entstehen?
6. Welche Tabellen sind bewusst nicht Bestandteil von V1?

---

## 2. Grundsatzentscheidungen

### 2.1 SportFM bleibt fachlich führend

Das bestehende SportFM-Datenmodell bleibt führend für:

- Sportstätten,
- Teileinheiten,
- Nutzer im Sinne von SportFM,
- Buchungen / Events,
- Wiederholungen,
- Occurrences,
- Winner-Logik,
- Gebühren,
- Rechnungen,
- Dokumente,
- SAP-Bezug.

Das Portal erzeugt keine zweite Buchungs-, Gebühren-, Rechnungs- oder Dokumentenwelt.

### 2.2 Portal-Tabellen ergänzen nur fehlende Portal-Fachlichkeit

Neue Tabellen entstehen nur dort, wo SportFM heute keine eigene Struktur besitzt:

- Portalbenutzer,
- Portalorganisationen,
- Abteilungen und Berechtigte,
- Kontextzuordnung Portal → SportFM-Nutzer,
- digitale Anträge,
- Workflow / Arbeitskorb,
- Upload-Metadaten,
- Benachrichtigungen,
- Audit / Revision,
- optionale Portal-Konfigurationen.

### 2.3 REST-API ist keine Tabellen-API

Die REST-API greift nicht direkt fachlich auf Tabellen zu. Die REST-API ruft fachliche Packages auf.

```text
REST Controller
  ↓
.NET Application Service
  ↓
Oracle Package
  ↓
Tabellen / bestehende Packages
```

### 2.4 Packages kapseln Tabellenzugriffe

Für jede Portal-Domäne gibt es ein eigenes Package. Dieses Package ist die fachliche Zugriffsschicht auf die dazugehörigen Tabellen.

Beispiel:

```text
LHD_SPA_PORT_APPLICATION
LHD_SPA_PORT_APPLICATION_HISTORY
LHD_SPA_PORT_APPLICATION_REF
        ↑
PA_LHD_SPA_PORT_APPLICATION
```

### 2.5 Keine zweite Fachlogik im Portal

Das Portal darf insbesondere nicht selbst berechnen:

- Wiederholungen,
- Occurrences,
- freie Zeiten,
- Winner,
- Gebühren,
- Stornierungen,
- Rechnungen.

Diese Logik verbleibt in vorhandenen oder erweiterten SportFM-Packages, insbesondere:

| Package | Status | Bedeutung |
|---|---|---|
| `PA_LHD_SPA` | Bestand | zentrale SportFM-Fachlogik, u. a. Wiederholungen, Stornierungen, Gebühren |
| `PA_LHD_SPA_OCC` | Bestand | Occurrence-Ermittlung, Winner-Logik, performanter Terminzugriff |

---

## 3. Namenskonvention

Alle neuen Datenbankobjekte führen die bestehende SportFM-Namenskonvention fort.

| Objekttyp | Konvention | Beispiel |
|---|---|---|
| Tabellen | `LHD_SPA_PORT_...` | `LHD_SPA_PORT_APPLICATION` |
| Packages | `PA_LHD_SPA_PORT_...` | `PA_LHD_SPA_PORT_APPLICATION` |
| Views | `VW_LHD_SPA_PORT_...` | `VW_LHD_SPA_PORT_APPLICATIONS` |
| Trigger | `TG_LHD_SPA_PORT_...` | `TG_LHD_SPA_PORT_APPLICATION_BI` |
| Sequenzen | `SEQ_LHD_SPA_PORT_...` | `SEQ_LHD_SPA_PORT_APPLICATION` |

### 3.1 Technische Standardfelder neuer Portal-Tabellen

Neue Portal-Tabellen erhalten, sofern fachlich sinnvoll, folgende Standardfelder.

| Feld | Typ / Prinzip | Bedeutung |
|---|---|---|
| `ID_<ENTITY>` | `NUMBER` | technischer Primärschlüssel |
| `STATE` | `NUMBER` | fachlicher Zustand, z. B. aktiv, gelöscht, gesperrt |
| `CREATED_AT` | `TIMESTAMP` | Anlagezeitpunkt |
| `CREATED_BY` | `NUMBER` / `VARCHAR2` | anlegender Benutzer / System |
| `UPDATED_AT` | `TIMESTAMP` | Änderungszeitpunkt |
| `UPDATED_BY` | `NUMBER` / `VARCHAR2` | ändernder Benutzer / System |
| `VERSION` | `NUMBER` | optimistische Sperre / fachliche Version |
| `CORRELATION_ID` | `VARCHAR2` | Korrelation REST → DB → Log, optional |

### 3.2 Fachliche Statuskonvention V1

Für neue Portal-Tabellen gilt als Basis:

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv / aktuell |
| `0` | inaktiv / noch nicht aktiviert |
| `-1` | gelöscht / fachlich entfernt |

Domänenspezifische Statuswerte werden je Tabelle zusätzlich beschrieben.

---

## 4. Bestehende SportFM-Tabellen

### 4.1 Booking / Events / Belegung

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_EVENTS` | Bestand | Zentrale Tabelle für alle belegungsrelevanten Vorgänge | `ID_EVENT`, `ID_PARENT_EVENT`, `ID_BOOKING`, `BOOKING_NUMBER`, `ID_SPA`, `ID_USER`, `ID_EVENTTYPE`, Zeitraum, Uhrzeit, Sportart, `STATE` |
| `LHD_SPA_EVENTS_HIST` | Bestand | technische / historische Änderungssicht zu Events | Bezug zu `LHD_SPA_EVENTS` |
| `LHD_SPA_EVENTTYPES` | Bestand | Belegungs- und Eventtypen | `ID_EVENTTYPE`, `ID_EVENTCLASS`, `DESCRIPTION`, `RECURRING_INTERVAL`, `PRIO`, `STATE` |
| `LHD_SPA_EVENTCLASSES` | Bestand | Klassen von Eventtypen | `ID_EVENTCLASS`, `DESCRIPTION`, `PRIO`, `IS_RECURRING` |
| `LHD_SPA_EVENT2UNIT` | Bestand | Zuordnung Event zu buchbaren Teileinheiten | `ID_EVENT`, `ID_UNIT` |
| `LHD_SPA_RECURRING_PATTERN` | Bestand | Wiederholungsmuster zu Events | `ID_EVENT`, `FREQ`, `INTERVAL`, `MAX_OCCURRENCES`, `DAYS`, `HOLIDAYS` |
| `LHD_SPA_BOOKING_NUMBERS` | Bestand | Zähler für Buchungsnummern je Jahr | `YEAR`, `LAST_ID_BOOKING` |
| `LHD_SPA_CONTRACT_IDS` | Bestand | Vertrags-/Kontextbezug Nutzer, Anlage, Vertrag | `ID_USER`, `ID_SPA`, `ID_CONTRACT` |

Fachliche Festlegungen:

- Sperrungen, Reinigung, Schulbetrieb, GTA, wöchentliche Übungsbelegung, 14-tägige Übungsbelegung, Veranstaltungen und Stornierungen werden als Events geführt.
- Eine Stornierung ist ein eigener Eventtyp und keine reine Statusänderung.
- `LHD_SPA_EVENTS` wird nicht direkt als REST-Ressource veröffentlicht.

### 4.2 Occurrence / Availability / Winner

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_OCC` | Bestand | materialisierte konkrete Belegungsvorkommen | `EVENT_ID`, `SPA_ID`, `UNIT_ID`, `START_TS`, `END_TS`, `DAY_DATE`, `USER_ID`, `EVENTTYPE_ID`, Prioritäten |
| `LHD_SPA_OCC_WINNER` | Bestand | resultierende gültige Belegung nach Winner-Logik | `WINNER_ID`, `SPA_ID`, `UNIT_ID`, `DAY_DATE`, `START_TS`, `END_TS`, `EVENT_ID`, `OCC_ID` |
| `LHD_SPA_OCC_DAY_COVERAGE` | Bestand | Kennzeichnung berechneter Tage je Sportanlage | `SPA_ID`, `DAY_DATE`, `BUILT_TS` |
| `LHD_SPA_OCC_EVENT_DRT` | Bestand | technische Dirty-/Retry-Tabelle für Event-Neuberechnung | `EVENT_ID`, `TRIES`, `LAST_ERROR`, `UPDATED_TS` |
| `LHD_SPA_OCC_WINNER_DRT` | Bestand | technische Dirty-/Retry-Tabelle für Winner-Neuberechnung | `SPA_ID`, `DAY_DATE`, `TRIES`, `LAST_ERROR`, `UPDATED_TS` |

Fachliche Festlegungen:

- Die Occurrence-/Winner-Logik ist vorhanden und umgesetzt.
- Es gibt einen stündlichen Job und einen Fast Path.
- Kalender und freie Zeiten müssen diese Strukturen nutzen.

### 4.3 Documents

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_DOCUMENTS` | Bestand | Dokumente zu Nutzern, Rechnungen und Vorgängen | `ID_DOCUMENT`, `ID_SPORTSUSER`, `ID_DOCUMENT_TYPE`, `ID_DOCUMENT_STATE`, `TITLE`, `FILENAME`, `MIMETYPE`, `CONTENT`, `ID_INVOICE` |
| `LHD_SPA_DOCUMENTS_EVENTS` | Bestand | Zuordnung Dokument zu Event | `ID_DOCUMENT`, `ID_EVENT` |
| `LHD_SPA_DOCUMENT_TYPES` | Bestand | Dokumenttypen | `ID_DOCUMENT_TYPE`, `TITLE`, `DESCRIPTION` |
| `LHD_SPA_DOCUMENT_TEMPLATES` | Bestand | Dokumentvorlagen | `ID_DOCUMENT_TEMPLATE`, `TITLE`, `FILENAME`, `MIMETYPE`, `CONTENT`, `ID_DOCUMENT_TYPE`, `DEPARTMENT` |
| `LHD_SPA_DOCUMENT_TEXTMODULES` | Bestand | Textmodule für Dokumente | `ID_DOCUMENT_TEXTMODULE`, `TITLE`, `ID_DOCUMENT_TYPE`, `CONTENT` |
| `LHD_SPA_DOCUMENT_NUMBERS` | Bestand | Dokumentnummernzähler je Jahr | `YEAR`, `LAST_DOCUMENT_NR` |
| `LHD_SPA_DOCUMENTS_RE_2025` | Bestand / Sonderbestand | Rechnungs-/Dokumentbestand 2025 | `ID_INVOICE`, `ID_DOCUMENT`, `DOCUMENT_NUMBER`, `STATE`, `CONTENT` |

Fachliche Festlegungen:

- Grundsätzlich sollen alle eigenen Dokumente im Portal sichtbar sein.
- Zugriff erfolgt streng kontextbezogen über PortalUser / Organisation / Context.
- Es entsteht keine zweite Dokumentenablage für SportFM-Dokumente.

### 4.4 Charges / Gebühren

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_CHARGES` | Bestand | Gebührenregeln / Gebührensätze | `ID_CHARGE`, `ID_CHARGEGROUP`, `VALID_FROM`, `VALID_UNTIL`, `AMOUNT`, `BRUTTO`, `VAT`, `ID_FACILITYGROUP`, Berechnungsfelder, Zeitfenster |
| `LHD_SPA_CHARGETYPES` | Bestand | Gebührentypen | `ID_CHARGETYPE`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_CHARGEGROUPS` | Bestand | Gebührengruppen | `ID_CHARGEGROUP`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_CHARGE2FACILITYGROUP` | Bestand | Zuordnung Gebühr zu Anlagengruppe | `ID_CHARGE`, `ID_FACILITYGROUP` |
| `LHD_SPA_EVENTCHARGES` | Bestand | Gebührenzuordnung zu Events | `ID_EVENT`, `ID_CHARGE`, Mengen-/Betragsinformationen |
| `LHD_SPA_EVENTCHARGES_HIST` | Bestand | Historie der Eventgebühren | Bezug zu `LHD_SPA_EVENTCHARGES` |

Fachliche Festlegungen:

- Gebührenberechnung bleibt in SportFM / Oracle.
- Das Portal kann Gebühreninformationen anzeigen, aber nicht eigenständig berechnen.

### 4.5 Invoices / Rechnungen

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_INVOICES` | Bestand | Rechnungskopf inkl. SAP-Bezug | `ID_INVOICE`, `ID_USER`, Zeitraum, `STATE`, SAP-Belegnummern, SAP-Storno-Felder, `REMARK` |
| `LHD_SPA_INVOICES_2025` | Bestand / Jahresbestand | Rechnungsbestand 2025 | analog `LHD_SPA_INVOICES` |
| `LHD_SPA_INVOICE_CHARGEINFOS` | Bestand | Rechnungspositionen / Gebühreninformationen | `ID_INVOICE`, Gebühren-, Zeitraum- und Betragsinformationen |
| `LHD_SPA_INVOICE_CHARGEINFOS_2025` | Bestand / Jahresbestand | Rechnungspositionen 2025 | analog `LHD_SPA_INVOICE_CHARGEINFOS` |
| `LHD_SPA_INVOICE_CI_HIST` | Bestand | Historie Rechnungspositionen | Bezug zu Rechnungspositionen |

Fachliche Festlegungen:

- Grundsätzlich sollen alle eigenen Rechnungen im Portal sichtbar sein.
- SAP-Anbindung besteht bereits über REST/OAuth und wird nicht neu modelliert.
- ePayBL wird als eigener Integrationsprozess betrachtet.

### 4.6 Facility / Sportstruktur / Stammdaten

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_SPORTSCOMPLEXES` | Bestand | Sportkomplexe | `ID_SPORTSCOMPLEX`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_FACILITY2COMPLEX` | Bestand | Zuordnung Sportanlage zu Sportkomplex | `ID_SPA`, `ID_SPORTSCOMPLEX` |
| `LHD_SPA_FACILITYGROUPS` | Bestand | Sportanlagengruppen | `ID_FACILITYGROUP`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_SPORTCATEGORIES` | Bestand | Sportkategorien | `ID_SPORTCATEGORY`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_SPORTGROUPS` | Bestand | Sportgruppen | `ID_SPORTGROUP`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_SPORTSUBGROUPS` | Bestand | Sportuntergruppen | `ID_SPORTSUBGROUP`, `DESCRIPTION`, `STATE` |
| `LHD_SPA_SPORTTYPES` | Bestand | Sportarten | `ID_SPORTTYPE`, `DESCRIPTION`, `ID_SPORTCATEGORY`, `IN_USE`, `STATE`, `LSB_NR_2019` |

Fachliche Festlegungen:

- Stammdaten zu Sportanlagen und Teileinheiten liegen führend in RIB FM / SportFM.
- `OBJECT.OBJ_ID` aus RIB FM bleibt für Sportanlage und Teileinheit relevant.
- Das Portal pflegt keine Sportstättenstammdaten in V1.

### 4.7 System / Verwaltung / Logging

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_LOGGING` | Bestand | technisches / fachliches Logging | `ID_LOG`, `DATE1`, `USER1`, `MODUL`, `FUNCTION`, `PARAMETERS`, `MESSAGE` |
| `LHD_SPA_USER_SETTINGS` | Bestand | Benutzereinstellungen | `USERNAME`, `MODULE`, `SUBJECT`, `ITEM`, `VALUE`, `VALUE_LOB` |
| `LHD_SPA_VERSION` | Bestand | Versionsinformationen | `VERSION_NUMBER`, `DATE1`, `REMARK` |
| `LHD_SPA_CHANGEREQUESTS` | Bestand | Änderungsanforderungen / interne CR-Liste | `ID_CR`, Modul, Beschreibung, Status |
| `LHD_SPA_TEXT` | Bestand | Textbausteine / Oberflächentexte | `MODUL`, `TEXTMARK`, `TEXT` |

---

## 5. Neue Portal-Tabellen V1 – Gesamtübersicht

Die folgenden Tabellen bilden den V1-Zielstand für das Portal. Sie sind aus den Domänen und aus `Packages.md` abgeleitet.

| Tabelle | Domäne | Package | Priorität | Zweck |
|---|---|---|---|---|
| `LHD_SPA_PORT_USER` | Authentication / PortalUser | `PA_LHD_SPA_PORT_AUTH`, `PA_LHD_SPA_PORT_USER` | Muss | Portalbenutzerkonto |
| `LHD_SPA_PORT_USER_LOGIN` | Authentication | `PA_LHD_SPA_PORT_AUTH` | Sollte | Login-Historie / Sicherheitsereignisse |
| `LHD_SPA_PORT_REFRESH_TOKEN` | Authentication | `PA_LHD_SPA_PORT_AUTH` | Muss | Refresh Tokens / Sessions |
| `LHD_SPA_PORT_PASSWORD_RESET` | Authentication | `PA_LHD_SPA_PORT_AUTH` | Muss | Passwort-zurücksetzen-Vorgänge |
| `LHD_SPA_PORT_2FA_CHALLENGE` | Authentication | `PA_LHD_SPA_PORT_AUTH` | Sollte | 2FA-Challenges / Einmalcodes |
| `LHD_SPA_PORT_ORGANIZATION` | Organisation | `PA_LHD_SPA_PORT_ORG` | Muss | Portalorganisation |
| `LHD_SPA_PORT_DEPARTMENT` | Organisation | `PA_LHD_SPA_PORT_ORG` | Muss | Abteilungen einer Organisation |
| `LHD_SPA_PORT_MEMBERSHIP` | Organisation / PortalUser | `PA_LHD_SPA_PORT_ORG`, `PA_LHD_SPA_PORT_USER` | Muss | Mitgliedschaft Benutzer ↔ Organisation / Abteilung |
| `LHD_SPA_PORT_MEMBERSHIP_ROLE` | Berechtigung | `PA_LHD_SPA_PORT_ORG` | Muss | Rollen je Mitgliedschaft |
| `LHD_SPA_PORT_INVITATION` | Organisation | `PA_LHD_SPA_PORT_ORG` | Muss | Einladung weiterer Benutzer |
| `LHD_SPA_PORT_CONTEXT` | Context | `PA_LHD_SPA_PORT_CONTEXT` | Muss | Nutzungskontext Portal → SportFM-Nutzer |
| `LHD_SPA_PORT_CONTEXT_RIGHT` | Context / Berechtigung | `PA_LHD_SPA_PORT_CONTEXT` | Sollte | objekt-/kontextbezogene Rechte |
| `LHD_SPA_PORT_WIZARD_DEF` | Wizard | `PA_LHD_SPA_PORT_WIZARD` | Muss | Wizard-/Antragstypdefinition |
| `LHD_SPA_PORT_WIZARD_STEP` | Wizard | `PA_LHD_SPA_PORT_WIZARD` | Muss | Schritte eines Wizards |
| `LHD_SPA_PORT_WIZARD_FIELD` | Wizard | `PA_LHD_SPA_PORT_WIZARD` | Muss | Felddefinitionen je Schritt |
| `LHD_SPA_PORT_WIZARD_RULE` | Wizard | `PA_LHD_SPA_PORT_WIZARD` | Sollte | Validierungen / Sichtbarkeiten / Bedingungen |
| `LHD_SPA_PORT_WIZARD_VERSION` | Wizard | `PA_LHD_SPA_PORT_WIZARD` | Sollte | Versionierung der Wizard-Konfiguration |
| `LHD_SPA_PORT_APPLICATION` | Application | `PA_LHD_SPA_PORT_APPLICATION` | Muss | digitaler Antrag |
| `LHD_SPA_PORT_APPLICATION_DATA` | Application | `PA_LHD_SPA_PORT_APPLICATION` | Muss | strukturierte Antragsdaten / Payload |
| `LHD_SPA_PORT_APPLICATION_REF` | Application / Booking | `PA_LHD_SPA_PORT_APPLICATION` | Muss | fachliche Verweise auf Booking, Document, Invoice usw. |
| `LHD_SPA_PORT_APPLICATION_HISTORY` | Application / Workflow | `PA_LHD_SPA_PORT_APPLICATION`, `PA_LHD_SPA_PORT_WORKFLOW` | Muss | Antragshistorie |
| `LHD_SPA_PORT_WORKFLOW` | Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | Muss | Workflowinstanz |
| `LHD_SPA_PORT_WORKFLOW_STATE` | Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | Sollte | definierte Workflowstatus |
| `LHD_SPA_PORT_WORKFLOW_TRANSITION` | Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | Sollte | erlaubte Statusübergänge |
| `LHD_SPA_PORT_TASK` | Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | Muss | Aufgabe / Arbeitskorb |
| `LHD_SPA_PORT_TASK_HISTORY` | Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | Muss | Aufgabenhistorie |
| `LHD_SPA_PORT_FILE` | Upload | `PA_LHD_SPA_PORT_UPLOAD` | Muss | Datei-Metadaten |
| `LHD_SPA_PORT_FILE_LINK` | Upload | `PA_LHD_SPA_PORT_UPLOAD` | Muss | Datei-Zuordnung zu Antrag, Aufgabe, Dokument |
| `LHD_SPA_PORT_NOTIFICATION` | Notification | `PA_LHD_SPA_PORT_NOTIFICATION` | Muss | Portalnachricht |
| `LHD_SPA_PORT_MAIL_QUEUE` | Notification | `PA_LHD_SPA_PORT_NOTIFICATION` | Muss | asynchroner Mailversand |
| `LHD_SPA_PORT_MAIL_TEMPLATE` | Notification | `PA_LHD_SPA_PORT_NOTIFICATION` | Sollte | Mailvorlagen |
| `LHD_SPA_PORT_DASHBOARD_ITEM` | Dashboard | `PA_LHD_SPA_PORT_DASHBOARD` | Kann | gespeicherte Dashboard-Konfiguration / Kacheln |
| `LHD_SPA_PORT_REF_VALUE` | ReferenceData | `PA_LHD_SPA_PORT_REF` | Sollte | portalnahe Lookup-Werte |
| `LHD_SPA_PORT_AUDIT` | Audit | `PA_LHD_SPA_PORT_AUDIT` | Muss | fachliches Audit / Revision |
| `LHD_SPA_PORT_LOG` | Logging | `PA_LHD_SPA_PORT_LOG` | Sollte | technisches Portal-Logging |

Hinweis: Tabellen mit Priorität `Sollte` oder `Kann` können bei enger V1-Planung teilweise durch Konfigurationsdateien, bestehende Tabellen oder .NET-Logik ersetzt werden. Für eine belastbare Kalkulation werden sie dennoch aufgeführt.

---

## 6. Neue Portal-Tabellen V1 im Detail

## 6.1 Authentication / PortalUser

### 6.1.1 `LHD_SPA_PORT_USER`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Authentication / PortalUser |
| Package | `PA_LHD_SPA_PORT_AUTH`, `PA_LHD_SPA_PORT_USER` |
| Zweck | Portalbenutzerkonto für natürliche Personen, die sich am Portal anmelden |
| Primärschlüssel | `ID_PORT_USER` |
| Fachlicher Schlüssel | `EMAIL_NORMALIZED` |
| Löschlogik | fachlich sperren/löschen über `STATE`, kein physisches Löschen im Normalbetrieb |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_PORT_USER` | technische Benutzer-ID |
| `EMAIL` | E-Mail-Adresse für Login und Kommunikation |
| `EMAIL_NORMALIZED` | normalisierte E-Mail für eindeutige Suche |
| `PASSWORD_HASH` | Passwort-Hash, falls lokale Anmeldung genutzt wird |
| `PASSWORD_ALGORITHM` | verwendetes Hash-/Algorithmuskennzeichen |
| `DISPLAY_NAME` | Anzeigename |
| `FIRST_NAME` | Vorname |
| `LAST_NAME` | Nachname |
| `PHONE` | optionale Telefonnummer |
| `AUTH_PROVIDER` | lokal, BundID, MUK, OAuth, extern |
| `EXTERNAL_SUBJECT_ID` | externe Benutzerkennung bei BundID/MUK/OAuth |
| `EMAIL_VERIFIED_AT` | Zeitpunkt E-Mail-Verifizierung |
| `LAST_LOGIN_AT` | letzter erfolgreicher Login |
| `FAILED_LOGIN_COUNT` | Anzahl fehlgeschlagener Logins |
| `LOCKED_UNTIL` | Sperrfrist |
| `TWO_FACTOR_ENABLED` | 2FA aktiviert ja/nein |
| `STATE` | aktiv, inaktiv, gesperrt, gelöscht |

Indizes / Constraints:

| Objekt | Zweck |
|---|---|
| Unique Index auf `EMAIL_NORMALIZED` | Eindeutigkeit Login-E-Mail |
| Index auf `AUTH_PROVIDER`, `EXTERNAL_SUBJECT_ID` | externe Anmeldung |
| Index auf `STATE` | Benutzerverwaltung |

### 6.1.2 `LHD_SPA_PORT_REFRESH_TOKEN`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Authentication |
| Package | `PA_LHD_SPA_PORT_AUTH` |
| Zweck | Verwaltung aktiver Sessions / Refresh Tokens |
| Primärschlüssel | `ID_REFRESH_TOKEN` |
| Fremdschlüssel | `ID_PORT_USER` → `LHD_SPA_PORT_USER` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_REFRESH_TOKEN` | technische Token-ID |
| `ID_PORT_USER` | Benutzerbezug |
| `TOKEN_HASH` | Hash des Refresh Tokens |
| `ISSUED_AT` | Ausstellungszeitpunkt |
| `EXPIRES_AT` | Ablaufzeitpunkt |
| `REVOKED_AT` | Widerrufzeitpunkt |
| `REVOKE_REASON` | Grund des Widerrufs |
| `IP_ADDRESS` | IP-Adresse bei Ausstellung |
| `USER_AGENT` | Browser-/Clientkennung |
| `CLIENT_ID` | Portal, WPF, externer Client |

### 6.1.3 `LHD_SPA_PORT_PASSWORD_RESET`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Authentication |
| Package | `PA_LHD_SPA_PORT_AUTH` |
| Zweck | Passwort-zurücksetzen-Prozess |
| Primärschlüssel | `ID_PASSWORD_RESET` |
| Fremdschlüssel | `ID_PORT_USER` → `LHD_SPA_PORT_USER` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_PASSWORD_RESET` | technische ID |
| `ID_PORT_USER` | Benutzerbezug |
| `TOKEN_HASH` | Hash des Reset-Tokens |
| `REQUESTED_AT` | Anforderungszeitpunkt |
| `EXPIRES_AT` | Ablaufzeitpunkt |
| `USED_AT` | Nutzungszeitpunkt |
| `REQUEST_IP` | IP-Adresse |
| `STATE` | offen, genutzt, abgelaufen, gelöscht |

### 6.1.4 `LHD_SPA_PORT_2FA_CHALLENGE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Authentication |
| Package | `PA_LHD_SPA_PORT_AUTH` |
| Zweck | temporäre 2FA-Challenges |
| Priorität | Sollte, falls 2FA nicht vollständig über externen Identity Provider abgebildet wird |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_2FA_CHALLENGE` | technische ID |
| `ID_PORT_USER` | Benutzerbezug |
| `CHALLENGE_TYPE` | E-Mail, TOTP, SMS, extern |
| `CODE_HASH` | Hash des Codes |
| `CREATED_AT` | Erzeugung |
| `EXPIRES_AT` | Ablauf |
| `USED_AT` | Nutzung |
| `FAILED_ATTEMPTS` | Fehlversuche |
| `STATE` | offen, genutzt, abgelaufen |

### 6.1.5 `LHD_SPA_PORT_USER_LOGIN`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Authentication / Audit |
| Package | `PA_LHD_SPA_PORT_AUTH`, `PA_LHD_SPA_PORT_AUDIT` |
| Zweck | Login-Historie und Sicherheitsnachweis |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_USER_LOGIN` | technische ID |
| `ID_PORT_USER` | Benutzerbezug, optional bei unbekannter E-Mail |
| `LOGIN_NAME` | verwendeter Loginname / E-Mail |
| `LOGIN_AT` | Zeitpunkt |
| `SUCCESS` | erfolgreich ja/nein |
| `FAILURE_REASON` | Fehlergrund |
| `IP_ADDRESS` | IP-Adresse |
| `USER_AGENT` | Clientkennung |

---

## 6.2 Organisation / Membership / Context

### 6.2.1 `LHD_SPA_PORT_ORGANIZATION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Organisation |
| Package | `PA_LHD_SPA_PORT_ORG` |
| Zweck | Organisation im Portal, z. B. Verein, Schule, Kita, Firma |
| Primärschlüssel | `ID_ORGANIZATION` |
| Fachlicher Bezug | kann auf bestehenden SportFM-Nutzer verweisen |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_ORGANIZATION` | technische ID |
| `ORG_TYPE` | Verein, Schule, Kita, Firma, Person, Amt, Dienstleister |
| `NAME` | Organisationsname |
| `SHORT_NAME` | Kurzname |
| `REGISTER_NR` | Vereinsregister / externe Kennung, falls vorhanden |
| `SPORTFM_USER_ID` | Bezug zum bestehenden SportFM-Nutzer, falls eindeutig |
| `CONTACT_EMAIL` | zentrale Kontakt-E-Mail |
| `CONTACT_PHONE` | zentrale Telefonnummer |
| `ADDRESS_TEXT` | Adresse, falls nicht aus Bestand referenziert |
| `VERIFICATION_STATE` | ungeprüft, geprüft, abgelehnt |
| `STATE` | aktiv, gesperrt, gelöscht |

### 6.2.2 `LHD_SPA_PORT_DEPARTMENT`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Organisation |
| Package | `PA_LHD_SPA_PORT_ORG` |
| Zweck | Abteilungen oder Untereinheiten einer Organisation |
| Primärschlüssel | `ID_DEPARTMENT` |
| Fremdschlüssel | `ID_ORGANIZATION` → `LHD_SPA_PORT_ORGANIZATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_DEPARTMENT` | technische ID |
| `ID_ORGANIZATION` | Organisation |
| `NAME` | Abteilungsname |
| `SPORT_TYPE_ID` | optionale Sportartzuordnung |
| `CONTACT_PERSON` | Ansprechpartnertext |
| `STATE` | aktiv, gelöscht |

### 6.2.3 `LHD_SPA_PORT_MEMBERSHIP`

| Aspekt | Beschreibung |
|---|---|
| Domäne | PortalUser / Organisation |
| Package | `PA_LHD_SPA_PORT_USER`, `PA_LHD_SPA_PORT_ORG` |
| Zweck | Zuordnung Portalbenutzer zu Organisation / Abteilung |
| Primärschlüssel | `ID_MEMBERSHIP` |
| Fremdschlüssel | `ID_PORT_USER`, `ID_ORGANIZATION`, optional `ID_DEPARTMENT` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_MEMBERSHIP` | technische ID |
| `ID_PORT_USER` | Portalbenutzer |
| `ID_ORGANIZATION` | Organisation |
| `ID_DEPARTMENT` | optionale Abteilung |
| `MEMBERSHIP_STATE` | beantragt, aktiv, abgelehnt, beendet, gesperrt |
| `VERIFIED_BY` | prüfender Sachbearbeiter / Benutzer |
| `VERIFIED_AT` | Prüfzeitpunkt |
| `VALID_FROM` | Beginn Berechtigung |
| `VALID_UNTIL` | Ende Berechtigung |
| `IS_PRIMARY` | Hauptzuordnung ja/nein |

### 6.2.4 `LHD_SPA_PORT_MEMBERSHIP_ROLE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Berechtigung / Organisation |
| Package | `PA_LHD_SPA_PORT_ORG` |
| Zweck | Rollen je Mitgliedschaft |
| Primärschlüssel | `ID_MEMBERSHIP_ROLE` |
| Fremdschlüssel | `ID_MEMBERSHIP` → `LHD_SPA_PORT_MEMBERSHIP` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_MEMBERSHIP_ROLE` | technische ID |
| `ID_MEMBERSHIP` | Mitgliedschaft |
| `ROLE_CODE` | Rolle, z. B. `ORG_ADMIN`, `APPLICANT`, `READER`, `BILLING` |
| `VALID_FROM` | Beginn |
| `VALID_UNTIL` | Ende |
| `STATE` | aktiv, gelöscht |

### 6.2.5 `LHD_SPA_PORT_INVITATION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Organisation |
| Package | `PA_LHD_SPA_PORT_ORG`, `PA_LHD_SPA_PORT_NOTIFICATION` |
| Zweck | Einladung weiterer Benutzer zu einer Organisation |
| Primärschlüssel | `ID_INVITATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_INVITATION` | technische ID |
| `ID_ORGANIZATION` | Organisation |
| `ID_DEPARTMENT` | optionale Abteilung |
| `EMAIL` | eingeladene E-Mail |
| `ROLE_CODE` | vorgesehene Rolle |
| `INVITED_BY` | einladender Portalbenutzer |
| `INVITED_AT` | Einladungszeitpunkt |
| `TOKEN_HASH` | Einladungs-Token |
| `EXPIRES_AT` | Ablaufzeitpunkt |
| `ACCEPTED_AT` | Annahmezeitpunkt |
| `STATE` | offen, angenommen, abgelaufen, widerrufen |

### 6.2.6 `LHD_SPA_PORT_CONTEXT`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Context |
| Package | `PA_LHD_SPA_PORT_CONTEXT` |
| Zweck | aktiver fachlicher Nutzungskontext für Portalzugriffe |
| Primärschlüssel | `ID_CONTEXT` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_CONTEXT` | technische ID |
| `ID_PORT_USER` | Portalbenutzer |
| `ID_ORGANIZATION` | Organisation |
| `ID_DEPARTMENT` | optionale Abteilung |
| `ID_MEMBERSHIP` | konkrete Mitgliedschaft |
| `SPORTFM_USER_ID` | führender SportFM-Nutzer für Dokumente, Rechnungen, Buchungen |
| `CONTEXT_TYPE` | Verein, Schule, Kita, Firma, Privatperson, intern |
| `IS_DEFAULT` | Standardkontext |
| `STATE` | aktiv, gesperrt, gelöscht |

Fachliche Regel:

> Jeder Zugriff auf eigene Dokumente, Rechnungen, Anträge und Buchungen muss über einen gültigen Kontext geprüft werden.

### 6.2.7 `LHD_SPA_PORT_CONTEXT_RIGHT`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Context / Berechtigung |
| Package | `PA_LHD_SPA_PORT_CONTEXT` |
| Zweck | zusätzliche objekt- oder funktionsbezogene Rechte innerhalb eines Kontextes |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_CONTEXT_RIGHT` | technische ID |
| `ID_CONTEXT` | Kontext |
| `RIGHT_CODE` | Recht, z. B. `APPLICATION_SUBMIT`, `DOCUMENT_READ`, `INVOICE_READ` |
| `SCOPE_TYPE` | global, facility, application, invoice, document |
| `SCOPE_ID` | optionale Objekt-ID |
| `VALID_FROM` | Beginn |
| `VALID_UNTIL` | Ende |
| `STATE` | aktiv, gelöscht |

---

## 6.3 Wizard

Die Wizard-Domäne ist strategisch wichtig, weil neue Antragsarten langfristig möglichst über Konfiguration statt Programmierung entstehen sollen.

Für V1 wird mindestens ein konfigurierbarer Kern benötigt. Ein vollständiger grafischer Wizard-Designer ist nicht zwingend Bestandteil von V1, das Datenmodell soll diesen später aber nicht verhindern.

### 6.3.1 `LHD_SPA_PORT_WIZARD_DEF`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Wizard |
| Package | `PA_LHD_SPA_PORT_WIZARD` |
| Zweck | Definition eines Wizard-/Antragstyps |
| Primärschlüssel | `ID_WIZARD_DEF` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WIZARD_DEF` | technische ID |
| `WIZARD_CODE` | stabiler fachlicher Code, z. B. `TRAINING_STANDARD`, `EVENT_ESBZ` |
| `TITLE` | Titel |
| `DESCRIPTION` | Beschreibung |
| `APPLICATION_TYPE` | zu erzeugender Antragstyp |
| `CONFIG_VERSION` | fachliche Version |
| `VALID_FROM` | gültig ab |
| `VALID_UNTIL` | gültig bis |
| `STATE` | Entwurf, aktiv, archiviert, gelöscht |
| `CONFIG_JSON` | optionale Gesamtkonfiguration / Ergänzung |

### 6.3.2 `LHD_SPA_PORT_WIZARD_STEP`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Wizard |
| Package | `PA_LHD_SPA_PORT_WIZARD` |
| Zweck | Schrittdefinition innerhalb eines Wizards |
| Primärschlüssel | `ID_WIZARD_STEP` |
| Fremdschlüssel | `ID_WIZARD_DEF` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WIZARD_STEP` | technische ID |
| `ID_WIZARD_DEF` | Wizard |
| `STEP_CODE` | stabiler Schrittcode |
| `TITLE` | Schrittüberschrift |
| `DESCRIPTION` | Hilfetext |
| `SORT_ORDER` | Reihenfolge |
| `IS_OPTIONAL` | optionaler Schritt |
| `VISIBLE_RULE_CODE` | optionale Sichtbarkeitsregel |
| `CONFIG_JSON` | Schrittoptionen |
| `STATE` | aktiv, gelöscht |

### 6.3.3 `LHD_SPA_PORT_WIZARD_FIELD`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Wizard |
| Package | `PA_LHD_SPA_PORT_WIZARD` |
| Zweck | Felddefinition innerhalb eines Wizard-Schritts |
| Primärschlüssel | `ID_WIZARD_FIELD` |
| Fremdschlüssel | `ID_WIZARD_STEP` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WIZARD_FIELD` | technische ID |
| `ID_WIZARD_STEP` | Schritt |
| `FIELD_CODE` | stabiler Feldcode |
| `LABEL` | Beschriftung |
| `HELP_TEXT` | Hilfetext |
| `FIELD_TYPE` | Text, Number, Date, Time, Select, Checkbox, Upload, FacilitySearch usw. |
| `DATA_PATH` | Pfad in Application-Payload |
| `IS_REQUIRED` | Pflichtfeld |
| `SORT_ORDER` | Reihenfolge |
| `LOOKUP_CODE` | Auswahlliste / Reference Data |
| `DEFAULT_VALUE` | Defaultwert |
| `VALIDATION_RULE_CODE` | Validierungsregel |
| `VISIBLE_RULE_CODE` | Sichtbarkeitsregel |
| `CONFIG_JSON` | zusätzliche Feldkonfiguration |
| `STATE` | aktiv, gelöscht |

### 6.3.4 `LHD_SPA_PORT_WIZARD_RULE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Wizard |
| Package | `PA_LHD_SPA_PORT_WIZARD` |
| Zweck | Validierungs-, Sichtbarkeits- und Navigationsregeln |
| Primärschlüssel | `ID_WIZARD_RULE` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WIZARD_RULE` | technische ID |
| `ID_WIZARD_DEF` | Wizard |
| `RULE_CODE` | stabiler Regelcode |
| `RULE_TYPE` | validation, visibility, navigation, calculation |
| `EXPRESSION` | einfache Regel / Ausdruck |
| `ERROR_MESSAGE` | Fehlermeldung |
| `CONFIG_JSON` | komplexe Regelkonfiguration |
| `STATE` | aktiv, gelöscht |

### 6.3.5 `LHD_SPA_PORT_WIZARD_VERSION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Wizard |
| Package | `PA_LHD_SPA_PORT_WIZARD` |
| Zweck | Nachvollziehbarkeit veröffentlichter Wizard-Versionen |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WIZARD_VERSION` | technische ID |
| `ID_WIZARD_DEF` | Wizard |
| `VERSION_NO` | Versionsnummer |
| `PUBLISHED_AT` | Veröffentlichungszeitpunkt |
| `PUBLISHED_BY` | Benutzer |
| `SNAPSHOT_JSON` | Konfigurationssnapshot |
| `STATE` | aktiv, archiviert |

---

## 6.4 Application

### 6.4.1 `LHD_SPA_PORT_APPLICATION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Application |
| Package | `PA_LHD_SPA_PORT_APPLICATION` |
| Zweck | Kopfdatensatz eines digitalen Antrags |
| Primärschlüssel | `ID_APPLICATION` |
| Fremdschlüssel | `ID_CONTEXT`, optional `ID_WIZARD_DEF` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_APPLICATION` | technische Antrags-ID |
| `APPLICATION_NUMBER` | fachliche Antragsnummer |
| `ID_CONTEXT` | Portal-/Organisationkontext |
| `ID_PORT_USER_CREATED_BY` | anlegender Portalbenutzer |
| `ID_WIZARD_DEF` | verwendeter Wizard |
| `WIZARD_VERSION` | verwendete Wizard-Version |
| `APPLICATION_TYPE` | Antragstyp, z. B. Training, Veranstaltung, ESBZ |
| `TITLE` | Kurztitel |
| `SUBJECT` | Anliegen / Kurzbeschreibung |
| `APPLICATION_STATE` | Entwurf, eingereicht, in Prüfung, Rückfrage, genehmigt, abgelehnt, erledigt, zurückgezogen |
| `SUBMITTED_AT` | Einreichungszeitpunkt |
| `WITHDRAWN_AT` | Rücknahmezeitpunkt |
| `DECIDED_AT` | Entscheidungszeitpunkt |
| `SPORTFM_USER_ID` | SportFM-Nutzerbezug aus Context |
| `ID_EVENT` | optionale spätere Buchungs-/Eventreferenz |
| `STATE` | technischer Zustand |

### 6.4.2 `LHD_SPA_PORT_APPLICATION_DATA`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Application |
| Package | `PA_LHD_SPA_PORT_APPLICATION` |
| Zweck | strukturierte Antragsdaten / Payload |
| Primärschlüssel | `ID_APPLICATION_DATA` |
| Fremdschlüssel | `ID_APPLICATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_APPLICATION_DATA` | technische ID |
| `ID_APPLICATION` | Antrag |
| `DATA_VERSION` | Version der Payload |
| `PAYLOAD_JSON` | strukturierte Antragsdaten |
| `SUMMARY_JSON` | optional verdichtete Zusammenfassung für Listen / Arbeitskorb |
| `VALIDATION_STATE` | ungeprüft, gültig, fehlerhaft |
| `VALIDATED_AT` | Zeitpunkt letzter Validierung |
| `STATE` | aktiv, archiviert, gelöscht |

Fachliche Regel:

> Die Payload speichert den fachlichen Antrag, nicht die spätere Buchung. Die Überführung in Buchung / Reservierung erfolgt erst nach fachlicher Prüfung.

### 6.4.3 `LHD_SPA_PORT_APPLICATION_REF`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Application / Integration |
| Package | `PA_LHD_SPA_PORT_APPLICATION` |
| Zweck | flexible Referenzen eines Antrags auf SportFM-Objekte |
| Primärschlüssel | `ID_APPLICATION_REF` |
| Fremdschlüssel | `ID_APPLICATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_APPLICATION_REF` | technische ID |
| `ID_APPLICATION` | Antrag |
| `REF_TYPE` | Event, Document, Invoice, Facility, Unit, SAP, External |
| `REF_ID_NUM` | numerische Referenz |
| `REF_ID_TEXT` | textuelle Referenz |
| `REF_ROLE` | z. B. Zielbuchung, erzeugtes Dokument, ausgewählte Anlage |
| `STATE` | aktiv, gelöscht |

### 6.4.4 `LHD_SPA_PORT_APPLICATION_HISTORY`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Application / Workflow |
| Package | `PA_LHD_SPA_PORT_APPLICATION`, `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | fachliche Antragshistorie |
| Primärschlüssel | `ID_APPLICATION_HISTORY` |
| Fremdschlüssel | `ID_APPLICATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_APPLICATION_HISTORY` | technische ID |
| `ID_APPLICATION` | Antrag |
| `EVENT_TYPE` | erstellt, gespeichert, eingereicht, geprüft, Rückfrage, genehmigt, abgelehnt, zurückgezogen |
| `OLD_STATE` | vorheriger Status |
| `NEW_STATE` | neuer Status |
| `COMMENT_TEXT` | Kommentar |
| `ACTOR_TYPE` | PortalUser, Sachbearbeiter, System |
| `ACTOR_ID` | auslösende Person / System |
| `EVENT_AT` | Zeitpunkt |
| `DATA_JSON` | optionale Zusatzdaten |

---

## 6.5 Workflow / Arbeitskorb

### 6.5.1 `LHD_SPA_PORT_WORKFLOW`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Workflow |
| Package | `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | Workflowinstanz zu einem Antrag oder Portalvorgang |
| Primärschlüssel | `ID_WORKFLOW` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WORKFLOW` | technische Workflow-ID |
| `WORKFLOW_TYPE` | Application, Cancellation, Inquiry, InternalTask |
| `ID_APPLICATION` | optionaler Antragsbezug |
| `CURRENT_STATE` | aktueller Workflowstatus |
| `ASSIGNED_GROUP` | zuständige Gruppe / Sachgebiet |
| `ASSIGNED_USER` | zuständige Person, optional |
| `PRIORITY` | Priorität |
| `DUE_AT` | Fälligkeit |
| `STARTED_AT` | Startzeitpunkt |
| `COMPLETED_AT` | Abschlusszeitpunkt |
| `STATE` | aktiv, abgeschlossen, gelöscht |

### 6.5.2 `LHD_SPA_PORT_WORKFLOW_STATE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Workflow |
| Package | `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | definierte Workflowstatus je Workflowtyp |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WORKFLOW_STATE` | technische ID |
| `WORKFLOW_TYPE` | Workflowtyp |
| `STATE_CODE` | Statuscode |
| `TITLE` | Bezeichnung |
| `IS_INITIAL` | Startstatus |
| `IS_FINAL` | Endstatus |
| `SORT_ORDER` | Reihenfolge |
| `STATE` | aktiv, gelöscht |

### 6.5.3 `LHD_SPA_PORT_WORKFLOW_TRANSITION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Workflow |
| Package | `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | erlaubte Statusübergänge |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_WORKFLOW_TRANSITION` | technische ID |
| `WORKFLOW_TYPE` | Workflowtyp |
| `FROM_STATE` | Ausgangsstatus |
| `TO_STATE` | Zielstatus |
| `ACTION_CODE` | Aktion, z. B. submit, approve, reject, ask_question |
| `REQUIRED_ROLE` | benötigte Rolle |
| `CONFIG_JSON` | Bedingungen / Folgeaktionen |
| `STATE` | aktiv, gelöscht |

### 6.5.4 `LHD_SPA_PORT_TASK`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Workflow / Arbeitskorb |
| Package | `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | Aufgaben für Sachbearbeitung oder Portalnutzer |
| Primärschlüssel | `ID_TASK` |
| Fremdschlüssel | `ID_WORKFLOW`, optional `ID_APPLICATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_TASK` | technische Aufgaben-ID |
| `ID_WORKFLOW` | Workflow |
| `ID_APPLICATION` | Antrag |
| `TASK_TYPE` | Prüfung, Rückfrage, Entscheidung, Wiedervorlage |
| `TITLE` | Aufgabentitel |
| `DESCRIPTION` | Beschreibung |
| `TASK_STATE` | offen, in Bearbeitung, erledigt, storniert |
| `ASSIGNED_GROUP` | zuständige Gruppe |
| `ASSIGNED_USER` | zuständige Person |
| `DUE_AT` | Fälligkeit |
| `COMPLETED_AT` | Abschlusszeitpunkt |
| `COMPLETED_BY` | abschließender Benutzer |

### 6.5.5 `LHD_SPA_PORT_TASK_HISTORY`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Workflow |
| Package | `PA_LHD_SPA_PORT_WORKFLOW` |
| Zweck | Historie zu Aufgaben |
| Primärschlüssel | `ID_TASK_HISTORY` |
| Fremdschlüssel | `ID_TASK` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_TASK_HISTORY` | technische ID |
| `ID_TASK` | Aufgabe |
| `EVENT_TYPE` | erstellt, zugewiesen, übernommen, erledigt, zurückgegeben |
| `OLD_STATE` | vorheriger Zustand |
| `NEW_STATE` | neuer Zustand |
| `COMMENT_TEXT` | Kommentar |
| `ACTOR_ID` | auslösender Benutzer |
| `EVENT_AT` | Zeitpunkt |

---

## 6.6 Upload

### 6.6.1 `LHD_SPA_PORT_FILE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Upload |
| Package | `PA_LHD_SPA_PORT_UPLOAD` |
| Zweck | technische und fachliche Metadaten hochgeladener Dateien |
| Primärschlüssel | `ID_FILE` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_FILE` | technische Datei-ID |
| `ID_CONTEXT` | Kontext des Uploads |
| `UPLOADED_BY` | Portalbenutzer |
| `UPLOADED_AT` | Uploadzeitpunkt |
| `ORIGINAL_FILENAME` | ursprünglicher Dateiname |
| `STORED_FILENAME` | technischer Speichername, falls Dateisystem/Object Storage |
| `MIME_TYPE` | MIME-Type |
| `FILE_SIZE_BYTES` | Größe |
| `SHA256_HASH` | Prüfsumme |
| `STORAGE_TYPE` | DB, FILESYSTEM, OBJECT_STORAGE |
| `STORAGE_REF` | Speicherreferenz |
| `VIRUS_SCAN_STATE` | nicht geprüft, sauber, auffällig, Fehler |
| `VIRUS_SCAN_AT` | Prüfzeitpunkt |
| `STATE` | temporär, aktiv, gelöscht, quarantäne |

### 6.6.2 `LHD_SPA_PORT_FILE_LINK`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Upload |
| Package | `PA_LHD_SPA_PORT_UPLOAD` |
| Zweck | Zuordnung einer Datei zu fachlichen Objekten |
| Primärschlüssel | `ID_FILE_LINK` |
| Fremdschlüssel | `ID_FILE` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_FILE_LINK` | technische ID |
| `ID_FILE` | Datei |
| `TARGET_TYPE` | Application, Task, Document, Organization |
| `TARGET_ID` | Zielobjekt-ID |
| `LINK_ROLE` | Anlage, Nachweis, Rückfrage, Bescheid, intern |
| `VISIBLE_TO_PORTAL` | im Portal sichtbar ja/nein |
| `STATE` | aktiv, gelöscht |

Fachliche Regel:

> Uploads werden zunächst als Portaldateien geführt. Erst bei fachlicher Übernahme in SportFM können daraus SportFM-Dokumente oder Dokumentbezüge entstehen.

---

## 6.7 Notification / Mail

### 6.7.1 `LHD_SPA_PORT_NOTIFICATION`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Notification |
| Package | `PA_LHD_SPA_PORT_NOTIFICATION` |
| Zweck | Portalnachrichten für Benutzer / Organisationen |
| Primärschlüssel | `ID_NOTIFICATION` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_NOTIFICATION` | technische Nachricht-ID |
| `ID_PORT_USER` | Empfänger, optional |
| `ID_CONTEXT` | Kontext, optional |
| `NOTIFICATION_TYPE` | Antrag, Dokument, Rechnung, Rückfrage, System |
| `TITLE` | Titel |
| `BODY` | Nachrichtentext |
| `TARGET_TYPE` | Application, Invoice, Document, Task, General |
| `TARGET_ID` | Zielobjekt |
| `READ_AT` | Gelesen-Zeitpunkt |
| `STATE` | neu, gelesen, archiviert, gelöscht |

### 6.7.2 `LHD_SPA_PORT_MAIL_QUEUE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Notification |
| Package | `PA_LHD_SPA_PORT_NOTIFICATION` |
| Zweck | asynchroner Mailversand |
| Primärschlüssel | `ID_MAIL_QUEUE` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_MAIL_QUEUE` | technische ID |
| `MAIL_TYPE` | Registrierung, Passwort, Antrag, Rückfrage, Dokument, Rechnung |
| `TO_ADDRESS` | Empfänger |
| `CC_ADDRESS` | CC, optional |
| `BCC_ADDRESS` | BCC, optional |
| `SUBJECT` | Betreff |
| `BODY_TEXT` | Textinhalt |
| `BODY_HTML` | HTML-Inhalt |
| `SEND_STATE` | offen, gesendet, Fehler, abgebrochen |
| `TRIES` | Versandversuche |
| `LAST_ERROR` | letzter Fehler |
| `NEXT_TRY_AT` | nächster Versuch |
| `SENT_AT` | Versandzeitpunkt |
| `CORRELATION_ID` | technische Korrelation |

### 6.7.3 `LHD_SPA_PORT_MAIL_TEMPLATE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Notification |
| Package | `PA_LHD_SPA_PORT_NOTIFICATION` |
| Zweck | Mailvorlagen |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_MAIL_TEMPLATE` | technische ID |
| `TEMPLATE_CODE` | stabiler Vorlagencode |
| `TITLE` | interne Bezeichnung |
| `SUBJECT_TEMPLATE` | Betreffvorlage |
| `BODY_TEXT_TEMPLATE` | Textvorlage |
| `BODY_HTML_TEMPLATE` | HTML-Vorlage |
| `LANGUAGE_CODE` | Sprache |
| `VALID_FROM` | gültig ab |
| `VALID_UNTIL` | gültig bis |
| `STATE` | Entwurf, aktiv, archiviert, gelöscht |

---

## 6.8 Dashboard / ReferenceData / Administration

### 6.8.1 `LHD_SPA_PORT_DASHBOARD_ITEM`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Dashboard |
| Package | `PA_LHD_SPA_PORT_DASHBOARD` |
| Zweck | optionale Speicherung rollenabhängiger Dashboard-Kacheln / Konfiguration |
| Priorität | Kann |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_DASHBOARD_ITEM` | technische ID |
| `ROLE_CODE` | Rolle / Zielgruppe |
| `ITEM_CODE` | Kachelcode |
| `TITLE` | Titel |
| `SORT_ORDER` | Reihenfolge |
| `CONFIG_JSON` | Konfiguration |
| `STATE` | aktiv, gelöscht |

Hinweis:

> In V1 kann das Dashboard auch vollständig aus Services aggregiert werden. Die Tabelle ist nur erforderlich, wenn Dashboard-Inhalte administrativ konfigurierbar sein sollen.

### 6.8.2 `LHD_SPA_PORT_REF_VALUE`

| Aspekt | Beschreibung |
|---|---|
| Domäne | ReferenceData |
| Package | `PA_LHD_SPA_PORT_REF` |
| Zweck | portalnahe Lookup-Werte, die nicht bereits sauber aus SportFM-Stammdaten kommen |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_REF_VALUE` | technische ID |
| `REF_GROUP` | Gruppe, z. B. Antragstyp, UploadKategorie, Portalrolle |
| `REF_CODE` | stabiler Code |
| `TITLE` | Bezeichnung |
| `DESCRIPTION` | Beschreibung |
| `SORT_ORDER` | Reihenfolge |
| `VALID_FROM` | gültig ab |
| `VALID_UNTIL` | gültig bis |
| `STATE` | aktiv, gelöscht |

### 6.8.3 Portal-Administrationstabellen

Für V1 wird keine eigene generische Administrationstabellenwelt festgelegt. Administration nutzt die Tabellen der jeweiligen Domänen.

Beispiele:

| Administrativer Bereich | Tabelle |
|---|---|
| Benutzerverwaltung | `LHD_SPA_PORT_USER` |
| Organisationen | `LHD_SPA_PORT_ORGANIZATION`, `LHD_SPA_PORT_MEMBERSHIP` |
| Wizard-Konfiguration | `LHD_SPA_PORT_WIZARD_*` |
| Mailvorlagen | `LHD_SPA_PORT_MAIL_TEMPLATE` |
| Reference Data | `LHD_SPA_PORT_REF_VALUE` |

---

## 6.9 Audit / Logging

### 6.9.1 `LHD_SPA_PORT_AUDIT`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Audit / Revision |
| Package | `PA_LHD_SPA_PORT_AUDIT` |
| Zweck | fachliches Audit für Portalvorgänge |
| Primärschlüssel | `ID_AUDIT` |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_AUDIT` | technische ID |
| `EVENT_AT` | Zeitpunkt |
| `ACTOR_TYPE` | PortalUser, Sachbearbeiter, System, API-Client |
| `ACTOR_ID` | auslösender Akteur |
| `ID_CONTEXT` | Kontext, falls vorhanden |
| `ACTION_CODE` | Aktion, z. B. login, submit_application, download_document |
| `TARGET_TYPE` | Zielobjekttyp |
| `TARGET_ID` | Zielobjekt-ID |
| `RESULT_CODE` | success, denied, error |
| `MESSAGE` | Kurzbeschreibung |
| `DATA_JSON` | Zusatzinformationen |
| `CORRELATION_ID` | technische Korrelation |
| `IP_ADDRESS` | IP-Adresse |
| `USER_AGENT` | Clientkennung |

Fachliche Regel:

> Sicherheits- und rechtsrelevante Aktionen müssen auditierbar sein, insbesondere Login, Kontextwechsel, Antragseinreichung, Dokumentdownload, Rechnungsanzeige und administrative Änderungen.

### 6.9.2 `LHD_SPA_PORT_LOG`

| Aspekt | Beschreibung |
|---|---|
| Domäne | Logging |
| Package | `PA_LHD_SPA_PORT_LOG` |
| Zweck | technisches Logging für Portal-/REST-/Package-Fehler |
| Priorität | Sollte |

Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `ID_PORT_LOG` | technische ID |
| `LOG_AT` | Zeitpunkt |
| `LOG_LEVEL` | Debug, Info, Warning, Error, Critical |
| `MODULE` | Modul / Package / REST-Bereich |
| `FUNCTION_NAME` | Funktion / Operation |
| `MESSAGE` | Meldung |
| `ERROR_CODE` | Fehlercode |
| `ERROR_STACK` | technischer Stack, falls zulässig |
| `CORRELATION_ID` | Korrelation |
| `DATA_JSON` | Zusatzdaten |

Hinweis:

> Technisches Logging kann alternativ oder ergänzend über bestehende Logging-Infrastruktur erfolgen. Fachliches Audit darf dadurch nicht ersetzt werden.

---

## 7. Portal-Sichten auf bestehende Domänen ohne neue Kerntabellen

Für einige Domänen werden in V1 keine neuen fachlichen Kerntabellen benötigt. Sie nutzen bestehende SportFM-Tabellen und erhalten nur Package-/View-/DTO-Schichten.

| Domäne | Neue Kerntabellen V1 | Begründung |
|---|---:|---|
| Facility | nein | Sportanlagen und Teileinheiten liegen führend in RIB FM / SportFM |
| Availability | nein | freie Zeiten basieren auf `LHD_SPA_OCC` / `LHD_SPA_OCC_WINNER` |
| Booking | nein / optional | Buchungen bleiben `LHD_SPA_EVENTS`; Portal-Antragsbezug über `LHD_SPA_PORT_APPLICATION_REF` |
| Charge | nein | Gebühren bleiben `LHD_SPA_CHARGES` / bestehende Gebührenlogik |
| Invoice | nein | Rechnungen bleiben `LHD_SPA_INVOICES` |
| Document | nein | Dokumente bleiben `LHD_SPA_DOCUMENTS`; Uploads sind separate Portaldateien |

Mögliche Views:

| View | Zweck |
|---|---|
| `VW_LHD_SPA_PORT_FACILITY` | portalgeeignete Sportstättensicht |
| `VW_LHD_SPA_PORT_UNIT` | portalgeeignete Teileinheitensicht |
| `VW_LHD_SPA_PORT_AVAILABILITY` | freie Zeiten / Kalenderdaten |
| `VW_LHD_SPA_PORT_DOCUMENT` | kontextgefilterte Dokumentliste |
| `VW_LHD_SPA_PORT_INVOICE` | kontextgefilterte Rechnungsliste |

Views ersetzen keine Berechtigungsprüfung in Packages. Sie können aber die technische Umsetzung vereinfachen.

---

## 8. Zentrale Beziehungen

### 8.1 Portalbenutzer → Organisation → Kontext

```text
LHD_SPA_PORT_USER
  └─ LHD_SPA_PORT_MEMBERSHIP
       ├─ LHD_SPA_PORT_MEMBERSHIP_ROLE
       └─ LHD_SPA_PORT_ORGANIZATION
            └─ LHD_SPA_PORT_DEPARTMENT
                 └─ LHD_SPA_PORT_CONTEXT
                      └─ SPORTFM_USER_ID / bestehender SportFM-Nutzer
```

### 8.2 Antrag → Workflow → Upload → SportFM

```text
LHD_SPA_PORT_APPLICATION
  ├─ LHD_SPA_PORT_APPLICATION_DATA
  ├─ LHD_SPA_PORT_APPLICATION_HISTORY
  ├─ LHD_SPA_PORT_APPLICATION_REF
  ├─ LHD_SPA_PORT_WORKFLOW
  │    └─ LHD_SPA_PORT_TASK
  │         └─ LHD_SPA_PORT_TASK_HISTORY
  └─ LHD_SPA_PORT_FILE_LINK
       └─ LHD_SPA_PORT_FILE

LHD_SPA_PORT_APPLICATION_REF
  ├─ LHD_SPA_EVENTS
  ├─ LHD_SPA_DOCUMENTS
  ├─ LHD_SPA_INVOICES
  └─ weitere bestehende SportFM-Objekte
```

### 8.3 Wizard → Antrag

```text
LHD_SPA_PORT_WIZARD_DEF
  ├─ LHD_SPA_PORT_WIZARD_STEP
  │    └─ LHD_SPA_PORT_WIZARD_FIELD
  ├─ LHD_SPA_PORT_WIZARD_RULE
  └─ LHD_SPA_PORT_WIZARD_VERSION

LHD_SPA_PORT_APPLICATION
  └─ verweist auf verwendeten Wizard und Version
```

### 8.4 Benachrichtigung / Audit

```text
LHD_SPA_PORT_APPLICATION
  ├─ LHD_SPA_PORT_NOTIFICATION
  ├─ LHD_SPA_PORT_MAIL_QUEUE
  └─ LHD_SPA_PORT_AUDIT
```

---

## 9. Package-Zuordnung

| Package | Primäre Tabellen | Ruft Bestand auf |
|---|---|---|
| `PA_LHD_SPA_PORT_AUTH` | `LHD_SPA_PORT_USER`, `LHD_SPA_PORT_REFRESH_TOKEN`, `LHD_SPA_PORT_PASSWORD_RESET`, `LHD_SPA_PORT_2FA_CHALLENGE`, `LHD_SPA_PORT_USER_LOGIN` | nein |
| `PA_LHD_SPA_PORT_USER` | `LHD_SPA_PORT_USER`, `LHD_SPA_PORT_MEMBERSHIP`, `LHD_SPA_PORT_MEMBERSHIP_ROLE` | nein |
| `PA_LHD_SPA_PORT_ORG` | `LHD_SPA_PORT_ORGANIZATION`, `LHD_SPA_PORT_DEPARTMENT`, `LHD_SPA_PORT_MEMBERSHIP`, `LHD_SPA_PORT_INVITATION` | ggf. SportFM-Nutzer-Stammdaten |
| `PA_LHD_SPA_PORT_CONTEXT` | `LHD_SPA_PORT_CONTEXT`, `LHD_SPA_PORT_CONTEXT_RIGHT` | SportFM-Nutzerbezug |
| `PA_LHD_SPA_PORT_WIZARD` | `LHD_SPA_PORT_WIZARD_DEF`, `LHD_SPA_PORT_WIZARD_STEP`, `LHD_SPA_PORT_WIZARD_FIELD`, `LHD_SPA_PORT_WIZARD_RULE`, `LHD_SPA_PORT_WIZARD_VERSION` | nein |
| `PA_LHD_SPA_PORT_APPLICATION` | `LHD_SPA_PORT_APPLICATION`, `LHD_SPA_PORT_APPLICATION_DATA`, `LHD_SPA_PORT_APPLICATION_REF`, `LHD_SPA_PORT_APPLICATION_HISTORY` | `PA_LHD_SPA`, ggf. Bestandstabellen |
| `PA_LHD_SPA_PORT_WORKFLOW` | `LHD_SPA_PORT_WORKFLOW`, `LHD_SPA_PORT_WORKFLOW_STATE`, `LHD_SPA_PORT_WORKFLOW_TRANSITION`, `LHD_SPA_PORT_TASK`, `LHD_SPA_PORT_TASK_HISTORY` | ggf. Application |
| `PA_LHD_SPA_PORT_UPLOAD` | `LHD_SPA_PORT_FILE`, `LHD_SPA_PORT_FILE_LINK` | ggf. `LHD_SPA_DOCUMENTS` bei Übernahme |
| `PA_LHD_SPA_PORT_NOTIFICATION` | `LHD_SPA_PORT_NOTIFICATION`, `LHD_SPA_PORT_MAIL_QUEUE`, `LHD_SPA_PORT_MAIL_TEMPLATE` | nein |
| `PA_LHD_SPA_PORT_DASHBOARD` | optional `LHD_SPA_PORT_DASHBOARD_ITEM` | mehrere Domänen |
| `PA_LHD_SPA_PORT_FACILITY` | keine neuen Kerntabellen | RIB FM / SportFM-Stammdaten |
| `PA_LHD_SPA_PORT_AVAILABILITY` | keine neuen Kerntabellen | `PA_LHD_SPA_OCC`, `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER` |
| `PA_LHD_SPA_PORT_BOOKING` | keine neuen Kerntabellen, ggf. `LHD_SPA_PORT_APPLICATION_REF` | `PA_LHD_SPA`, `LHD_SPA_EVENTS` |
| `PA_LHD_SPA_PORT_CHARGE` | keine neuen Kerntabellen | `PA_LHD_SPA`, `LHD_SPA_CHARGES`, `LHD_SPA_EVENTCHARGES` |
| `PA_LHD_SPA_PORT_INVOICE` | keine neuen Kerntabellen | `LHD_SPA_INVOICES`, SAP-Bezüge |
| `PA_LHD_SPA_PORT_DOCUMENT` | keine neuen Kerntabellen | `LHD_SPA_DOCUMENTS` |
| `PA_LHD_SPA_PORT_REF` | `LHD_SPA_PORT_REF_VALUE` | SportFM-Stammdaten |
| `PA_LHD_SPA_PORT_AUDIT` | `LHD_SPA_PORT_AUDIT` | optional bestehendes Logging |
| `PA_LHD_SPA_PORT_LOG` | `LHD_SPA_PORT_LOG` | optional `LHD_SPA_LOGGING` |

---

## 10. V1-Mindestumfang für kalkulierbare Umsetzung

Für eine belastbare V1-Kalkulation sollten mindestens diese Tabellen eingeplant werden:

```text
Authentication / PortalUser
  LHD_SPA_PORT_USER
  LHD_SPA_PORT_REFRESH_TOKEN
  LHD_SPA_PORT_PASSWORD_RESET

Organisation / Context
  LHD_SPA_PORT_ORGANIZATION
  LHD_SPA_PORT_DEPARTMENT
  LHD_SPA_PORT_MEMBERSHIP
  LHD_SPA_PORT_MEMBERSHIP_ROLE
  LHD_SPA_PORT_INVITATION
  LHD_SPA_PORT_CONTEXT

Wizard / Application
  LHD_SPA_PORT_WIZARD_DEF
  LHD_SPA_PORT_WIZARD_STEP
  LHD_SPA_PORT_WIZARD_FIELD
  LHD_SPA_PORT_WIZARD_RULE
  LHD_SPA_PORT_APPLICATION
  LHD_SPA_PORT_APPLICATION_DATA
  LHD_SPA_PORT_APPLICATION_REF
  LHD_SPA_PORT_APPLICATION_HISTORY

Workflow
  LHD_SPA_PORT_WORKFLOW
  LHD_SPA_PORT_TASK
  LHD_SPA_PORT_TASK_HISTORY

Upload
  LHD_SPA_PORT_FILE
  LHD_SPA_PORT_FILE_LINK

Notification
  LHD_SPA_PORT_NOTIFICATION
  LHD_SPA_PORT_MAIL_QUEUE

Audit
  LHD_SPA_PORT_AUDIT
```

Diese Tabellen bilden den Kern des Portals. Ohne diese Struktur wären Antrag, Kontext, Rechte, Arbeitskorb, Uploads und Nachvollziehbarkeit nicht sauber kalkulierbar.

---

## 11. Bewusst nicht zwingend V1

Folgende Tabellen sind fachlich sinnvoll, aber nicht zwingend für eine erste produktive V1 erforderlich.

| Tabelle | Bewertung |
|---|---|
| `LHD_SPA_PORT_2FA_CHALLENGE` | nur erforderlich, wenn 2FA lokal implementiert wird |
| `LHD_SPA_PORT_USER_LOGIN` | sinnvoll für Sicherheit, kann auch über Audit abgebildet werden |
| `LHD_SPA_PORT_CONTEXT_RIGHT` | nur erforderlich bei granularen Objektrechten |
| `LHD_SPA_PORT_WIZARD_VERSION` | sinnvoll für Langfristigkeit, kann anfangs über `CONFIG_VERSION` gelöst werden |
| `LHD_SPA_PORT_WORKFLOW_STATE` | erforderlich bei konfigurierbarem Workflow, sonst Code/ReferenceData möglich |
| `LHD_SPA_PORT_WORKFLOW_TRANSITION` | erforderlich bei konfigurierbarem Workflow, sonst Code/ReferenceData möglich |
| `LHD_SPA_PORT_MAIL_TEMPLATE` | kann anfangs in Konfiguration / Dateien liegen |
| `LHD_SPA_PORT_DASHBOARD_ITEM` | nur bei konfigurierbarem Dashboard |
| `LHD_SPA_PORT_REF_VALUE` | nur für nicht vorhandene Lookup-Werte nötig |
| `LHD_SPA_PORT_LOG` | technisches Logging kann ggf. außerhalb der DB erfolgen |

---

## 12. Offene Punkte vor finaler DDL

| Punkt | Klärung |
|---|---|
| Datentyp für JSON | Oracle 19c: `CLOB` mit JSON-Check oder vorhandener JSON-Strategie festlegen |
| ID-Generierung | Sequenzen und Trigger vs. Identity-Spalten verbindlich festlegen |
| externe Identitäten | genaue Abbildung BundID / MUK / OAuth klären |
| 2FA | lokal im Portal oder über Identity Provider |
| Dateiablage | DB-BLOB, Dateisystem oder Object Storage festlegen |
| Organisationsprüfung | fachlicher Freischaltprozess und Prüfrechte klären |
| SportFM-Nutzerzuordnung | eindeutige Regeln Portalorganisation → bestehender SportFM-Nutzer definieren |
| Workflowkonfiguration | V1 statisch oder tabellenbasiert konfigurierbar entscheiden |
| Wizardkonfiguration | Mindestumfang relational vs. `CONFIG_JSON` entscheiden |
| Datenschutz / Löschung | Aufbewahrung, Sperrung, Löschfristen und Anonymisierung definieren |
| Audit-Aufbewahrung | Fristen und Zugriff auf Auditdaten festlegen |

---

## 13. Zusammenfassung

Das bestehende SportFM-Datenmodell ist für Buchungen, Belegungen, Wiederholungen, Winner-Logik, Gebühren, Rechnungen, Dokumente und SAP-Bezüge tragfähig und bleibt führend.

Für das Portal müssen zusätzliche Tabellen entstehen, weil SportFM bisher keine eigene Struktur für Portalbenutzer, Organisationen, digitale Anträge, Workflows, Uploads, Benachrichtigungen und Portal-Audit besitzt.

Die wichtigsten neuen Datenbereiche sind:

- Authentication / PortalUser,
- Organisation / Membership / Context,
- Wizard,
- Application,
- Workflow / Arbeitskorb,
- Upload,
- Notification / Mail,
- Audit / Logging.

Die Tabellen werden bewusst domänenorientiert aufgebaut und den Packages aus `Packages.md` zugeordnet. Damit entsteht eine konsistente Linie:

```text
Domäne
  ↓
Tabelle
  ↓
Package
  ↓
REST-Service
  ↓
Portal-Funktion
  ↓
Arbeitspaket / Kalkulation
```
