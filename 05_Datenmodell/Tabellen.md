# Tabellen

> Status: Arbeitsstand für das Projekthandbuch SportFM  
> Kapitel: `05_Datenmodell/Tabellen.md`  
> Grundlage: GitHub-Master, Snapshot vom 05.07.2026, Lastenheft, Oracle-DDLs aus `GIT_IMSP.zip`, Domänenkapitel und fachliche Korrekturen.

## 1. Ziel des Kapitels

Dieses Kapitel dokumentiert die für SportFM relevanten Tabellen und ordnet sie fachlich den Domänen zu.

Es beschreibt:

- vorhandene SportFM-Tabellen aus dem Oracle-Bestand,
- fachliche Bedeutung der Tabellen,
- zentrale Beziehungen,
- geplante neue Portal-Tabellen für V1,
- bewusst nicht in V1 enthaltene Tabellen,
- relevante PL/SQL-Packages im Zusammenhang mit dem Datenmodell.

Dieses Kapitel ist **keine vollständige DDL-Spezifikation**. Die physische Struktur wird durch die Oracle-DDLs bestimmt. Dieses Kapitel dient als fachliche und technische Orientierung für Pflichtenheft, REST-API, Umsetzung und Aufwandsschätzung.

## 2. Grundsätze

### 2.1 SportFM bleibt führend

Das bestehende SportFM-Datenmodell bleibt die fachliche Grundlage. Das Portal erzeugt keine zweite Buchungslogik und keine eigene Gebühren- oder Rechnungslogik.

### 2.2 REST-API ist Zugriffsschicht, keine Tabellen-API

Die REST-API bildet fachliche Operationen ab. Sie gibt keine Tabellenstruktur direkt nach außen frei.

Beispiele:

```text
Antrag einreichen
Reservierung prüfen
Buchung erstellen
Stornierung erfassen
Gebühren berechnen
Dokument bereitstellen
Rechnung anzeigen
```

Nicht Ziel ist eine reine CRUD-API auf Tabellen wie `LHD_SPA_EVENTS` oder `LHD_SPA_INVOICES`.

### 2.3 Fachlogik bleibt in SportFM / Oracle

Vorhandene Fachlogik wird weiterverwendet, insbesondere:

| Objekt | Bedeutung |
|---|---|
| `PA_LHD_SPA` | bestehendes zentrales Package für Wiederholungen, Stornierungen, Gebühren und weitere Buchungslogik |
| `PA_LHD_SPA_OCC` | bestehendes Package für performante Occurrence- und Winner-Ermittlung |
| `p_get_charges` | bestehende Gebührenfunktion im fachlichen Kontext der Gebührenermittlung |
| stündlicher Occurrence-Job | Aktualisierung der performanten Termin-/Belegungsstruktur |
| Fast Path | kurzfristige Aktualisierung der Occurrence-/Winner-Logik |

### 2.4 Namenskonvention

Alle neuen Datenbankobjekte führen die bestehende SportFM-Namenskonvention fort.

| Objekttyp | Konvention | Beispiel |
|---|---|---|
| Tabellen | `LHD_SPA_...` | `LHD_SPA_PORT_APPLICATION` |
| Packages | `PA_LHD_SPA_...` | `PA_LHD_SPA_PORT_APPLICATION` |
| Trigger | `TG_LHD_SPA_...` | `TG_LHD_SPA_PORT_APPLICATION` |
| Views | `VW_LHD_SPA_...` | `VW_LHD_SPA_PORT_APPLICATIONS` |

## 3. Fachliches Strukturmodell

### 3.1 Sportkomplex, Sportanlage, Teileinheit

Für das Projekthandbuch gelten folgende Begriffe:

| Begriff | Bedeutung | Buchbar |
|---|---|---:|
| Sportkomplex | organisatorische Zusammenfassung mehrerer Sportanlagen, z. B. Stadion oder Sportpark | nein |
| Sportanlage | fachliche Anlage innerhalb eines Sportkomplexes, z. B. Laufbahn, Rasenplatz, Dreifeldhalle | nein |
| Teileinheit | kleinste buchbare Einheit, z. B. Hallendrittel, Bahn, Platz, Teilfläche | ja |

Fachlicher Grundsatz:

> Buchungsobjekt ist die Teileinheit. Sportanlagen und Sportkomplexe dienen Strukturierung, Suche, Darstellung und Auswertung.

### 3.2 Facility ↔ Booking

Eine Buchung wird nicht fachlich als Buchung „auf Facility“ verstanden, sondern auf den buchbaren Teileinheiten einer Sportanlage.

Relevante Tabellen:

| Tabelle | Bedeutung |
|---|---|
| `LHD_SPA_EVENTS` | fachlicher Buchungskern / Event |
| `LHD_SPA_EVENT2UNIT` | Zuordnung Event zu Teileinheit |
| `LHD_SPA_OCC` | konkrete berechnete Belegungsvorkommen |
| `LHD_SPA_OCC_WINNER` | resultierende gültige Belegung nach Winner-Logik |
| `LHD_SPA_SPORTSCOMPLEXES` | Sportkomplexe |
| `LHD_SPA_FACILITY2COMPLEX` | Zuordnung Sportanlage zu Sportkomplex |

### 3.3 Charge ↔ Invoice

Gebühren und Rechnungen sind fachlich getrennt.

| Bereich | Tabellen | Bedeutung |
|---|---|---|
| Gebührenkonfiguration | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS`, `LHD_SPA_CHARGE2FACILITYGROUP` | Regeln, Beträge, Gültigkeit, Anlagengruppen |
| Gebühren je Event | `LHD_SPA_EVENTCHARGES` | zugeordnete Gebühren einer Buchung / eines Events |
| Rechnung | `LHD_SPA_INVOICES` | Rechnungskopf inkl. SAP-Bezug |
| Rechnungsposition / Berechnungsergebnis | `LHD_SPA_INVOICE_CHARGEINFOS` | abgerechnete Gebühreninformationen je Zeitraum/Event/Gebühr |
| Dokument zur Rechnung | `LHD_SPA_DOCUMENTS.ID_INVOICE` | Dokumentbezug zur Rechnung |

## 4. Statuswerte

### 4.1 Events

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv |
| `-1` | gelöscht |

Buchungen werden aktuell nicht fachlich historisiert. Sie enden zu einem bestimmten Datum oder werden über fachliche Stornierungen abgebildet.

### 4.2 Dokumente

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |
| `2` | bearbeitet |
| `3` | gedruckt |

### 4.3 Rechnungen

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |

## 5. Bestehende Tabellen nach Domänen

### 5.1 Booking / Events / Belegung

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_EVENTS` | Bestand | Zentrale Tabelle für alle belegungsrelevanten Events | `ID_EVENT` PK, `ID_PARENT_EVENT`, `ID_BOOKING`, `BOOKING_NUMBER`, `ID_SPA`, `ID_USER`, `ID_EVENTTYPE`, Zeitraum, Uhrzeit, Sportart, Status |
| `LHD_SPA_EVENTS_HIST` | Bestand | Historische / technische Änderungssicht zu Events | ähnliche Struktur wie `LHD_SPA_EVENTS`, ergänzt um Änderungsinformationen |
| `LHD_SPA_EVENTTYPES` | Bestand | Belegungs- / Eventtypen | `ID_EVENTTYPE`, `ID_EVENTCLASS`, `DESCRIPTION`, `RECURRING_INTERVAL`, `PRIO`, `STATE` |
| `LHD_SPA_EVENTCLASSES` | Bestand | Klassen von Eventtypen | `ID_EVENTCLASS`, `DESCRIPTION`, `PRIO`, `IS_RECURRING` |
| `LHD_SPA_EVENT2UNIT` | Bestand | Zuordnung eines Events zu buchbaren Teileinheiten | `ID_EVENT` → `LHD_SPA_EVENTS`, `ID_UNIT` → RIB-FM-Objekt |
| `LHD_SPA_RECURRING_PATTERN` | Bestand | Wiederholungsmuster zu einem Event | `ID_EVENT`, `FREQ`, `INTERVAL`, `MAX_OCCURRENCES`, `DAYS`, `HOLIDAYS` |
| `LHD_SPA_BOOKING_NUMBERS` | Bestand | Zähler für Buchungsnummern je Jahr | `YEAR`, `LAST_ID_BOOKING` |
| `LHD_SPA_CONTRACT_IDS` | Bestand | Vertrags-/Kontextbezug Nutzer, Anlage, Vertrag | `ID_USER`, `ID_SPA`, `ID_CONTRACT`, Kennzeichen Schule/Nutzer |
| `LHD_SPA_CONTRACT_IDS_XXX` | Bestand / Altbestand | technische oder historische Variante von `LHD_SPA_CONTRACT_IDS` | gleiche fachliche Einordnung wie `LHD_SPA_CONTRACT_IDS`; genaue Nutzung bei Bedarf prüfen |

Fachliche Hinweise:

- Sperrungen, Reinigung, Schulbetrieb, GTA, wöchentliche Übungsbelegung, 14-tägige Übungsbelegung, Veranstaltungen und Stornierungen werden als Events geführt.
- Eine Stornierung ist fachlich ein eigener Eventtyp und keine reine Statusänderung.
- `LHD_SPA_EVENTS` darf nicht direkt nach außen als REST-Ressource offengelegt werden.

### 5.2 Occurrence / Availability / Winner

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_OCC` | Bestand | Materialisierte konkrete Belegungsvorkommen | `ID` PK, `EVENT_ID`, `SPA_ID`, `UNIT_ID`, `START_TS`, `END_TS`, `DAY_DATE`, `USER_ID`, `EVENTTYPE_ID`, Prioritäten |
| `LHD_SPA_OCC_WINNER` | Bestand | Resultierende gültige Belegung nach Winner-Logik | `WINNER_ID`, `SPA_ID`, `UNIT_ID`, `DAY_DATE`, `START_TS`, `END_TS`, `EVENT_ID`, `OCC_ID` |
| `LHD_SPA_OCC_DAY_COVERAGE` | Bestand | Kennzeichnung berechneter Tage je Sportanlage | `SPA_ID`, `DAY_DATE`, `BUILT_TS` |
| `LHD_SPA_OCC_EVENT_DRT` | Bestand | technische Dirty-/Retry-Tabelle für Event-Neuberechnung | `EVENT_ID`, `TRIES`, `LAST_ERROR`, `UPDATED_TS` |
| `LHD_SPA_OCC_WINNER_DRT` | Bestand | technische Dirty-/Retry-Tabelle für Winner-Neuberechnung | `SPA_ID`, `DAY_DATE`, `TRIES`, `LAST_ERROR`, `UPDATED_TS` |

Fachliche Hinweise:

- Die Occurrence-/Winner-Logik ist vorhanden und umgesetzt.
- Es gibt eine stündliche Aktualisierung und einen Fast Path.
- Die Suche nach freien Zeiten und Kalenderansichten müssen diese Logik nutzen.
- Portal und REST-API dürfen keine zweite Belegungsberechnung implementieren.

### 5.3 Documents

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_DOCUMENTS` | Bestand | Dokumente zu Nutzern, Rechnungen und Vorgängen | `ID_DOCUMENT` PK, `ID_SPORTSUSER`, `ID_DOCUMENT_TYPE`, `ID_DOCUMENT_STATE`, `TITLE`, `FILENAME`, `MIMETYPE`, `CONTENT`, Version, `ID_INVOICE` |
| `LHD_SPA_DOCUMENTS_EVENTS` | Bestand | Zuordnung Dokument zu Event | `ID_DOCUMENT` → `LHD_SPA_DOCUMENTS`, `ID_EVENT` → `LHD_SPA_EVENTS` |
| `LHD_SPA_DOCUMENT_TYPES` | Bestand | Dokumenttypen | `ID_DOCUMENT_TYPE`, `TITLE`, `DESCRIPTION` |
| `LHD_SPA_DOCUMENT_TEMPLATES` | Bestand | Dokumentvorlagen | `ID_DOCUMENT_TEMPLATE`, `TITLE`, `FILENAME`, `MIMETYPE`, `CONTENT`, `ID_DOCUMENT_TYPE`, `DEPARTMENT` |
| `LHD_SPA_DOCUMENT_TEXTMODULES` | Bestand | Textmodule für Dokumente | `ID_DOCUMENT_TEXTMODULE`, `TITLE`, `ID_DOCUMENT_TYPE`, `CONTENT` |
| `LHD_SPA_DOCUMENT_NUMBERS` | Bestand | Dokumentnummernzähler je Jahr | `YEAR`, `LAST_DOCUMENT_NR` |
| `LHD_SPA_DOCUMENTS_RE_2025` | Bestand / Sonderbestand | Rechnungs-/Dokumentbestand 2025 | `ID_INVOICE`, `ID_DOCUMENT`, `DOCUMENT_NUMBER`, `STATE`, `CONTENT` |

Fachliche Hinweise:

- Dokumente sind grundsätzlich portalrelevant.
- Portalnutzer sollen ihre eigenen Dokumente sehen können.
- Die Zugriffskontrolle erfolgt über Benutzer-/Organisationskontext, nicht über eine zweite Dokumentenablage.

### 5.4 Charges / Gebühren

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_CHARGES` | Bestand | Gebührenregeln / Gebührensätze | `ID_CHARGE` PK, `ID_CHARGEGROUP`, `VALID_FROM`, `VALID_UNTIL`, `AMOUNT`, `BRUTTO`, `VAT`, `ID_FACILITYGROUP`, Berechnungsfelder, Zeitfenster |
| `LHD_SPA_CHARGETYPES` | Bestand | Gebührentypen | `ID_CHARGETYPE`, `DESCRIPTION`, `BASICCHARGE`, `STANDARD` |
| `LHD_SPA_CHARGEGROUPS` | Bestand | Gebührengruppen | `ID_CHARGEGROUP`, `DESCRIPTION`, Gültigkeitszeitraum, Kurzbezeichnung |
| `LHD_SPA_CHARGE2FACILITYGROUP` | Bestand | Zuordnung Gebühren zu Sportanlagengruppen | `ID_FACILITYGROUP`, `ID_CHARGE`, Kennzeichen Standard / Grundgebühr |
| `LHD_SPA_EVENTCHARGES` | Bestand | Zuordnung Gebühren zu Events | `ID_EVENT`, `ID_CHARGE`, Benutzer-/Zeitinformationen |
| `LHD_SPA_EVENTCHARGES_HIST` | Bestand | Historie zu Event-Gebühren | `ID_EVENT`, `ID_CHARGE`, Änderungsinformationen |

Fachliche Hinweise:

- Gebühren werden in SportFM berechnet.
- Das Portal darf Gebühren nur anzeigen, wenn sie durch SportFM berechnet oder bereitgestellt wurden.
- `p_get_charges` ist im Kontext der bestehenden Gebührenermittlung zu berücksichtigen.

### 5.5 Invoices / Rechnungen / SAP

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_INVOICES` | Bestand | Rechnungskopf | `ID_INVOICE` PK, `ID_USER`, Abrechnungszeitraum, `STATE`, SAP-Belegnummern, SAP-Storno, Bemerkung |
| `LHD_SPA_INVOICE_CHARGEINFOS` | Bestand | Rechnungs-/Gebühreninformationen je Event und Gebühr | `ID_EVENT`, `ID_CHARGE`, Abrechnungszeitraum, Dauer, Anzahl Termine, Beträge, `ID_INVOICE` |
| `LHD_SPA_INVOICES_2025` | Bestand / Sonderbestand | Rechnungsbestand 2025 | gleiche fachliche Einordnung wie `LHD_SPA_INVOICES` |
| `LHD_SPA_INVOICE_CHARGEINFOS_2025` | Bestand / Sonderbestand | Rechnungspositionsbestand 2025 | gleiche fachliche Einordnung wie `LHD_SPA_INVOICE_CHARGEINFOS` |
| `LHD_SPA_INVOICE_CI_HIST` | Bestand | Historie zu Rechnungs-/Gebühreninformationen | `ID_EVENT`, `ID_CHARGE`, Zeitraum, Betrag, Status |

Fachliche Hinweise:

- Die SAP-Integration existiert bereits über ein Modul mit API-Client und OAuth-Client.
- SAP erfolgt über REST-API an SAP.
- Das Portal zeigt Rechnungen an, erzeugt aber keine eigene Rechnungslogik.
- Jeder berechtigte Benutzer sieht seine eigenen Rechnungen.

### 5.6 Facility / Sportstruktur / Sportarten

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_SPORTSCOMPLEXES` | Bestand | Sportkomplexe | `ID_COMPLEX`, `DESCRIPTION`, `COMPLEX_NR`, `ID_COMPLEX_FACILITY` |
| `LHD_SPA_FACILITY2COMPLEX` | Bestand | Zuordnung Sportanlage zu Sportkomplex | `ID_COMPLEX`, `ID_FACILITY` |
| `LHD_SPA_FACILITYGROUPS` | Bestand | Gruppen von Sportanlagen für Gebühren / Strukturierung | `ID_FACILITYGROUP`, `DESCRIPTION`, Gültigkeit |
| `LHD_SPA_SPORTCATEGORIES` | Bestand | Sportkategorien | `ID_SPORTCATEGORY`, `DESCRIPTION` |
| `LHD_SPA_SPORTGROUPS` | Bestand | Sportgruppen | `ID_SPORTGROUP`, `DESCRIPTION` |
| `LHD_SPA_SPORTSUBGROUPS` | Bestand | Sportuntergruppen | `ID_SPORTSUBGROUP`, `ID_SPORTGROUP`, `DESCRIPTION` |
| `LHD_SPA_SPORTTYPES` | Bestand | Sportarten | `ID_SPORTTYPE`, `DESCRIPTION`, `ID_SPORTCATEGORY`, `KATEGORIE`, `IN_USE`, `STATE`, `LSB_NR_2019` |

Fachliche Hinweise:

- Stammdaten zu Sportanlagen und Teileinheiten liegen führend in RIB FM / SportFM.
- `OBJECT.OBJ_ID` aus RIB FM ist für Sportanlage und Teileinheit relevant.
- Keine Neuentwicklung der Stammdatenpflege im Portal.

### 5.7 System / Verwaltung / Logging

| Tabelle | Status | Zweck | Wichtige Felder / Beziehungen |
|---|---|---|---|
| `LHD_SPA_LOGGING` | Bestand | technisches / fachliches Logging | `ID_LOG`, `DATE1`, `USER1`, `MODUL`, `FUNCTION`, `PARAMETERS`, `MESSAGE` |
| `LHD_SPA_USER_SETTINGS` | Bestand | Benutzereinstellungen | `USERNAME`, `MODULE`, `SUBJECT`, `ITEM`, `VALUE`, `VALUE_LOB` |
| `LHD_SPA_VERSION` | Bestand | Versionsinformationen | `VERSION_NUMBER`, `DATE1`, `REMARK` |
| `LHD_SPA_CHANGEREQUESTS` | Bestand | Änderungsanforderungen / interne CR-Liste | `ID_CR`, Datumsfelder, Modul, Beschreibung, Status |
| `LHD_SPA_TEXT` | Bestand | Textbausteine / Oberflächentexte | `MODUL`, `TEXTMARK`, `TEXT` |

## 6. Geplante neue Portal-Tabellen V1

Die folgenden Tabellen sind der bereinigte V1-Stand aus den bisher erarbeiteten Domänen. Sie erweitern SportFM um Portal-, Authentifizierungs-, Antrags-, Workflow-, Upload-, Notification- und Audit-Funktionen.

| Tabelle | Priorität | Domäne | Zweck |
|---|---|---|---|
| `LHD_SPA_PORT_USER` | Muss | Authentication / PortalUser | Portalnutzer |
| `LHD_SPA_PORT_REFRESH_TOKEN` | Muss | Authentication | Sitzungen / Refresh Tokens |
| `LHD_SPA_PORT_PASSWORD_RESET` | Muss | Authentication | Passwort-zurücksetzen-Vorgänge |
| `LHD_SPA_PORT_ORGANIZATION` | Muss | PortalUser / Organisation | Organisation / Verein / Nutzerkontext im Portal |
| `LHD_SPA_PORT_DEPARTMENT` | Muss | PortalUser / Organisation | Abteilungen innerhalb einer Organisation |
| `LHD_SPA_PORT_MEMBERSHIP` | Muss | PortalUser / Organisation | Zuordnung Benutzer zu Organisation / Abteilung |
| `LHD_SPA_PORT_MEMBERSHIP_ROLE` | Muss | Rollen / Berechtigungen | Rollen je Mitgliedschaft |
| `LHD_SPA_PORT_INVITATION` | Muss | PortalUser / Organisation | Einladung weiterer Portalnutzer |
| `LHD_SPA_PORT_CONTEXT` | Muss | Kontext | Zuordnung Portalnutzer / Organisation zum SportFM-Nutzerkontext |
| `LHD_SPA_PORT_APPLICATION` | Muss | Application | Antrag inkl. strukturierter Payload |
| `LHD_SPA_PORT_APPLICATION_HISTORY` | Muss | Application / Workflow | fachliche Antragshistorie |
| `LHD_SPA_PORT_WORKFLOW` | Muss | Workflow | Workflowstatus zum Antrag |
| `LHD_SPA_PORT_TASK` | Muss | Workflow / Arbeitskorb | Aufgaben / Arbeitskorb |
| `LHD_SPA_PORT_NOTIFICATION` | Muss | Notification | Portalnachrichten |
| `LHD_SPA_PORT_MAIL_QUEUE` | Muss | Notification | asynchroner Mailversand |
| `LHD_SPA_PORT_MAIL_TEMPLATE` | Sollte | Notification | Mailvorlagen |
| `LHD_SPA_PORT_FILE` | Muss | Upload / Dokumentbezug | Datei-Metadaten |
| `LHD_SPA_PORT_FILE_LINK` | Muss | Upload / Application / Workflow | Datei-Zuordnung zu Antrag / Workflow |
| `LHD_SPA_PORT_AUDIT` | Muss | Audit / Revision | fachliches Audit |

### 6.1 V1-Entscheidung zum Wizard

Für V1 wird kein vollständiges generisches Wizard-Datenmodell in relationalen Tabellen definiert.

Der Antrag wird über eine strukturierte Payload gespeichert. Die Wizard-Konfiguration kann in V1 außerhalb eines generischen Tabellenmodells gehalten werden. Dadurch bleibt V1 umsetzbar und reduziert Komplexität.

## 7. Bewusst nicht Bestandteil V1

| Objekt / Tabellenbereich | Grund |
|---|---|
| generische Wizard-Tabellen | V1 nutzt Payload-/Konfigurationsansatz; vollständiger Wizard-Designer ist nicht V1 |
| generische ApplicationData-Tabellen | Payload reicht für V1 |
| Workflow-Designer-Tabellen | nicht Bestandteil V1 |
| Regelengine-Tabellen | nicht Bestandteil V1 |
| Dashboard-Konfigurationstabellen | Dashboard-Aggregation erfolgt in .NET / Service-Schicht |
| generische Portal-Konfigurationstabellen | nicht Bestandteil V1 |

## 8. Neue Packages V1 im Datenmodellkontext

| Package | Status | Zweck |
|---|---|---|
| `PA_LHD_SPA_PORT_AUTH` | neu | Login, Registrierung, Passwort, Token |
| `PA_LHD_SPA_PORT_ORG` | neu | Organisationen und Abteilungen |
| `PA_LHD_SPA_PORT_MEMBER` | neu | Mitgliedschaften, Rollen, Einladungen |
| `PA_LHD_SPA_PORT_CONTEXT` | neu | Kontextprüfung / Kontextwechsel |
| `PA_LHD_SPA_PORT_APPLICATION` | neu | Antrag speichern, laden, einreichen |
| `PA_LHD_SPA_PORT_WORKFLOW` | neu | Workflow, Aufgaben, Status |
| `PA_LHD_SPA_PORT_NOTIFICATION` | neu | Portalnachrichten, Mailqueue |
| `PA_LHD_SPA_PORT_UPLOAD` | neu | Upload-Metadaten, Datei-Zuordnung |
| `PA_LHD_SPA_PORT_AUDIT` | neu | Audit schreiben / lesen |
| `PA_LHD_SPA_PORT_REF` | neu / Erweiterung | Auswahldaten / Reference Data |
| `PA_LHD_SPA` | Bestand / Erweiterung | Booking, Stornierung, Gebühren, `p_get_charges` |
| `PA_LHD_SPA_OCC` | Bestand / Erweiterung | Availability, Occurrences, Winner |

Nicht V1:

| Package | Grund |
|---|---|
| `PA_LHD_SPA_PORT_WIZARD` | Wizard-Konfiguration V1 nicht als DB-Modul |
| `PA_LHD_SPA_PORT_CONFIG` | keine generische Konfigurationsverwaltung in V1 |
| `PA_LHD_SPA_PORT_DASHBOARD` | Dashboard wird in .NET aggregiert |
| `PA_LHD_SPA_PORT_ADMIN` | Administration nutzt vorhandene Services |

## 9. Zentrale Beziehungen

### 9.1 Booking-Kern

```text
LHD_SPA_EVENTS
  ├─ LHD_SPA_EVENT2UNIT
  │    └─ RIB FM OBJECT / Teileinheit
  ├─ LHD_SPA_RECURRING_PATTERN
  ├─ LHD_SPA_EVENTCHARGES
  │    └─ LHD_SPA_CHARGES
  ├─ LHD_SPA_DOCUMENTS_EVENTS
  │    └─ LHD_SPA_DOCUMENTS
  └─ LHD_SPA_OCC / LHD_SPA_OCC_WINNER
```

### 9.2 Gebühren und Rechnung

```text
LHD_SPA_CHARGEGROUPS
  └─ LHD_SPA_CHARGES
       ├─ LHD_SPA_CHARGETYPES
       ├─ LHD_SPA_CHARGE2FACILITYGROUP
       └─ LHD_SPA_EVENTCHARGES

LHD_SPA_INVOICES
  ├─ LHD_SPA_INVOICE_CHARGEINFOS
  └─ LHD_SPA_DOCUMENTS
```

### 9.3 Portal-Antrag zu SportFM

```text
LHD_SPA_PORT_USER
  └─ LHD_SPA_PORT_MEMBERSHIP
       └─ LHD_SPA_PORT_ORGANIZATION
            └─ LHD_SPA_PORT_CONTEXT
                 └─ SportFM-Nutzerkontext

LHD_SPA_PORT_APPLICATION
  ├─ LHD_SPA_PORT_APPLICATION_HISTORY
  ├─ LHD_SPA_PORT_WORKFLOW
  ├─ LHD_SPA_PORT_TASK
  ├─ LHD_SPA_PORT_FILE_LINK
  └─ Übergabe an Booking / LHD_SPA_EVENTS nach fachlicher Prüfung
```

## 10. Offene Punkte

Diese Punkte sind vor physischer DDL-Erstellung oder finaler REST-Spezifikation zu klären:

| Punkt | Klärung |
|---|---|
| Detailspalten der neuen Portal-Tabellen | erst im technischen Datenmodell / DDL festlegen |
| genaue Speicherung der Application-Payload | JSON-Struktur und Versionierung definieren |
| Zuordnung Portalorganisation zu bestehendem SportFM-Nutzer | fachlich exakt festlegen |
| Rollenmodell je Organisation / Abteilung | mit Kapitel Rollen und Berechtigungen synchronisieren |
| Dokumentfreigabe im Portal | alle Dokumente grundsätzlich sichtbar, Zugriff aber strikt kontextbezogen |
| Rechnungsanzeige im Portal | alle eigenen Rechnungen sichtbar, ePayBL-Prozess separat betrachten |
| historische Behandlung von Portal-Anträgen | über Application History / Audit festlegen |

## 11. Zusammenfassung

Das bestehende Datenmodell ist fachlich tragfähig und enthält bereits die zentralen Strukturen für Buchung, Belegung, Wiederholungen, Winner-Logik, Gebühren, Rechnungen, Dokumente und Logging.

Für V1 entstehen neue Tabellen vor allem dort, wo SportFM heute keine Portalstruktur besitzt:

- Portalbenutzer,
- Organisationen und Mitgliedschaften,
- Kontextzuordnung zu SportFM,
- digitale Anträge,
- Workflow / Arbeitskorb,
- Uploads,
- Benachrichtigungen,
- Audit.

Die bestehende Buchungs-, Belegungs-, Gebühren-, Rechnungs- und Dokumentenlogik wird nicht neu modelliert, sondern über REST und gezielte Packages für Portal und WPF nutzbar gemacht.
