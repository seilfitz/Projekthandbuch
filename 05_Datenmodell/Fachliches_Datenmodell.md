# Fachliches Datenmodell

| Feld | Wert |
|---|---|
| Kapitel | 05 – Datenmodell |
| Dokument | Fachliches Datenmodell |
| Status | Arbeitsstand 1.0 |
| Typ | Fachliches Vertiefungsdokument |
| Grundlage | Lastenheft, Snapshot, Oracle-DDLs, Domänenkapitel, Architekturkapitel, fachliche Korrekturen |

---

## 1 Zweck

Dieses Dokument beschreibt das **fachliche Datenmodell** der SportFM-Plattform.

Es beschreibt nicht primär Tabellen, sondern die fachlichen Objekte, ihre Bedeutung, Lebenszyklen, Beziehungen und Datenhoheit.

Das Dokument bildet die Brücke zwischen:

- den fachlichen Domänen,
- dem Oracle-Bestandsmodell,
- der späteren REST-API,
- dem Onlineportal,
- der WPF-Migration,
- und den Arbeitspaketen.

Technische Details zu Tabellen, Spalten, Indizes, Packages und Migration gehören in die nachgelagerten Dokumente:

- `Oracle_Datenmodell.md`,
- `Tabellen.md`,
- `Packages.md`,
- `Datenmigration.md`.

---

## 2 Quellen und Verbindlichkeit

Dieses Datenmodell basiert ausschließlich auf den vorliegenden Quellen und fachlichen Festlegungen.

Verwendete Hauptquellen:

- `2026-07-05_Snapshot1.txt`,
- Lastenheft SportFM,
- Oracle-DDLs aus `GIT_IMSP.zip`,
- Domänenbeschreibungen aus Kapitel 03,
- Architekturkapitel,
- fachliche Korrekturen aus der Projektabstimmung,
- bekannte PL/SQL-Packages `PA_LHD_SPA` und `PA_LHD_SPA_OCC`.

Nicht gesicherte Sachverhalte werden nicht als Fakt beschrieben, sondern als Klärungspunkt geführt.

---

## 3 Grundprinzipien

### 3.1 Fachliches Modell vor technischem Schema

Das fachliche Datenmodell beschreibt, **was** ein Objekt fachlich bedeutet.

Es beschreibt nicht vollständig, **wie** es technisch gespeichert wird.

Beispiel:

```text
Antrag
  ↓
Bearbeitung / Workflow
  ↓
Reservierung oder Buchung
  ↓
Occurrence / Winner
  ↓
Gebühr
  ↓
Rechnung
  ↓
Dokument / Portalansicht
```

### 3.2 Bestand bleibt führend

Für bestehende SportFM-Kernbereiche bleibt Oracle mit PL/SQL führend.

Das betrifft insbesondere:

- Buchungen / Events,
- Wiederholungen,
- Occurrences,
- Winner,
- Gebühren,
- Rechnungen,
- Dokumente,
- Sportstättenbezüge,
- SAP-Anbindung.

### 3.3 Portal ergänzt, ersetzt aber nicht

Das Onlineportal erzeugt neue fachliche Datenobjekte.

Es ersetzt keine bestehenden SportFM-Kernobjekte.

Neue Portalobjekte sind insbesondere:

- Portalbenutzer,
- Organisation / Kontext,
- Antrag,
- Wizard-Konfiguration,
- Workflow,
- Upload,
- Portalnachricht.

### 3.4 Keine doppelte Fachlogik

Fachliche Regeln werden nicht im Portal nachgebaut.

Das betrifft insbesondere:

- Wiederholungslogik,
- Occurrence-Ermittlung,
- Winner-Ermittlung,
- Konfliktlogik,
- Gebührenberechnung,
- Stornierungslogik,
- Dokumentnummernlogik,
- SAP-Anbindung.

### 3.5 REST folgt Fachobjekten, nicht Tabellen

Die REST-API bildet keine reine CRUD-Schicht auf Oracle-Tabellen.

Sie stellt fachliche Operationen bereit, zum Beispiel:

- Antrag einreichen,
- freie Zeiten suchen,
- Reservierung prüfen,
- Buchung erzeugen,
- Stornierung erzeugen,
- Gebühren anzeigen,
- Rechnung anzeigen,
- Dokument herunterladen.

---

## 4 Fachliche Hauptbereiche

| Nr. | Hauptbereich | Beschreibung |
|---:|---|---|
| 1 | Identität und Kontext | Portalbenutzer, Organisationen, Rollen, Arbeitskontext |
| 2 | Antrag und Bearbeitung | Antrag, Wizard, Workflow, Upload |
| 3 | Sportstätte und Belegung | Sportanlage, Teileinheit, Buchung, Event, Occurrence, Winner, Verfügbarkeit |
| 4 | Gebühren, Rechnungen und Dokumente | Gebühren, Rechnungen, SAP-Bezug, Dokumente, Vorlagen |
| 5 | Querschnitt | Benachrichtigungen, Dashboard, Administration, Logging |

---

## 5 Identität und Kontext

### 5.1 Identity

Identity beschreibt die technische Identität eines Portalzugangs.

Fachliche Aufgaben:

- Registrierung,
- Anmeldung,
- Passwortprozesse,
- Sperrstatus,
- Zwei-Faktor-Authentifizierung,
- Token,
- OAuth für technische Zugriffe.

Identity ist nicht identisch mit dem SportFM-Nutzer.

```text
Identity
  ↓
PortalUser
  ↓
Organisation / Context
```

### 5.2 PortalUser

PortalUser beschreibt das fachliche Benutzerprofil im Portal.

Fachliche Inhalte:

- Name,
- Kontaktdaten,
- E-Mail,
- Einstellungen,
- Zustimmungen,
- Zuordnungen zu Organisationen,
- Berechtigungen innerhalb einer Organisation.

Ein PortalUser kann für mehrere Organisationen handeln.

Ein SportFM-Nutzer kann mehreren PortalUsern zugeordnet sein.

### 5.3 Organisation

Organisation beschreibt die fachliche Einheit, für die ein PortalUser handelt.

Beispiele:

- Verein,
- Vereinsabteilung,
- Schule,
- Kita,
- Firma,
- Privatperson als Einzelkontext.

Wichtig:

Ein großer Verein wird im Portal feiner modelliert als im heutigen SportFM-Bestand.

```text
SportFM-Nutzer
  ↓
Organisation
  ↓
Abteilung / Bereich
  ↓
PortalUser
```

### 5.4 Context

Context beschreibt den aktiven fachlichen Arbeits- und Sichtbarkeitsraum.

Beispiel:

Ein PortalUser ist mehreren Organisationen zugeordnet und wählt aktiv aus, für welche Organisation er arbeitet.

```text
PortalUser
  ↓
Context
  ↓
Anträge / Dokumente / Rechnungen / Buchungen
```

Context ist zentral für:

- Berechtigungen,
- Mandantentrennung,
- Sichtbarkeit,
- Portal-Dashboard,
- REST-Zugriffskontrolle.

---

## 6 Antrag und Bearbeitung

### 6.1 Application

Application beschreibt den fachlichen Antrag im Portal.

Ein Antrag ist **kein Event** und **keine Buchung**.

Ein Antrag beschreibt einen digitalen Vorgang, der später zu einer Reservierung oder Buchung führen kann.

Fachliche Inhalte:

- Antragstyp,
- Antragsteller,
- Organisation,
- Kontext,
- gewünschte Sportstätte,
- gewünschte Teileinheit,
- gewünschter Zeitraum,
- gewünschte Uhrzeiten,
- Sportart,
- ergänzende Angaben,
- Uploads,
- Status,
- Verlauf,
- Zuordnung zu späterer Reservierung oder Buchung.

Fachlicher Lebenszyklus:

```text
Entwurf
  ↓
Eingereicht
  ↓
In Prüfung
  ↓
Rückfrage / Nachforderung
  ↓
Genehmigt oder abgelehnt
  ↓
Buchung erzeugt / abgeschlossen
```

Die finalen Statuswerte werden im technischen Detailmodell festgelegt.

### 6.2 Wizard

Wizard beschreibt den konfigurierbaren Antragsassistenten.

Ziel:

Neue Antragstypen sollen langfristig möglichst durch Konfiguration entstehen und nicht durch hart programmierte Einzelmasken.

Fachliche Inhalte:

- Wizarddefinition,
- Version,
- Antragstyp,
- Schritte,
- Felder,
- Pflichtfelder,
- Sichtbarkeitsregeln,
- Validierungen,
- Uploadvorgaben,
- Zusammenfassung,
- Einreichlogik,
- Mapping auf Application-Daten.

```text
WizardDefinition
  ↓
WizardStep
  ↓
WizardField
  ↓
WizardValidation
  ↓
ApplicationPayload
```

Der Wizard erzeugt keinen direkten SportFM-Event.

Er erzeugt strukturierte Antragsdaten.

### 6.3 Workflow

Workflow beschreibt den fachlichen Arbeitsablauf nach Einreichung eines Antrags.

Fachliche Inhalte:

- Vorgang,
- Aufgabe,
- Bearbeiter,
- Rolle,
- Status,
- Rückfrage,
- Entscheidung,
- Historie,
- Frist,
- Eskalation.

Workflow verbindet Portal und Sachbearbeitung.

```text
Application
  ↓
WorkflowInstance
  ↓
WorkflowTask
  ↓
Decision
  ↓
Reservation oder BookingEvent
```

### 6.4 Upload

Upload beschreibt die Annahme und Prüfung von Dateien im Portal.

Upload ist nicht identisch mit Document.

- Upload = eingereichte Datei im Portalvorgang.
- Document = dauerhaft verwaltetes Dokument im SportFM-Dokumentenbestand.

Fachliche Inhalte:

- Datei,
- Dateiname,
- Dateityp,
- Größe,
- Uploadstatus,
- Prüfung,
- Zuordnung zu Antrag, Organisation oder späterem Dokument.

```text
Upload
  ↓
UploadValidationResult
  ↓
UploadAssignment
  ↓
Document oder fachliche Zuordnung
```

---

## 7 Sportstätte und Belegung

### 7.1 Facility

Facility beschreibt die fachlichen Sportstättenobjekte.

Fachliche Inhalte:

- Sportkomplex,
- Sportanlage,
- Teileinheit,
- Sportanlagengruppe,
- Standort,
- Eigenschaften,
- Zuordnung zu Sportarten,
- Zuordnung zu Gebühren und Verfügbarkeit.

Facility beschreibt **wo** eine Nutzung stattfindet.

Booking beschreibt **dass und wann** eine Nutzung stattfindet.

Diese Trennung ist verbindlich:

```text
Facility ≠ Booking
```

### 7.2 Booking / Event

Booking beschreibt die verbindliche Buchungs- und Belegungslogik.

Im Oracle-Bestand wird diese fachliche Domäne zentral über `LHD_SPA_EVENTS` abgebildet.

Fachliche Inhalte:

- Event,
- Buchungsnummer,
- Parent-Event,
- Sportanlage,
- Nutzer,
- Eventtyp,
- Titel,
- Beschreibung,
- Zeitraum,
- Uhrzeit,
- Dauer,
- Sportart,
- Sportgruppe,
- Sportuntergruppe,
- Wiederholungskennzeichen,
- Teileinheitenbezug,
- Status,
- Organisationseinheit,
- Komplexveranstaltung.

Alle belegungsrelevanten Vorgänge werden als Events geführt.

Dazu zählen gesichert:

- Sperrung,
- Reinigung,
- Schulbetrieb,
- GTA,
- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltung,
- Stornierung.

### 7.3 Reservierung

Reservierung beschreibt eine fachliche Vormerkung oder Planung vor der verbindlichen Buchung.

Eine Reservierung ist keine Rechnung und keine endgültige Buchung.

Sie dient der fachlichen Prüfung, Planung und Bearbeitung.

Die konkrete technische Abbildung wird im Detailmodell festgelegt.

### 7.4 RecurringPattern

RecurringPattern beschreibt Wiederholungsmuster.

Im Oracle-Bestand wird dies über `LHD_SPA_RECURRING_PATTERN` abgebildet.

Fachliche Inhalte:

- Eventbezug,
- Frequenz,
- Intervall,
- maximale Vorkommen,
- Wochentage,
- Feiertagsbehandlung.

Die Berechnung erfolgt im Bestand über `PA_LHD_SPA` und die Occurrence-Logik.

### 7.5 Occurrence

Occurrence beschreibt konkrete Einzelvorkommen aus Events und Wiederholungsmustern.

Im Oracle-Bestand wird dies über `LHD_SPA_OCC` abgebildet.

Fachliche Inhalte:

- Eventbezug,
- Sportanlage,
- Teileinheit,
- Startzeit,
- Endzeit,
- Datum,
- Nutzer,
- Eventtyp,
- Priorität,
- Stornierungsbezug,
- Feiertagsinformationen.

Occurrences sind Grundlage für performante Kalender- und Belegungszugriffe.

Die Ermittlung erfolgt über `PA_LHD_SPA_OCC`.

Es existiert ein stündlicher Job sowie ein Fast Path.

### 7.6 OccurrenceWinner

OccurrenceWinner beschreibt die resultierende gültige Belegung.

Im Oracle-Bestand wird dies über `LHD_SPA_OCC_WINNER` abgebildet.

Fachliche Inhalte:

- gültiges Vorkommen,
- Sportanlage,
- Teileinheit,
- Datum,
- Start- und Endzeit,
- Eventbezug,
- Occurrence-Bezug,
- Nutzer,
- Eventtyp,
- Priorität.

Winner ist für Kalender, Belegungsanzeige und freie Zeiten maßgeblich.

Das Portal darf diese Logik nicht selbst berechnen.

### 7.7 Availability

Availability beschreibt Verfügbarkeit und freie Zeiten.

Fachliche Inhalte:

- Suchanfrage,
- Zeitraum,
- Wochentage,
- Uhrzeiten,
- Sportart,
- Sportanlagentyp,
- Stadtteil / Bereich,
- freie Zeitfenster,
- Konflikte,
- Begründung der Nichtverfügbarkeit.

Availability basiert auf Occurrence und Winner.

```text
LHD_SPA_OCC
  ↓
LHD_SPA_OCC_WINNER
  ↓
AvailabilityResult
```

---

## 8 Gebühren, Rechnungen und Dokumente

### 8.1 Charge

Charge beschreibt Gebühren und Gebühreninformationen.

Charge ist keine Rechnung.

Diese Trennung ist verbindlich:

```text
Charge ≠ Invoice
```

Fachliche Inhalte:

- Gebühr,
- Gebührentyp,
- Gebührengruppe,
- Betrag,
- Brutto-/Netto-Kennzeichen,
- Mehrwertsteuer,
- Gültigkeitszeitraum,
- Sportanlagengruppe,
- Berechnungszeitraum,
- Berechnungshäufigkeit,
- Zeiteinheit,
- Tageszeitbezug,
- Teileinheitensplitting,
- EventCharge,
- InvoiceChargeInfo.

Im Oracle-Bestand sind insbesondere relevant:

- `LHD_SPA_CHARGES`,
- `LHD_SPA_CHARGETYPES`,
- `LHD_SPA_CHARGEGROUPS`,
- `LHD_SPA_CHARGE2FACILITYGROUP`,
- `LHD_SPA_EVENTCHARGES`,
- `LHD_SPA_INVOICE_CHARGEINFOS`.

Die Gebührenberechnung erfolgt im Bestand, insbesondere über `PA_LHD_SPA` und `p_get_charges`.

Das Portal darf Gebühren anzeigen, aber nicht selbst berechnen.

### 8.2 Invoice

Invoice beschreibt Rechnungen und deren fachlichen Status.

Fachliche Inhalte:

- Rechnung,
- Rechnungszeitraum,
- Nutzer,
- Status,
- SAP-Beleg,
- SAP-Storno,
- Zahlstatus,
- Rechnungspositionen,
- Rechnungsdokumente,
- Bemerkungen.

Im Oracle-Bestand sind insbesondere relevant:

- `LHD_SPA_INVOICES`,
- `LHD_SPA_INVOICE_CHARGEINFOS`,
- historische / jahresbezogene Rechnungstabellen.

Invoice ist nicht verantwortlich für Gebührenberechnung.

Invoice ist verantwortlich für Rechnungssicht, Belegbezug und Zahlungsbezug.

SAP ist bereits über ein Modul mit API-Client und OAuth-Client angebunden.

Die SAP-Kommunikation erfolgt per REST-API an SAP.

### 8.3 Document

Document beschreibt dauerhaft verwaltete Dokumente.

Fachliche Inhalte:

- Dokument,
- Dokumenttyp,
- Dokumentstatus,
- Dokumentnummer,
- Dateiinhalt,
- MIME-Type,
- Vorlage,
- Textbaustein,
- Version,
- Bezug zu SportFM-Nutzer,
- Bezug zu Event,
- Bezug zu Rechnung.

Im Oracle-Bestand sind insbesondere relevant:

- `LHD_SPA_DOCUMENTS`,
- `LHD_SPA_DOCUMENTS_EVENTS`,
- `LHD_SPA_DOCUMENT_TYPES`,
- `LHD_SPA_DOCUMENT_TEMPLATES`,
- `LHD_SPA_DOCUMENT_TEXTMODULES`,
- `LHD_SPA_DOCUMENT_NUMBERS`.

Dokumente sind grundsätzlich portalgeeignet, sofern der Portalnutzer berechtigt ist.

Jeder Portalnutzer sieht nur die Dokumente seines berechtigten Kontextes.

---

## 9 Querschnittsdomänen

### 9.1 Notification

Notification beschreibt Meldungen und Benachrichtigungen.

Fachliche Inhalte:

- Portalnachricht,
- Empfänger,
- Versandkanal,
- MailQueue,
- Versandstatus,
- Lesestatus,
- Vorlage,
- Bezug zu Antrag, Workflow, Dokument oder Rechnung.

Diese Domäne ist neu und darf nicht in Wizard oder Workflow versteckt werden.

### 9.2 Dashboard

Dashboard ist eine Aggregationsdomäne.

Sie besitzt in V1 keine eigene führende Fachlogik.

Dashboard aggregiert:

- offene Anträge,
- Aufgaben,
- Rückfragen,
- neue Dokumente,
- offene Rechnungen,
- Nachrichten,
- relevante Buchungen.

### 9.3 Administration

Administration beschreibt administrative Daten.

Fachliche Inhalte:

- Systemparameter,
- Konfiguration,
- administrative Sichten,
- Protokollierung,
- Audit,
- Pflege von Referenzdaten, soweit nicht im Bestand führend.

### 9.4 Logging

Logging beschreibt technische und fachliche Protokollierung.

Im Bestand existiert `LHD_SPA_LOGGING`.

Für das Portal sind zusätzliche Audit-Anforderungen zu prüfen, insbesondere für:

- Anmeldung,
- Dokumentdownload,
- Rechnungsanzeige,
- Antragsänderung,
- Statuswechsel,
- administrative Änderungen.

---

## 10 Zentrale Beziehungen

| Von | Nach | Beziehung |
|---|---|---|
| Identity | PortalUser | technische Identität besitzt fachliches Profil |
| PortalUser | Organisation | Benutzer handelt für eine oder mehrere Organisationen |
| Organisation | Context | Organisation bildet Arbeitskontext |
| Context | Application | Antrag wird in einem Kontext gestellt |
| Application | Wizard | Antrag basiert auf Wizarddefinition |
| Application | Workflow | Antrag startet Arbeitsablauf |
| Application | Upload | Antrag kann Dateien enthalten |
| Application | Facility | Antrag bezieht sich auf Sportstätte / Teileinheit |
| Workflow | Reservation | Entscheidung kann Reservierung erzeugen |
| Workflow | Booking / Event | Entscheidung kann Buchung erzeugen |
| Booking / Event | RecurringPattern | Event kann Wiederholungsmuster besitzen |
| Booking / Event | Occurrence | Event erzeugt konkrete Vorkommen |
| Occurrence | OccurrenceWinner | Winner bestimmt gültige Belegung |
| Booking / Event | Charge | Buchung kann Gebühren erzeugen |
| Charge | Invoice | Gebühreninformationen fließen in Rechnung |
| Invoice | Document | Rechnung kann Dokument besitzen |
| Booking / Event | Document | Buchung kann Genehmigung / Widerruf erzeugen |
| Upload | Document | geprüfter Upload kann dauerhaftes Dokument werden |
| Notification | Application / Workflow / Document / Invoice | Meldung bezieht sich auf Fachobjekt |

---

## 11 Gesamtlebenszyklus

Der fachliche Gesamtprozess lautet:

```text
PortalUser
  ↓
wählt Context
  ↓
startet Wizard
  ↓
speichert Application als Entwurf
  ↓
reicht Application ein
  ↓
Workflow startet
  ↓
Sachbearbeitung prüft
  ↓
Rückfrage oder Entscheidung
  ↓
Reservierung / Buchung in SportFM
  ↓
Occurrence- und Winner-Ermittlung
  ↓
Gebührenberechnung
  ↓
Dokumenterzeugung
  ↓
Abrechnung / Rechnung
  ↓
SAP / Zahlung
  ↓
Anzeige im Portal
```

---

## 12 Datenhoheit

| Datenobjekt | Führend | Bemerkung |
|---|---|---|
| Identity | neue Auth-Schicht | technische Identität |
| PortalUser | neue PortalUser-Domäne | fachliches Portalprofil |
| Organisation | neue Organisation-Domäne mit SportFM-Bezug | Zuordnung zu SportFM-Nutzer erforderlich |
| Context | neue Context-Domäne | Sichtbarkeit und Berechtigung |
| Application | neue Application-Domäne | digitaler Portalvorgang |
| Wizard | neue Wizard-Domäne | Konfiguration |
| Workflow | neue Workflow-Domäne | Bearbeitungsablauf |
| Upload | neue Upload-Domäne | Dateiannahme |
| Facility | Oracle / RIB FM / SportFM | Bestand führend |
| Booking / Event | Oracle / SportFM | Bestand führend |
| RecurringPattern | Oracle / `PA_LHD_SPA` | Bestand führend |
| Occurrence | Oracle / `PA_LHD_SPA_OCC` | Bestand führend |
| OccurrenceWinner | Oracle / `PA_LHD_SPA_OCC` | Bestand führend |
| Charge | Oracle / `PA_LHD_SPA` / `p_get_charges` | Bestand führend |
| Invoice | Oracle / SAP-Integration | Bestand führend |
| Document | Oracle / SportFM | Bestand führend |
| Notification | neue Plattformdomäne | querschnittlich |
| Dashboard | Aggregation | keine eigene führende Datenhaltung V1 |
| Logging | Bestand + neue Plattformprotokollierung | Umfang noch festzulegen |

---

## 13 Statusmodelle

### 13.1 Eventstatus

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv |
| `-1` | gelöscht |

Buchungen werden fachlich nicht über komplexe Statusketten historisiert.

Sie enden über Datum oder werden über Stornierungs-Events abgebildet.

### 13.2 Dokumentstatus

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |
| `2` | bearbeitet |
| `3` | gedruckt |

### 13.3 Rechnungsstatus

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |

### 13.4 Portalstatus

Portalstatus sind neue fachliche Status und dürfen nicht mit Bestandsstatus vermischt werden.

Beispiele:

- Entwurf,
- eingereicht,
- in Prüfung,
- Rückfrage,
- genehmigt,
- abgelehnt,
- abgeschlossen.

Finale Werte werden im technischen Detailmodell festgelegt.

---

## 14 Fachliche Abgrenzungen

### 14.1 Antrag ist keine Buchung

Ein Antrag beschreibt den Wunsch eines Portalnutzers.

Eine Buchung beschreibt die verbindliche Nutzung in SportFM.

### 14.2 Reservierung ist keine Rechnung

Eine Reservierung ist fachlich vorbereitend.

Eine Rechnung entsteht erst nach Abrechnung.

### 14.3 Stornierung ist ein Event

Eine Stornierung wird fachlich als eigene Buchung / eigener Eventtyp abgebildet.

Sie ist nicht nur eine Statusänderung am ursprünglichen Event.

### 14.4 Upload ist kein Dokument

Ein Upload ist eine eingereichte Datei.

Ein Dokument ist ein dauerhaft verwaltetes Fachobjekt.

### 14.5 Gebühr ist keine Rechnung

Gebühren beschreiben Berechnungs- und Positionsinformationen.

Rechnungen beschreiben den abrechenbaren Beleg mit SAP-/Zahlungsbezug.

### 14.6 Facility ist keine Booking

Facility beschreibt den Ort / die Ressource.

Booking beschreibt die Nutzung dieser Ressource zu einem Zeitpunkt oder Zeitraum.

---

## 15 Bestandsobjekte aus Oracle-DDL

Die folgende Tabelle ist eine fachliche Zuordnung der gesichteten Oracle-DDLs.

| Fachobjekt | Bestandsobjekte / Tabellen | Fachliche Bedeutung |
|---|---|---|
| Booking / Event | `LHD_SPA_EVENTS`, `LHD_SPA_EVENTS_HIST` | zentrale Buchungs- und Eventdaten |
| Eventtyp / Eventklasse | `LHD_SPA_EVENTTYPES`, `LHD_SPA_EVENTCLASSES` | Art und Priorität von Belegungen |
| Teileinheitenbezug | `LHD_SPA_EVENT2UNIT` | Zuordnung Event zu Teileinheiten |
| Wiederholung | `LHD_SPA_RECURRING_PATTERN` | Muster wiederkehrender Events |
| Occurrence | `LHD_SPA_OCC` | konkrete berechnete Einzelvorkommen |
| Winner | `LHD_SPA_OCC_WINNER` | resultierende gültige Belegung |
| Occurrence-Verwaltung | `LHD_SPA_OCC_DAY_COVERAGE`, `LHD_SPA_OCC_EVENT_DRT`, `LHD_SPA_OCC_WINNER_DRT` | Job-/Fast-Path-nahe Steuerung |
| Gebühren | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS` | Gebührenstammdaten |
| Gebührenbezug | `LHD_SPA_EVENTCHARGES`, `LHD_SPA_CHARGE2FACILITYGROUP` | Zuordnung Gebühren zu Events / Anlagen |
| Rechnungen | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICE_CHARGEINFOS` | Rechnungs- und Positionsinformationen |
| Dokumente | `LHD_SPA_DOCUMENTS`, `LHD_SPA_DOCUMENTS_EVENTS` | Dokumente und Eventbezug |
| Vorlagen | `LHD_SPA_DOCUMENT_TEMPLATES`, `LHD_SPA_DOCUMENT_TEXTMODULES`, `LHD_SPA_DOCUMENT_TYPES` | Dokumentvorlagen und Textbausteine |
| Sportstruktur | `LHD_SPA_SPORTTYPES`, `LHD_SPA_SPORTGROUPS`, `LHD_SPA_SPORTSUBGROUPS`, `LHD_SPA_SPORTCATEGORIES` | Sportarten und fachliche Gruppierung |
| Sportkomplexe | `LHD_SPA_SPORTSCOMPLEXES`, `LHD_SPA_FACILITY2COMPLEX`, `LHD_SPA_FACILITYGROUPS` | Anlagenstruktur und Gruppierung |
| Nummern | `LHD_SPA_BOOKING_NUMBERS`, `LHD_SPA_DOCUMENT_NUMBERS`, `LHD_SPA_CONTRACT_IDS` | Nummernkreise und Vertragsbezüge |
| System / Verwaltung | `LHD_SPA_LOGGING`, `LHD_SPA_USER_SETTINGS`, `LHD_SPA_VERSION`, `LHD_SPA_TEXT`, `LHD_SPA_CHANGEREQUESTS` | Querschnitt und Verwaltung |

---

## 16 Auswirkungen auf REST

Das fachliche Datenmodell führt zu folgenden REST-Grundsätzen:

- REST folgt Domänen, nicht Tabellen.
- REST liefert fachliche DTOs, keine rohen Oracle-Zeilen.
- REST kapselt Oracle und PL/SQL.
- REST prüft Berechtigung über PortalUser, Organisation und Context.
- REST nutzt Bestandslogik über definierte Gateways.
- REST stellt Dokumente und Rechnungen nur berechtigt bereit.
- REST berechnet keine Gebühren im Portal.
- REST berechnet keine Occurrences oder Winner im Portal.

Beispielstruktur:

```text
/api/v1/auth
/api/v1/portal-users
/api/v1/organizations
/api/v1/contexts
/api/v1/applications
/api/v1/wizards
/api/v1/workflows
/api/v1/uploads
/api/v1/facilities
/api/v1/availability
/api/v1/bookings
/api/v1/charges
/api/v1/invoices
/api/v1/documents
/api/v1/notifications
```

---

## 17 Auswirkungen auf Arbeitspakete

| Bereich | Arbeitspaket-Typ |
|---|---|
| Identity / Auth | Neuentwicklung |
| PortalUser / Organisation / Context | Neuentwicklung |
| Application | Neuentwicklung |
| Wizard | Neuentwicklung / Konfiguration |
| Workflow | Neuentwicklung |
| Upload | Neuentwicklung |
| Facility | Bestandsfreilegung |
| Booking / Event | Bestandsfreilegung / spätere WPF-Migration |
| Availability | Bestandsfreilegung / Performance |
| Occurrence / Winner | Bestandsfreilegung, keine Neuentwicklung im Portal |
| Charge | Bestandsfreilegung |
| Invoice | Bestandsfreilegung / Integration |
| Document | Bestandsfreilegung / Download / Berechtigung |
| Notification | Neuentwicklung |
| Dashboard | Aggregation |
| Logging / Audit | Bestand erweitern |

---

## 18 Auswirkungen auf Tests

| Bereich | Testschwerpunkt |
|---|---|
| Identity / PortalUser | Anmeldung, 2FA, Sperren, Berechtigungen |
| Context | Benutzer sieht nur eigene Daten |
| Application | Entwurf, Einreichung, Status, Historie |
| Wizard | Pflichtfelder, Sichtbarkeit, Versionierung, Konfiguration |
| Workflow | Statusübergänge, Rückfragen, Entscheidungen |
| Upload | Dateitypen, Größen, Zuordnung, Fehlerfälle |
| Facility | Filter, Suchlogik, Sichtbarkeit |
| Availability | freie Zeiten, Konflikte, Performance |
| Booking / Event | Bestandstreue, Eventtypen, Stornierung |
| Occurrence / Winner | korrekte Nutzung vorhandener Logik |
| Charge | Gebührenanzeige ohne Neuberechnung im Portal |
| Invoice | nur eigene Rechnungen, SAP-/Zahlstatus |
| Document | nur eigene Dokumente, Download, Status |
| Notification | Versand, Lesestatus, Bezug zu Fachobjekten |
| Logging / Audit | Nachvollziehbarkeit sicherheitsrelevanter Vorgänge |

---

## 19 Offene Klärungspunkte

| Kürzel | Klärungspunkt | Priorität |
|---|---|---|
| KP-FDM-001 | Finale Tabellenstruktur für Application, Wizard, Workflow, Upload, PortalUser, Organisation und Context festlegen | sehr hoch |
| KP-FDM-002 | Fachliche Pflichtfelder je Antragstyp definieren | sehr hoch |
| KP-FDM-003 | Rollenmodell für Vereine, Abteilungen und Ansprechpartner finalisieren | sehr hoch |
| KP-FDM-004 | Zuordnung Portalorganisation zu SportFM-Nutzer festlegen | sehr hoch |
| KP-FDM-005 | Uploadstrategie festlegen: gesonderte Uploadtabellen oder direkte Übernahme in Dokumentenbestand | hoch |
| KP-FDM-006 | Portal-Sichtbarkeit von Dokumenten und Rechnungen technisch definieren | hoch |
| KP-FDM-007 | Zahlstatus / ePayBL / SAP fachlich abgrenzen | mittel |
| KP-FDM-008 | Audit- und Protokollumfang für Portalzugriffe festlegen | mittel |
| KP-FDM-009 | Technische Abbildung von Reservierung gegenüber Booking / Event festlegen | hoch |
| KP-FDM-010 | Konfigurationsmodell für Wizard-Versionierung festlegen | hoch |

---

## 20 Ergebnis

Das fachliche Datenmodell zeigt:

- Die bestehende SportFM-Fachlogik bleibt führend.
- Das Portal ergänzt neue Vorgangs-, Identitäts- und Workflowdaten.
- Antrag, Wizard und Workflow sind neue Kerndomänen.
- Buchung, Occurrence, Winner, Gebühren, Rechnungen und Dokumente bleiben Bestandsdomänen.
- `PA_LHD_SPA` bleibt für Wiederholungen, Stornierungen, Gebühren und weitere Buchungslogik maßgeblich.
- `PA_LHD_SPA_OCC` bleibt für die performante Occurrence- und Winner-Logik maßgeblich.
- SAP bleibt über die bestehende REST-/OAuth-basierte Integration angebunden.

Die zentrale fachliche Trennung lautet:

```text
PortalUser ≠ SportFM-Nutzer
Antrag ≠ Buchung
Reservierung ≠ Rechnung
Upload ≠ Dokument
Gebühr ≠ Rechnung
Facility ≠ Booking
Stornierung = eigener Eventtyp
```

Dieses Dokument ist die fachliche Grundlage für:

- `Oracle_Datenmodell.md`,
- `Tabellen.md`,
- `Packages.md`,
- `Datenmigration.md`,
- `04_REST_API`,
- Aufwandsschätzung,
- Testkonzept.
