# Domäne Booking

| Feld | Wert |
|---|---|
| Kapitel | 03 – Domänen |
| Dokument | Booking |
| Status | Konsolidierter Arbeitsstand |
| Typ | Bestandsdomäne / REST-Freilegung |
| Priorität | Sehr hoch |
| Leitquellen | `Quellen/2026-07-05_Snapshot1.txt`, DDL-Dateien `LHD_SPA_EVENTS.sql`, `LHD_SPA_EVENT2UNIT.sql`, `LHD_SPA_EVENTCLASSES.sql`, `LHD_SPA_EVENTTYPES.sql`, `LHD_SPA_RECURRING_PATTERN.sql`, `LHD_SPA_OCC*.sql`, `LHD_SPA_BOOKING_NUMBERS.sql` |

---

## 1 Zweck

Die Domäne **Booking** beschreibt die bestehende SportFM-Buchungslogik.

Sie ist keine Neuentwicklung.

Booking bleibt führende Bestandsdomäne für Belegungen, Wiederholungen, Stornierungen, Occurrences, Winner-Ermittlung, Gebührenbezug und Kalenderlogik.

Ziel dieses Dokuments ist nicht die Neukonzeption der Buchungslogik, sondern die fachliche Dokumentation des Bestands und die Ableitung der notwendigen REST-, Portal- und Migrationsschnittstellen.

---

## 2 Projektbewertung

| Bereich | Bestand | Erweiterung | Neuentwicklung | Bewertung |
|---|:---:|:---:|:---:|---|
| Oracle | x | x |  | bestehendes Datenmodell bleibt führend |
| PL/SQL | x | x |  | bestehende Packages `PA_LHD_SPA` und `PA_LHD_SPA_OCC` bleiben führend |
| REST |  |  | x | neue fachliche Zugriffsschicht erforderlich |
| DTO |  |  | x | fachliche DTOs erforderlich, keine Tabellen-DTOs |
| Portal |  | x | x | Anzeige, Antragseinbindung, Kalender, freie Zeiten |
| WPF | x | x |  | spätere Migration auf REST |
| Tests |  | x | x | Regression gegen bestehende Buchungslogik erforderlich |

---

## 3 Grundsatz

Booking wird nicht neu entworfen.

Die bestehende Buchungslogik ist produktiv, umgesetzt und fachlich führend.

Verbindliche Grundsätze:

- keine zweite Buchungslogik im Portal,
- keine zweite Occurrence-Berechnung in .NET,
- keine zweite Winner-Berechnung in .NET,
- keine neue Gebührenlogik im Portal,
- keine direkte Tabellen-API,
- REST kapselt fachliche Operationen und nutzt den Bestand.

---

## 4 Bestand

### 4.1 Fachliche Funktionen im Bestand

Gesichert vorhanden sind:

- Buchungen als Events,
- wiederkehrende Belegungen,
- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltungen,
- Sperrungen,
- Reinigung,
- Schulbetrieb,
- GTA,
- Stornierungen,
- Gebührenlogik,
- Occurrence-Ermittlung,
- Winner-Ermittlung,
- Kalenderlogik,
- Suche nach freien Zeiten,
- Dokumenten- und Rechnungsbezug.

### 4.2 Bestehende PL/SQL-Packages

| Package | Zweck |
|---|---|
| `PA_LHD_SPA` | bestehendes zentrales Package für Wiederholungen, Stornierungen, Gebühren und weitere Buchungslogik |
| `PA_LHD_SPA_OCC` | bestehendes Package für Occurrences und performante Zugrifflogik der Termine |

### 4.3 Occurrence- und Winner-Logik

Die performante Terminlogik ist im Bestand gelöst.

`LHD_SPA_OCC` enthält konkrete Belegungsvorkommen.

`LHD_SPA_OCC_WINNER` enthält die resultierende gültige Belegung.

Die Aktualisierung erfolgt im Bestand stündlich und zusätzlich über einen Fast Path.

---

## 5 Fachmodell Bestand

### 5.1 Event als zentrales Buchungsobjekt

In SportFM sind belegungsrelevante Vorgänge als Events modelliert.

Das betrifft nicht nur klassische Nutzungsbuchungen, sondern auch Sperrungen, Reinigung, Schulbetrieb, GTA, Veranstaltungen und Stornierungen.

```text
Event
  ↓
Recurring Pattern, falls wiederkehrend
  ↓
Event2Unit
  ↓
Occurrence
  ↓
Winner
```

### 5.2 Buchbare Einheit

Die kleinste buchbare Einheit ist die **Teileinheit**.

Sportanlagen dienen der fachlichen Gruppierung von Teileinheiten.

Sportkomplexe fassen Sportanlagen organisatorisch zusammen.

```text
Sportkomplex
  └─ Sportanlage
       └─ Teileinheit = buchbare Einheit
```

Booking bezieht sich fachlich auf Teileinheiten.

Sportanlage und Sportkomplex werden für Suche, Darstellung, Filterung und Auswertung verwendet.

---

## 6 Buchungsarten / Eventtypen

Aktuell produktiv relevante Buchungs- bzw. Belegungsarten sind:

- Sperrung,
- Reinigung,
- Schulbetrieb,
- GTA,
- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltung,
- Stornierung.

Sperrungen, Reinigung, Schulbetrieb und GTA sind ebenfalls Events in `LHD_SPA_EVENTS`.

Die resultierende Belegung wird aus den Events über Occurrence und Winner berechnet.

---

## 7 Statusmodell Bestand

### 7.1 Events

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv |
| `-1` | gelöscht |

Buchungen werden nicht als komplexes Statusmodell historisiert.

Sie enden fachlich über ein Datum oder werden durch entsprechende Events, insbesondere Stornierungen, abgebildet.

### 7.2 Stornierung

Eine Stornierung ist fachlich eine eigene Buchung mit dem Typ Stornierung.

Die Stornierung ist keine einfache Statusänderung am ursprünglichen Event.

Auslöser im Bestand sind z. B. E-Mail, Anruf oder Sachbearbeitung.

---

## 8 Relevante Oracle-Tabellen

### 8.1 Buchungskern

| Tabelle | Zweck |
|---|---|
| `LHD_SPA_EVENTS` | zentrale Event-/Buchungstabelle |
| `LHD_SPA_EVENTS_HIST` | Historien-/Änderungsstruktur zu Events |
| `LHD_SPA_EVENTTYPES` | Eventtypen / Buchungsarten |
| `LHD_SPA_EVENTCLASSES` | Eventklassen |
| `LHD_SPA_EVENT2UNIT` | Zuordnung Event zu Teileinheiten |
| `LHD_SPA_BOOKING_NUMBERS` | Buchungsnummern je Jahr |
| `LHD_SPA_RECURRING_PATTERN` | Wiederholungsmuster |

### 8.2 Occurrence / Winner

| Tabelle | Zweck |
|---|---|
| `LHD_SPA_OCC` | konkrete Belegungsvorkommen |
| `LHD_SPA_OCC_WINNER` | resultierende gültige Belegung |
| `LHD_SPA_OCC_DAY_COVERAGE` | Tagesabdeckung / Aktualisierungsstand |
| `LHD_SPA_OCC_EVENT_DRT` | technische Dirty-/Retry-Struktur für Event-Occurrences |
| `LHD_SPA_OCC_WINNER_DRT` | technische Dirty-/Retry-Struktur für Winner |

### 8.3 Weitere Zuordnung

| Tabelle | Zweck |
|---|---|
| `LHD_SPA_FACILITYGROUPS` | Sportanlagengruppen / Gebühren- und Strukturbezug |

---

## 9 Wichtige Spalten aus dem Bestand

### 9.1 `LHD_SPA_EVENTS`

Zentrale Spalten:

- `ID_EVENT`,
- `ID_PARENT_EVENT`,
- `YEAR`,
- `ID_BOOKING`,
- `BOOKING_COUNTER`,
- `BOOKING_NUMBER`,
- `ID_SPA`,
- `SPA_NR`,
- `ID_USER`,
- `ID_EVENTTYPE`,
- `TITLE`,
- `DESCRIPTION`,
- `DATE_BEGIN`,
- `DATE_END`,
- `TIME_BEGIN`,
- `TIME_END`,
- `DURATION`,
- `ID_SPORTTYPE`,
- `ID_SPORTGROUP`,
- `ID_SPORTSUBGROUP`,
- `IS_RECURRING`,
- `IS_ALL_UNIT`,
- `STATE`,
- `DEPARTMENT`,
- `IS_COMPLEXEVENT`.

### 9.2 `LHD_SPA_RECURRING_PATTERN`

Zentrale Spalten:

- `ID_EVENT`,
- `FREQ`,
- `INTERVAL`,
- `MAX_OCCURRENCES`,
- `DAYS`,
- `HOLIDAYS`.

### 9.3 `LHD_SPA_EVENT2UNIT`

Zentrale Spalten:

- `ID_EVENT`,
- `ID_UNIT`.

### 9.4 `LHD_SPA_OCC`

Zentrale Spalten:

- `ID`,
- `EVENT_ID`,
- `SPA_ID`,
- `UNIT_ID`,
- `START_TS`,
- `END_TS`,
- `DAY_DATE`,
- `USER_ID`,
- `EVENTTYPE_ID`,
- `PRIORITY`,
- `PRIORITY_ETP`,
- `CANCEL_EVENT_ID`,
- `HOLIDAYS_MASK`,
- `CREATED_TS`.

### 9.5 `LHD_SPA_OCC_WINNER`

Zentrale Spalten:

- `WINNER_ID`,
- `SPA_ID`,
- `UNIT_ID`,
- `DAY_DATE`,
- `START_TS`,
- `END_TS`,
- `EVENT_ID`,
- `OCC_ID`,
- `USER_ID`,
- `EVENTTYPE_ID`,
- `PRIORITY`,
- `PRIORITY_ETP`,
- `CREATED_TS`.

---

## 10 Abgrenzung zu anderen Domänen

| Domäne | Beziehung zu Booking |
|---|---|
| `Application` | erfasst Anträge, erzeugt aber keine Buchung selbst |
| `Workflow` | steuert Bearbeitung und Genehmigung, führt aber keine Buchungslogik aus |
| `Availability` | liest freie Zeiten und Belegung aus Occurrence / Winner |
| `Facility` | liefert Sportkomplex, Sportanlage und Teileinheit |
| `Document` | stellt Dokumente zu Buchungen bereit |
| `Charge` | stellt Gebühreninformationen bereit, Berechnung bleibt Bestand |
| `Invoice` | stellt Rechnungen bereit, Erzeugung bleibt Bestand |
| `Notification` | informiert über relevante Ereignisse, verändert keine Buchung |
| `Context` | begrenzt Sichtbarkeit nach OE-/Abteilungs-/SportFM-Kontext |

---

## 11 REST-Strategie

Die REST-API ist eine fachliche Zugriffsschicht auf den Bestand.

Sie bildet keine Tabellen direkt ab.

Fachliche DTOs sind erforderlich, z. B.:

- `BookingDto`,
- `BookingDetailDto`,
- `BookingCalendarItemDto`,
- `BookingSeriesDto`,
- `BookingUnitDto`,
- `BookingChargeInfoDto`,
- `BookingDocumentInfoDto`.

---

## 12 REST-Endpunkte V1

| ID | Methode | Pfad | Zweck |
|---|---|---|---|
| BOOK-API-001 | `GET` | `/api/v1/bookings` | Buchungen suchen / listen |
| BOOK-API-002 | `GET` | `/api/v1/bookings/{id}` | Buchungsdetails lesen |
| BOOK-API-003 | `GET` | `/api/v1/bookings/{id}/units` | zugeordnete Teileinheiten lesen |
| BOOK-API-004 | `GET` | `/api/v1/bookings/{id}/occurrences` | Occurrences einer Buchung lesen |
| BOOK-API-005 | `GET` | `/api/v1/bookings/{id}/documents` | Dokumente zur Buchung lesen |
| BOOK-API-006 | `GET` | `/api/v1/bookings/{id}/charges` | Gebühreninformationen zur Buchung lesen |
| BOOK-API-007 | `GET` | `/api/v1/bookings/{id}/invoices` | Rechnungsbezüge zur Buchung lesen |
| BOOK-API-008 | `GET` | `/api/v1/calendar` | Kalenderdaten aus Buchungen / Winner lesen |
| BOOK-API-009 | `GET` | `/api/v1/event-types` | Eventtypen lesen |

Ändernde Booking-Endpunkte werden erst definiert, wenn die konkrete Portal- und WPF-Migration dies erfordert.

Für V1 steht die kontrollierte Bereitstellung des Bestands im Vordergrund.

---

## 13 Portal-Sicht

Das Portal darf im Bereich Booking:

- eigene Buchungen anzeigen,
- Buchungsdetails anzeigen,
- Kalenderinformationen anzeigen,
- zugeordnete Dokumente anzeigen,
- zugeordnete Rechnungen anzeigen,
- freie Zeiten über `Availability` anzeigen,
- aus freien Zeiten in einen Antrag wechseln.

Das Portal darf nicht:

- Winner berechnen,
- Occurrences berechnen,
- Gebühren berechnen,
- Buchungen direkt an der Datenbank verändern,
- SAP-Prozesse auslösen.

---

## 14 Sichtbarkeit und Kontext

Buchungen sind nur im jeweils zulässigen SportFM-Kontext sichtbar.

Der Kontext kann sich aus einer OE oder optional aus einer Abteilung ergeben.

Abteilungen ohne eigenen SportFM-Kontext nutzen den Kontext der übergeordneten OE.

Die genaue Kontextprüfung liegt in der Domäne `Context`.

---

## 15 Blazor-Frontend

### 15.1 Seiten / Bereiche

| ID | Seite / Bereich | Zweck |
|---|---|---|
| BOOK-UI-001 | Meine Buchungen | kontextbezogene Buchungsliste |
| BOOK-UI-002 | Buchungsdetails | Details zu einer Buchung |
| BOOK-UI-003 | Kalender | Anzeige von Belegungen / Winner-Daten |
| BOOK-UI-004 | Buchungsdokumente | Dokumente zur Buchung anzeigen |
| BOOK-UI-005 | Buchungsrechnungen | Rechnungsbezüge anzeigen |

### 15.2 Komponenten

| Komponente | Zweck |
|---|---|
| `BookingList` | Buchungsliste |
| `BookingDetail` | Buchungsdetail |
| `BookingCalendar` | Kalenderanzeige |
| `BookingStatusBadge` | Statusanzeige aktiv / gelöscht |
| `BookingUnitList` | Teileinheiten anzeigen |
| `BookingDocumentList` | Dokumente zur Buchung |
| `BookingInvoiceList` | Rechnungen zur Buchung |

---

## 16 Berechtigungen

| Berechtigung | Zweck |
|---|---|
| `Booking.Read` | Buchungen im zulässigen Kontext lesen |
| `Booking.Calendar.Read` | Kalenderdaten lesen |
| `Booking.Occurrences.Read` | Occurrences lesen |
| `Booking.Documents.Read` | Dokumente zur Buchung lesen |
| `Booking.Charges.Read` | Gebühreninformationen lesen |
| `Booking.Invoices.Read` | Rechnungsbezüge lesen |

Ändernde Berechtigungen werden erst mit konkreten Änderungsfunktionen definiert.

---

## 17 Validierungen

| ID | Validierung | Ebene |
|---|---|---|
| BOOK-VAL-001 | Buchung existiert | REST / BookingService |
| BOOK-VAL-002 | Buchung ist nicht gelöscht, sofern keine gelöschten Daten angefordert werden | BookingService |
| BOOK-VAL-003 | Benutzer besitzt zulässigen Kontext | Context |
| BOOK-VAL-004 | Benutzer darf Dokumente zur Buchung sehen | Context / Document |
| BOOK-VAL-005 | Benutzer darf Rechnungen zur Buchung sehen | Context / Invoice |
| BOOK-VAL-006 | Kalenderzeitraum ist begrenzt | REST / Performance |
| BOOK-VAL-007 | Teileinheit gehört zur Buchung | BookingService |

---

## 18 Testfälle

| Testfall | Beschreibung |
|---|---|
| TF-BOOK-001 | Buchungsliste im Kontext laden |
| TF-BOOK-002 | fremde Buchung nicht anzeigen |
| TF-BOOK-003 | Buchungsdetails lesen |
| TF-BOOK-004 | zugeordnete Teileinheiten lesen |
| TF-BOOK-005 | Occurrences einer Buchung lesen |
| TF-BOOK-006 | Kalenderdaten laden |
| TF-BOOK-007 | Dokumente zur Buchung lesen |
| TF-BOOK-008 | Rechnungsbezüge zur Buchung lesen |
| TF-BOOK-009 | gelöschte Buchung standardmäßig ausblenden |
| TF-BOOK-010 | REST nutzt Bestand und erzeugt keine eigene Occurrence-Logik |
| TF-BOOK-011 | REST nutzt Bestand und erzeugt keine eigene Winner-Logik |
| TF-BOOK-012 | Performance Kalenderabfrage prüfen |

---

## 19 Arbeitspakete

| AP | Titel | Inhalt |
|---|---|---|
| AP-BOOK-001 | Bestandsmapping | Tabellen, Packages, bestehende Logik dokumentieren |
| AP-BOOK-002 | DTOs | Booking-, Kalender- und Detail-DTOs definieren |
| AP-BOOK-003 | REST Lesen | Leseendpunkte für Buchungen |
| AP-BOOK-004 | Kalenderzugriff | REST-Zugriff auf Kalender-/Winner-Daten |
| AP-BOOK-005 | Occurrence-Zugriff | vorhandene Occurrence-Daten bereitstellen |
| AP-BOOK-006 | Dokumentenbezug | Document-Domäne anbinden |
| AP-BOOK-007 | Gebührenbezug | Charge-Domäne anbinden |
| AP-BOOK-008 | Rechnungsbezug | Invoice-Domäne anbinden |
| AP-BOOK-009 | Kontextprüfung | Context-Domäne anbinden |
| AP-BOOK-010 | Portal | Buchungsliste, Detail, Kalenderbereiche |
| AP-BOOK-011 | Tests | Regression, REST, Kontext, Performance |
| AP-BOOK-012 | Dokumentation | API- und Bestandsdokumentation |

---

## 20 Aufwandstreiber

| Treiber | Einfluss |
|---|---|
| Mapping Bestandstabellen zu fachlichen DTOs | mittel |
| Kontextprüfung für OE und optionale Abteilungskontexte | hoch |
| Kalenderperformance | hoch |
| Abgrenzung Portal-Sicht / interne WPF-Sicht | mittel |
| Dokumenten- und Rechnungsbezug | mittel |
| spätere schreibende WPF-Migration | hoch, aber nicht V1 dieses Dokuments |
| Regression gegen bestehende Logik | hoch |

---

## 21 Risiken

| Risiko | Bewertung | Maßnahme |
|---|---|---|
| REST bildet Tabellen statt Fachfunktionen ab | hoch | fachliche DTOs und Services verwenden |
| Portal berechnet eigene Belegungslogik | sehr hoch | Occurrence / Winner ausschließlich aus Bestand nutzen |
| Kontextprüfung unvollständig | hoch | Context-Domäne verbindlich anbinden |
| gelöschte Buchungen werden falsch angezeigt | mittel | Status `1` / `-1` konsequent berücksichtigen |
| Kalenderabfragen werden zu groß | hoch | Zeitraumgrenzen, Filter und Caching prüfen |
| bestehende Package-Logik wird umgangen | hoch | `PA_LHD_SPA` / `PA_LHD_SPA_OCC` als führend behandeln |

---

## 22 Offene Punkte

| ID | Status | Offener Punkt | Relevanz |
|---|---|---|---|
| O-BOOK-001 | Prüfen | Welche schreibenden Booking-Funktionen werden in V1 tatsächlich über REST benötigt? | hoch |
| O-BOOK-002 | Prüfen | Welche internen WPF-Funktionen werden zuerst auf REST migriert? | mittel |
| O-BOOK-003 | Prüfen | Welche Kalenderdetailtiefe ist im Portal zulässig? | hoch |
| O-BOOK-004 | Entscheiden | Zeitraumgrenzen für Kalender- und Occurrence-Endpunkte | hoch |

---

## 23 Traceability-Matrix

| Quelle | Funktion | REST | Service | UI | Test | AP |
|---|---|---|---|---|---|---|
| Snapshot / Bestand | Buchungen als Events | BOOK-API-001/002 | BookingService | BookingList / BookingDetail | TF-BOOK-001/003 | AP-BOOK-003 |
| DDL `LHD_SPA_EVENTS` | Eventdetails | BOOK-API-002 | BookingService | BookingDetail | TF-BOOK-003 | AP-BOOK-002/003 |
| DDL `LHD_SPA_EVENT2UNIT` | Teileinheiten | BOOK-API-003 | BookingService | BookingUnitList | TF-BOOK-004 | AP-BOOK-003 |
| DDL `LHD_SPA_RECURRING_PATTERN` | Wiederholungen | BOOK-API-002/004 | BookingService | BookingDetail | TF-BOOK-005 | AP-BOOK-005 |
| DDL `LHD_SPA_OCC` | Occurrences | BOOK-API-004 | BookingService / Availability | BookingCalendar | TF-BOOK-005 | AP-BOOK-005 |
| DDL `LHD_SPA_OCC_WINNER` | gültige Belegung | BOOK-API-008 | BookingService / Availability | BookingCalendar | TF-BOOK-006 | AP-BOOK-004 |
| Bestand `PA_LHD_SPA_OCC` | performante Zugrifflogik | BOOK-API-004/008 | BookingService | BookingCalendar | TF-BOOK-010/011 | AP-BOOK-004/005 |
| Context.md | Sichtbarkeit | alle BOOK-APIs | ContextService | alle Booking-Bereiche | TF-BOOK-002 | AP-BOOK-009 |

---

## 24 Änderungsauswirkungen

Änderungen an `Booking.md` wirken sich aus auf:

- `03_Domaenen/Application.md`,
- `03_Domaenen/Workflow.md`,
- `03_Domaenen/Availability.md`,
- `03_Domaenen/Facility.md`,
- `03_Domaenen/Document.md`,
- `03_Domaenen/Charge.md`,
- `03_Domaenen/Invoice.md`,
- `03_Domaenen/Context.md`,
- `04_REST_API/Endpunkte.md`,
- `04_REST_API/DTOs.md`,
- `05_Datenmodell/Tabellen.md`,
- `05_Datenmodell/Packages.md`,
- `06_Arbeitspakete/Arbeitspaketliste.md`,
- `07_Kalkulation/Aufwandsschaetzung.md`,
- `09_Testkonzept/Testfaelle.md`,
- `12_Offene_Punkte/Offene_Punkte.md`.

---

## 25 Ergebnis

Die Domäne Booking ist als Bestandsdomäne beschrieben.

Sie wird nicht neu konzipiert.

Die bestehende Oracle- und PL/SQL-Logik bleibt führend.

Die Umsetzung im Projekt besteht aus:

- fachlicher Dokumentation des Bestands,
- DTO- und REST-Freilegung,
- Kontext- und Berechtigungsprüfung,
- Portal-Anzeige,
- späterer WPF-Migration,
- Regression gegen die bestehende Logik.

Die konkrete Kalkulation hängt vor allem von der gewünschten REST-Tiefe, der Kalenderdetailtiefe, der Kontextprüfung und dem Umfang der ersten WPF-Migrationsschritte ab.
