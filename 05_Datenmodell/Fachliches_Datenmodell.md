# Fachliches Datenmodell

| Feld | Wert |
|---|---|
| Kapitel | 05 – Datenmodell |
| Dokument | Fachliches Datenmodell |
| Status | Fertiggestellt |
| Typ | Fachliches Vertiefungsdokument |
| Priorität | Sehr hoch |
| Vorgängerdokument | `05_Datenmodell/Datenmodell_Uebersicht.md` |

---

## 1 Zweck

Dieses Dokument vertieft das fachliche Datenmodell der SportFM-Plattform.

Es beschreibt die zentralen Datenobjekte, ihre fachliche Bedeutung, Lebenszyklen, Beziehungen und Verantwortlichkeiten.

Im Unterschied zu `Datenmodell_Uebersicht.md` steht hier nicht die Gesamtlandkarte im Vordergrund, sondern die fachliche Ausarbeitung der einzelnen Datenbereiche.

Technische Details zu Oracle-Tabellen, Spalten, Indizes, Triggern und Packages folgen in:

- `Oracle_Datenmodell.md`,
- `Tabellen.md`,
- `Packages.md`,
- `Datenmigration.md`.

---

## 2 Grundsätze

### 2.1 Fachliches Modell vor technischem Schema

Das fachliche Datenmodell beschreibt, **was** die Daten fachlich bedeuten.

Es beschreibt nicht vollständig, **wie** sie technisch gespeichert werden.

Beispiel:

```text
Antrag
  ↓
Arbeitsablauf
  ↓
Buchung
  ↓
Belegung
  ↓
Gebühr
  ↓
Rechnung
  ↓
Dokument
```

Diese Sicht ist für Pflichtenheft, REST-API, Arbeitspakete und Tests führend.

### 2.2 Bestand bleibt führend

Für bestehende SportFM-Kernbereiche bleibt Oracle mit PL/SQL führend.

Dies betrifft insbesondere:

- Buchungen,
- Wiederholungen,
- Occurrences,
- Winner,
- Gebühren,
- Rechnungen,
- Dokumente,
- Sportstättenbezüge.

### 2.3 Portal ergänzt, ersetzt aber nicht

Das Portal erzeugt neue fachliche Datenobjekte.

Es ersetzt keine bestehenden SportFM-Kernobjekte.

Neue Portalobjekte sind insbesondere:

- Antrag,
- Antragssassistent,
- Arbeitsablauf,
- Upload,
- Portalbenutzer,
- Organisation,
- Kontext,
- Meldung.

### 2.4 Keine doppelte Fachlogik

Fachliche Regeln werden nicht im Portal nachgebaut.

Das betrifft insbesondere:

- Gebührenberechnung,
- Belegungsberechnung,
- Konfliktlogik,
- Winner-Ermittlung,
- Stornierungslogik,
- Dokumentnummernlogik,
- SAP-Anbindung.

---

## 3 Fachliche Hauptbereiche

Das fachliche Datenmodell besteht aus fünf Hauptbereichen.

| Nr. | Hauptbereich | Beschreibung |
|---:|---|---|
| 1 | Identität und Kontext | Portalbenutzer, Organisationen, Rollen, aktiver Sichtbarkeitsraum |
| 2 | Antrag und Bearbeitung | Antrag, Antragsassistent, Arbeitsablauf, Upload |
| 3 | Sportstätte und Belegung | Sportanlage, Teileinheit, Verfügbarkeit, Buchung, Occurrence, Winner |
| 4 | Gebühren, Rechnungen und Dokumente | Gebühren, Rechnungen, SAP-Bezug, Dokumente, Vorlagen |
| 5 | Querschnitt | Meldungen, Dashboard, Administration, Protokollierung |

---

## 4 Identität und Kontext

### 4.1 Authentication

Die Domäne Authentication beschreibt die technische Identität eines Portalzugangs.

Fachliche Aufgaben:

- Anmeldung,
- Registrierung,
- Passwortprozesse,
- Token,
- Sperrstatus,
- Zwei-Faktor-Authentifizierung,
- OAuth für technische Zugriffe.

Authentication ist nicht verantwortlich für das fachliche Profil des Nutzers.

### 4.2 PortalUser

PortalUser beschreibt das fachliche Benutzerprofil im Portal.

Fachliche Inhalte:

- Name,
- Kontaktdaten,
- E-Mail,
- Einstellungen,
- Zustimmungen,
- Favoriten,
- Zuordnungen zu Organisationen.

PortalUser ist von der technischen Identität getrennt.

```text
Identity
  ↓
PortalUser
```

### 4.3 Organisation

Organisation beschreibt fachliche Einheiten im Portal.

Beispiele:

- Verein,
- Abteilung,
- Schule,
- Kita,
- Firma,
- Privatperson als Einzelkontext.

Wichtig:

Ein SportFM-Nutzer kann mehrere Portalbenutzer besitzen.

Ein großer Verein kann mehrere Abteilungen und mehrere berechtigte Personen besitzen.

```text
SportFM-Nutzer
  ↓
Organisation
  ↓
Abteilung
  ↓
Portalbenutzer
```

### 4.4 Context

Context beschreibt den aktiven fachlichen Arbeits- und Sichtbarkeitsraum.

Beispiel:

Ein Portalbenutzer ist mehreren Organisationen zugeordnet und wählt aktiv aus, für welche Organisation er arbeitet.

```text
PortalUser
  ↓
Context
  ↓
Anträge / Dokumente / Rechnungen / Buchungen
```

Context ist zentral für Berechtigungen und Mandantentrennung.

---

## 5 Antrag und Bearbeitung

### 5.1 Application

Application beschreibt den fachlichen Antrag im Portal.

Ein Antrag ist kein Event und keine Buchung.

Ein Antrag ist ein digitaler Vorgang, der später zu einer Reservierung oder Buchung führen kann.

Fachliche Inhalte:

- Antragstyp,
- Antragsteller,
- Organisation,
- gewählter Kontext,
- Antragsdaten,
- gewünschte Sportstätte,
- gewünschte Zeiträume,
- Uploads,
- Status,
- Verlauf,
- Zuordnung zur späteren Buchung.

Lebenszyklus:

```text
Entwurf
  ↓
Eingereicht
  ↓
In Prüfung
  ↓
Rückfrage / Nachforderung
  ↓
Genehmigt oder Abgelehnt
  ↓
Buchung erzeugt / abgeschlossen
```

### 5.2 Wizard

Wizard beschreibt den konfigurierbaren Antragsassistenten.

Ziel:

Neue Antragstypen sollen möglichst durch Konfiguration entstehen.

Fachliche Inhalte:

- Wizarddefinition,
- Version,
- Schritte,
- Felder,
- Pflichtfelder,
- Sichtbarkeitsregeln,
- Validierungen,
- Uploadvorgaben,
- Zusammenfassung,
- Einreichlogik.

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

### 5.3 Workflow

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
BookingEvent
```

### 5.4 Upload

Upload beschreibt die technische und fachliche Annahme von Dateien.

Upload ist nicht identisch mit Document.

Upload ist die Annahme und Prüfung.

Document ist die dauerhafte Dokumentverwaltung.

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

## 6 Sportstätte und Belegung

### 6.1 Facility

Facility beschreibt die fachlichen Sportstättenobjekte.

Fachliche Inhalte:

- Sportkomplex,
- Sportanlage,
- Teileinheit,
- Sportanlagengruppe,
- Standort,
- Eigenschaften,
- Zuordnung zu Gebühren und Verfügbarkeit.

Facility beschreibt **wo** Nutzung stattfindet.

Booking beschreibt **dass und wann** Nutzung stattfindet.

### 6.2 Availability

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

Es erfolgt keine Neuberechnung im Portal.

```text
LHD_SPA_OCC
  ↓
LHD_SPA_OCC_WINNER
  ↓
AvailabilityResult
```

### 6.3 Booking

Booking beschreibt die Buchungs- und Belegungslogik.

Fachliche Inhalte:

- Event,
- Buchung,
- Eventtyp,
- Eventklasse,
- Buchungsnummer,
- Zeitraum,
- Uhrzeit,
- Nutzer,
- Sportstätte,
- Teileinheiten,
- Wiederholungsmuster,
- Status,
- Gebührenbezug,
- Dokumentbezug.

Alle belegungsrelevanten Vorgänge werden als Events geführt.

Dazu zählen:

- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltung,
- Sperrung,
- Reinigung,
- Schulbetrieb,
- GTA,
- Stornierung.

### 6.4 RecurringPattern

RecurringPattern beschreibt Wiederholungsmuster.

Fachliche Inhalte:

- Frequenz,
- Intervall,
- Wochentage,
- maximale Vorkommen,
- Feiertagsbehandlung.

Die Berechnung erfolgt im Bestand.

### 6.5 Occurrence

Occurrence beschreibt konkrete Einzelvorkommen aus Events und Wiederholungsmustern.

Fachliche Inhalte:

- Eventbezug,
- Sportanlage,
- Teileinheit,
- Startzeit,
- Endzeit,
- Datum,
- Nutzer,
- Eventtyp,
- Priorität.

Occurrences sind Grundlage für performante Kalender- und Belegungszugriffe.

### 6.6 OccurrenceWinner

OccurrenceWinner beschreibt die resultierende gültige Belegung.

Fachliche Inhalte:

- gültiges Vorkommen,
- verdrängte / überlagerte Belegungen,
- Priorität,
- Sperrungen,
- Stornierungen,
- Schulbetrieb,
- Veranstaltungen.

Winner ist führend für:

- Kalender,
- freie Zeiten,
- Konfliktprüfung,
- Belegungsanzeige.

---

## 7 Gebühren, Rechnungen und Dokumente

### 7.1 Charge

Charge beschreibt Gebühren und Gebühreninformationen.

Fachliche Inhalte:

- Gebühr,
- Gebührentyp,
- Gebührengruppe,
- Betrag,
- Mehrwertsteuer,
- Gültigkeitszeitraum,
- Sportanlagengruppe,
- Tageszeitbezug,
- EventCharge,
- InvoiceChargeInfo.

Die Gebührenberechnung erfolgt über den Bestand, insbesondere `PA_LHD_SPA.p_get_charges`.

Das Portal darf Gebühren anzeigen, aber nicht selbst berechnen.

### 7.2 Invoice

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
- Rechnungsdokumente.

Invoice ist nicht verantwortlich für Gebührenberechnung.

Invoice ist verantwortlich für Rechnungssicht und Zahlstatus.

### 7.3 Document

Document beschreibt dauerhaft verwaltete Dokumente.

Fachliche Inhalte:

- Dokument,
- Dokumenttyp,
- Dokumentstatus,
- Dokumentnummer,
- Dateiinhalt,
- Vorlage,
- Textbaustein,
- Bezug zu Nutzer,
- Bezug zu Event,
- Bezug zu Rechnung.

Dokumentstatus:

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |
| `2` | bearbeitet |
| `3` | gedruckt |

Dokumente sind grundsätzlich portalgeeignet, wenn der Portalnutzer berechtigt ist.

---

## 8 Querschnittsdomänen

### 8.1 Notification

Notification beschreibt Meldungen und Benachrichtigungen.

Fachliche Inhalte:

- Portalnachricht,
- Empfänger,
- Versandkanal,
- MailQueue,
- Versandstatus,
- Lesestatus,
- Vorlage,
- Bezug zu Antrag, Dokument, Rechnung oder Workflow.

### 8.2 Dashboard

Dashboard ist eine Aggregationsdomäne.

Sie besitzt in V1 keine eigene fachliche Persistenz.

Dashboard aggregiert:

- offene Anträge,
- Aufgaben,
- Rückfragen,
- neue Dokumente,
- offene Rechnungen,
- Nachrichten.

### 8.3 Administration

Administration beschreibt administrative Daten.

Fachliche Inhalte:

- Systemparameter,
- Konfiguration,
- administrative Sichten,
- Protokollierung,
- Audit,
- Pflege von Referenzdaten, soweit nicht im Bestand führend.

---

## 9 Zentrale Beziehungen

| Von | Nach | Beziehung |
|---|---|---|
| Identity | PortalUser | technische Identität besitzt fachliches Profil |
| PortalUser | Organisation | Benutzer gehört zu Organisationen |
| Organisation | Context | Organisation bildet Arbeitskontext |
| Context | Application | Antrag wird in einem Kontext gestellt |
| Application | Wizard | Antrag basiert auf Wizarddefinition |
| Application | Workflow | Antrag startet Arbeitsablauf |
| Application | Upload | Antrag kann Dateien enthalten |
| Application | Facility | Antrag bezieht sich auf Sportstätte / Teileinheit |
| Workflow | BookingEvent | Entscheidung kann Buchung erzeugen |
| BookingEvent | RecurringPattern | Event kann Wiederholungsmuster besitzen |
| BookingEvent | Occurrence | Event erzeugt konkrete Vorkommen |
| Occurrence | OccurrenceWinner | Winner bestimmt gültige Belegung |
| BookingEvent | Charge | Buchung kann Gebühren erzeugen |
| Charge | Invoice | Gebühreninformationen fließen in Rechnung |
| Invoice | Document | Rechnung kann Dokument besitzen |
| BookingEvent | Document | Buchung kann Genehmigung / Widerruf erzeugen |
| Upload | Document | geprüfter Upload kann dauerhaftes Dokument werden |
| Notification | Application / Workflow / Document / Invoice | Meldung bezieht sich auf Fachobjekt |

---

## 10 Lebenszyklus Gesamtprozess

Der fachliche Gesamtprozess lautet:

```text
Portalbenutzer
  ↓
wählt Kontext
  ↓
startet Antragsassistent
  ↓
speichert Antrag als Entwurf
  ↓
reicht Antrag ein
  ↓
Arbeitsablauf startet
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

## 11 Datenhoheit

| Datenobjekt | Führend | Bemerkung |
|---|---|---|
| Identity | neue Auth-Schicht | technische Identität |
| PortalUser | PortalUser-Domäne | fachliches Portalprofil |
| Organisation | Organisation-Domäne / SportFM-Bezug | Zuordnung zu SportFM-Nutzer erforderlich |
| Context | Context-Domäne | Sichtbarkeit und Berechtigung |
| Application | Application-Domäne | neuer Portalvorgang |
| Wizard | Wizard-Domäne | Konfiguration |
| Workflow | Workflow-Domäne | Bearbeitungsablauf |
| Upload | Upload-Domäne | Dateiannahme |
| Facility | Oracle / SportFM | Bestand führend |
| BookingEvent | Oracle / SportFM | Bestand führend |
| Occurrence | Oracle / `PA_LHD_SPA_OCC` | Bestand führend |
| OccurrenceWinner | Oracle / `PA_LHD_SPA_OCC` | Bestand führend |
| Charge | Oracle / `PA_LHD_SPA` | Bestand führend |
| Invoice | Oracle / SAP-Integration | Bestand führend |
| Document | Oracle / SportFM | Bestand führend |
| Notification | neue Plattformdomäne | querschnittlich |
| Dashboard | Aggregation | keine eigene führende Datenhaltung V1 |

---

## 12 Statusmodelle

### 12.1 Eventstatus

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv |
| `-1` | gelöscht |

Buchungen werden fachlich nicht über komplexe Statusketten historisiert.

Sie enden über Datum oder werden über Stornierungs-Events abgebildet.

### 12.2 Dokumentstatus

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |
| `2` | bearbeitet |
| `3` | gedruckt |

### 12.3 Rechnungsstatus

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |

### 12.4 Portalstatus

Portalstatus sind neue fachliche Status und dürfen nicht mit Bestandsstatus vermischt werden.

Beispiele:

- Antragsentwurf,
- eingereicht,
- in Prüfung,
- Rückfrage,
- genehmigt,
- abgelehnt,
- abgeschlossen.

Finale Werte werden im Detaildatenmodell festgelegt.

---

## 13 Fachliche Abgrenzungen

### 13.1 Antrag ist keine Buchung

Ein Antrag beschreibt den Wunsch eines Portalnutzers.

Eine Buchung beschreibt die verbindliche Nutzung in SportFM.

### 13.2 Reservierung ist keine Rechnung

Eine Reservierung ist fachlich unverbindlich oder vorbereitend.

Eine Rechnung entsteht erst nach Abrechnung.

### 13.3 Stornierung ist ein Event

Eine Stornierung wird fachlich als eigene Buchung / eigener Eventtyp abgebildet.

Sie ist nicht nur eine Statusänderung am ursprünglichen Event.

### 13.4 Upload ist kein Dokument

Ein Upload ist eine angenommene Datei.

Ein Dokument ist ein dauerhaft verwaltetes Fachobjekt.

### 13.5 Gebühr ist keine Rechnung

Gebühren beschreiben Berechnungs- und Positionsinformationen.

Rechnungen beschreiben den abrechenbaren Beleg mit SAP-/Zahlungsbezug.

---

## 14 Auswirkungen auf REST

Das fachliche Datenmodell führt zu folgenden REST-Grundsätzen:

- REST folgt Domänen, nicht Tabellen.
- REST liefert fachliche Datenübertragungsobjekte.
- REST kapselt Oracle und PL/SQL.
- REST prüft Berechtigung über PortalUser, Organisation und Context.
- REST ruft Bestandslogik nur über definierte Gateways auf.
- REST stellt Dokumente und Rechnungen nur berechtigt bereit.
- REST berechnet keine Gebühren im Portal.

Beispielstruktur:

```text
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

## 15 Auswirkungen auf Arbeitspakete

Das fachliche Datenmodell erzeugt Arbeitspakete in folgenden Bereichen:

| Bereich | Arbeitspaket-Typ |
|---|---|
| Portalidentität | Neuentwicklung |
| Organisation / Kontext | Neuentwicklung |
| Antrag | Neuentwicklung |
| Antragsassistent | Neuentwicklung / Konfiguration |
| Arbeitsablauf | Neuentwicklung |
| Upload | Neuentwicklung |
| Sportstätten | Bestandsfreilegung |
| Buchungen | Bestandsfreilegung / später Migration |
| Verfügbarkeit | Bestandsfreilegung / Performance |
| Gebühren | Bestandsfreilegung |
| Rechnungen | Bestandsfreilegung / Integration |
| Dokumente | Bestandsfreilegung / Download / Berechtigung |
| Meldungen | Neuentwicklung |
| Dashboard | Aggregation |

---

## 16 Auswirkungen auf Tests

Aus fachlicher Sicht sind mindestens folgende Testschwerpunkte erforderlich:

| Bereich | Testschwerpunkt |
|---|---|
| PortalUser / Context | Benutzer sieht nur eigene Daten |
| Application | Entwurf, Einreichung, Status, Historie |
| Wizard | Pflichtfelder, Sichtbarkeit, Versionierung |
| Workflow | Statusübergänge, Rückfragen, Entscheidungen |
| Upload | Dateitypen, Größen, Zuordnung, Fehlerfälle |
| Facility | Filter, Suchlogik, Sichtbarkeit |
| Availability | freie Zeiten, Konflikte, Performance |
| Booking | Bestandstreue, Eventtypen, Stornierung |
| Charge | Gebührenanzeige ohne Neuberechnung im Portal |
| Invoice | nur eigene Rechnungen, SAP-/Zahlstatus |
| Document | nur eigene Dokumente, Download, Status |
| Notification | Versand, Lesestatus, Bezug zu Fachobjekten |

---

## 17 Offene Klärungspunkte

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

---

## 18 Ergebnis

Das fachliche Datenmodell zeigt:

- Die bestehende SportFM-Fachlogik bleibt führend.
- Das Portal ergänzt neue Vorgangs-, Identitäts- und Workflowdaten.
- Antrag, Antragsassistent und Arbeitsablauf sind neue Kerndomänen.
- Buchung, Occurrence, Winner, Gebühren, Rechnungen und Dokumente bleiben Bestandsdomänen.
- Die zentrale fachliche Trennung lautet:

```text
Portalvorgang ≠ SportFM-Buchung
Upload ≠ Dokument
Gebühr ≠ Rechnung
PortalUser ≠ SportFM-Nutzer
Facility ≠ Booking
```

Dieses Dokument ist fachlich abgeschlossen und bildet die Grundlage für:

- `Oracle_Datenmodell.md`,
- `Tabellen.md`,
- `Packages.md`,
- `Datenmigration.md`,
- `04_REST_API`.
