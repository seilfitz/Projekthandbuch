# Oracle-Datenmodell

> Arbeitsstand: Abschnitt 1 bis 8  
> Quelle: GitHub-Master, `2026-07-05_Snapshot1.txt`, Lastenheft SportFM, Oracle-DDLs aus `GIT_IMSP.zip`, bisher erarbeitete Domänen- und Architekturprinzipien sowie fachliche Klärungen im Projektchat.

## 1. Zweck dieses Kapitels

Dieses Kapitel beschreibt das aktuell erkennbare Oracle-Datenmodell von SportFM auf fachlicher Ebene.

Ziel ist nicht die vollständige technische Dokumentation jeder Spalte, jedes Indexes oder jeder Constraint. Ziel ist die Ableitung der fachlichen Struktur, der zentralen Datenobjekte und der Konsequenzen für REST-API, Onlineportal und spätere Migration der WPF-Anwendung.

Das Kapitel folgt den vereinbarten Projektprinzipien:

- keine Neumodellierung ohne fachliche Grundlage,
- keine erfundenen Tabellen oder Beziehungen,
- vorhandene SportFM-Fachlogik bleibt führend,
- REST-API kapselt Fachoperationen und bildet keine rohe Tabellen-API,
- Portal und WPF nutzen künftig dieselbe Geschäftslogik.

## 2. Einordnung in die Gesamtarchitektur

SportFM nutzt Oracle 19c als führende Daten- und Logikschicht. Die bestehende WPF-Anwendung greift derzeit direkt auf Oracle und die vorhandene PL/SQL-Logik zu. Die Zielarchitektur sieht vor, diesen direkten Zugriff schrittweise durch eine REST-API abzulösen.

```text
Heute:

WPF SportFM
  ↓
Oracle / PL-SQL
  ↓
SportFM-Tabellen
```

```text
Zielbild:

Onlineportal / WPF / weitere Clients
  ↓
REST-API SportFM
  ↓
Business Services
  ↓
Oracle / PL-SQL
  ↓
SportFM-Tabellen
```

Die Oracle-Struktur ist damit kein reines Persistenzmodell, sondern Bestandteil der bestehenden Fachlogik. Besonders relevant sind die vorhandenen PL/SQL-Packages:

| Package | Fachliche Rolle |
|---|---|
| `PA_LHD_SPA` | bestehendes zentrales Package für Wiederholungen, Stornierungen, Gebühren und weitere Buchungslogik |
| `PA_LHD_SPA_OCC` | Package für Ermittlung und Pflege der Occurrences sowie performante Zugriffslogik auf Termine |

Zusätzlich wurde fachlich bestätigt, dass die Occurrence-/Winner-Logik produktiv gelöst ist. Sie läuft über einen stündlichen Job und zusätzlich über einen Fast Path.

## 3. Fachliche Leitentscheidung des Datenmodells

Die wichtigste fachliche Grundentscheidung lautet:

> Belegungsrelevante Sachverhalte werden in SportFM als Events modelliert.

Das betrifft nicht nur klassische Nutzungsbuchungen, sondern auch:

- Sperrungen,
- Reinigung,
- Schulbetrieb,
- GTA,
- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltungen,
- Stornierungen.

Eine Stornierung ist damit keine reine Statusänderung an einer bestehenden Buchung, sondern selbst ein Event vom Typ Stornierung.

Diese Modellierung ist für die Plattformarchitektur wesentlich, weil dadurch alle belegungsrelevanten Sachverhalte in einer gemeinsamen Logik verarbeitet werden können. Die resultierende tatsächliche Belegung wird nicht direkt aus den Events gelesen, sondern über die Occurrence- und Winner-Logik ermittelt.

## 4. Fachliche Domänen aus dem Oracle-Modell

Aus den vorhandenen DDLs ergeben sich folgende fachliche Domänen:

| Domäne | Zentrale Tabellen |
|---|---|
| Buchung / Events | `LHD_SPA_EVENTS`, `LHD_SPA_EVENTTYPES`, `LHD_SPA_EVENTCLASSES`, `LHD_SPA_EVENT2UNIT` |
| Wiederholungen | `LHD_SPA_RECURRING_PATTERN` |
| Belegung / Occurrences | `LHD_SPA_OCC`, `LHD_SPA_OCC_DAY_COVERAGE`, `LHD_SPA_OCC_EVENT_DRT` |
| Resultierende Belegung / Winner | `LHD_SPA_OCC_WINNER`, `LHD_SPA_OCC_WINNER_DRT` |
| Dokumente | `LHD_SPA_DOCUMENTS`, `LHD_SPA_DOCUMENTS_EVENTS`, `LHD_SPA_DOCUMENT_TYPES`, `LHD_SPA_DOCUMENT_TEMPLATES`, `LHD_SPA_DOCUMENT_TEXTMODULES`, `LHD_SPA_DOCUMENT_NUMBERS` |
| Gebühren | `LHD_SPA_CHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS`, `LHD_SPA_CHARGE2FACILITYGROUP`, `LHD_SPA_EVENTCHARGES` |
| Rechnungen | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICE_CHARGEINFOS` |
| Sportstruktur | `LHD_SPA_SPORTTYPES`, `LHD_SPA_SPORTCATEGORIES`, `LHD_SPA_SPORTGROUPS`, `LHD_SPA_SPORTSUBGROUPS`, `LHD_SPA_SPORTSCOMPLEXES`, `LHD_SPA_FACILITY2COMPLEX`, `LHD_SPA_FACILITYGROUPS` |
| Nummernkreise | `LHD_SPA_BOOKING_NUMBERS`, `LHD_SPA_DOCUMENT_NUMBERS`, `LHD_SPA_CONTRACT_IDS` |
| System / Verwaltung | `LHD_SPA_LOGGING`, `LHD_SPA_USER_SETTINGS`, `LHD_SPA_VERSION`, `LHD_SPA_CHANGEREQUESTS`, `LHD_SPA_TEXT` |

## 5. Domäne Buchung / Events

### 5.1 Zentrale Tabelle `LHD_SPA_EVENTS`

`LHD_SPA_EVENTS` ist der zentrale fachliche Buchungskern von SportFM.

Wichtige erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `ID_EVENT` | technische Event-ID |
| `ID_PARENT_EVENT` | Parent-/Child-Bezug für fachliche Zusammenhänge |
| `YEAR` | Buchungsjahr / Nummernkreisbestandteil |
| `ID_BOOKING` | Buchungs-ID innerhalb des Jahres |
| `BOOKING_COUNTER` | Zähler innerhalb einer Buchung |
| `BOOKING_NUMBER` | virtuell gebildete Buchungsnummer |
| `ID_SPA` | Bezug zur Sportanlage / buchbaren Ressource |
| `SPA_NR` | Nummer der Sportanlage |
| `ID_USER` | Bezug zum SportFM-Nutzer |
| `ID_EVENTTYPE` | Belegungsart / Eventtyp |
| `TITLE` | Titel / Kurzbezeichnung |
| `DESCRIPTION` | Beschreibung |
| `DATE_BEGIN`, `DATE_END` | Datumszeitraum der Buchung |
| `TIME_BEGIN`, `TIME_END` | Uhrzeitbereich im Format der bestehenden Logik |
| `DURATION` | virtuell berechnete Dauer |
| `ID_SPORTTYPE` | Sportart |
| `ID_SPORTGROUP` | Sportgruppe |
| `ID_SPORTSUBGROUP` | Sportuntergruppe |
| `IS_RECURRING` | Kennzeichen für wiederkehrende Belegung |
| `IS_ALL_UNIT` | Kennzeichen für gesamte Anlage / alle Teileinheiten |
| `STATE` | Status der Buchung |
| `DEPARTMENT` | zuständige Organisationseinheit |
| `IS_COMPLEXEVENT` | Kennzeichen für Komplexveranstaltung |

Fachlich enthält `LHD_SPA_EVENTS` nicht nur einfache Termine, sondern die Grundlage für Buchungen, Sperrungen, Schulbetrieb, GTA, Veranstaltungen und Stornierungen.

### 5.2 Statuslogik Events

Derzeit gelten für Events folgende Statuswerte:

| Wert | Bedeutung |
|---:|---|
| `1` | aktiv |
| `-1` | gelöscht |

Buchungen werden fachlich nicht historisiert, sondern enden zu einem bestimmten Datum. Historisierungs- oder Änderungsanforderungen dürfen daher nicht ungeprüft aus dem Portal heraus unterstellt werden.

### 5.3 Eventtypen

Produktiv relevante Eventtypen sind derzeit:

- Sperrung,
- Reinigung,
- Schulbetrieb,
- GTA,
- wöchentliche Übungsbelegung,
- 14-tägige Übungsbelegung,
- Veranstaltung,
- Stornierung.

Die Tabellen `LHD_SPA_EVENTTYPES` und `LHD_SPA_EVENTCLASSES` bilden die Typisierung und Klassifizierung ab. Erkennbar sind u. a. Prioritätsinformationen (`PRIO`) und Wiederholungskennzeichen bzw. Wiederholungsintervalle.

### 5.4 Zuordnung zu Teileinheiten

Die Tabelle `LHD_SPA_EVENT2UNIT` bildet die Zuordnung von Events zu Teileinheiten ab. Damit wird abgebildet, ob eine Belegung die gesamte Sportanlage oder konkrete Teileinheiten betrifft.

Für die REST-API ist wichtig: Ein Event darf nicht nur als Termin verstanden werden. Ein Event ist ein fachliches Belegungsobjekt mit Ressource, Nutzer, Zeitraum, Typ, Sportbezug, ggf. Wiederholungsmuster und Teileinheitenbezug.

## 6. Domäne Wiederholungen

### 6.1 Tabelle `LHD_SPA_RECURRING_PATTERN`

Die Tabelle `LHD_SPA_RECURRING_PATTERN` enthält die strukturellen Angaben für wiederkehrende Belegungen.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `FREQ` | Frequenz |
| `INTERVAL` | Intervall |
| `MAX_OCCURRENCES` | maximale Anzahl Vorkommen |
| `DAYS` | Wochentage / Tagesmuster |
| `HOLIDAYS` | Feiertags-/Ferienbehandlung bzw. Maskierung gemäß bestehender Logik |

Die fachliche Ermittlung von Wiederholungen erfolgt nicht im Portal, sondern über die bestehende SportFM-Logik, insbesondere `PA_LHD_SPA`.

### 6.2 Konsequenz für Portal und REST

Das Portal darf Serien nicht eigenständig expandieren. Der Wizard bzw. die REST-API darf nur das gewünschte Muster entgegennehmen. Die konkrete Erzeugung und Bewertung der Termine erfolgt in SportFM.

Beispielhafte fachliche Anfrage an die API:

```text
Training mit wöchentlicher Wiederholung für Zeitraum X beantragen
```

Nicht zulässig als Zielarchitektur:

```text
Portal berechnet selbst alle Einzeltermine und schreibt sie direkt in Tabellen
```

## 7. Domäne Occurrences und Winner

### 7.1 Tabelle `LHD_SPA_OCC`

`LHD_SPA_OCC` bildet konkrete, aus Events abgeleitete Einzelvorkommen ab.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `EVENT_ID` | Bezug zum auslösenden Event |
| `SPA_ID` | Sportanlage |
| `UNIT_ID` | Teileinheit |
| `START_TS`, `END_TS` | konkreter Zeitbereich |
| `DAY_DATE` | Tag des Vorkommens |
| `USER_ID` | Nutzerbezug |
| `EVENTTYPE_ID` | Eventtyp |
| `PRIORITY`, `PRIORITY_ETP` | Prioritätsinformationen |
| `CANCEL_EVENT_ID` | Bezug zu Stornierungslogik |
| `HOLIDAYS_MASK` | Ferien-/Feiertagsmaskierung |
| `CREATED_TS` | Erzeugungszeitpunkt |

Diese Tabelle ist als Performance-Schicht zu verstehen. Sie materialisiert konkrete Termininstanzen aus der fachlichen Event- und Wiederholungslogik.

### 7.2 Tabelle `LHD_SPA_OCC_WINNER`

`LHD_SPA_OCC_WINNER` enthält die resultierende bzw. gültige Belegung nach Anwendung der Winner-Logik.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `SPA_ID` | Sportanlage |
| `UNIT_ID` | Teileinheit |
| `DAY_DATE` | Tag |
| `START_TS`, `END_TS` | gültiger Zeitbereich |
| `EVENT_ID` | maßgebliches Event |
| `OCC_ID` | zugrunde liegende Occurrence |
| `USER_ID` | Nutzer |
| `EVENTTYPE_ID` | Eventtyp |
| `PRIORITY`, `PRIORITY_ETP` | Prioritätsinformationen |
| `CREATED_TS` | Erzeugungszeitpunkt |

Die Winner-Logik ist fachlich entscheidend: Kalender, freie Zeiten und Belegungsanzeigen sollen nicht die rohen Events interpretieren, sondern die resultierende Belegung verwenden.

### 7.3 Hilfstabellen und Jobs

Ergänzend existieren:

| Tabelle | Einordnung |
|---|---|
| `LHD_SPA_OCC_DAY_COVERAGE` | Abdeckung / Aufbaustand pro Tag |
| `LHD_SPA_OCC_EVENT_DRT` | Dirty-/Retry-Informationen für Event-bezogene Occurrence-Ermittlung |
| `LHD_SPA_OCC_WINNER_DRT` | Dirty-/Retry-Informationen für Winner-Ermittlung |

Fachlich wurde bestätigt:

- Occurrence-/Winner-Ermittlung ist umgesetzt,
- Berechnung läuft stündlich,
- zusätzlich existiert ein Fast Path,
- Details der internen Berechnung sind für dieses Kapitel nicht neu zu spezifizieren.

## 8. Domäne Dokumente

### 8.1 Tabelle `LHD_SPA_DOCUMENTS`

`LHD_SPA_DOCUMENTS` ist die zentrale Dokumententabelle von SportFM.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `ID_SPORTSUSER` | Nutzerbezug |
| `ID_DOCUMENT_TYPE` | Dokumenttyp |
| `ID_DOCUMENT_STATE` | Dokumentstatus |
| `TITLE` | Titel |
| `DESCRIPTION` | Beschreibung |
| `FILENAME` | Dateiname |
| `MIMETYPE` | MIME-Type |
| `CONTENT` | Dokumentinhalt / BLOB |
| `DOCUMENT_VERSION` | Dokumentversion |
| `UPDATE_VERSION` | Aktualisierungsversion |
| `DATE1`, `USER1` | Erstellung / Erstbearbeitung |
| `DATE2`, `USER2` | Änderung |
| `YEAR` | Jahr für Nummerierung |
| `DOCUMENT_COUNTER` | Dokumentzähler |
| `DOCUMENT_NUMBER` | Dokumentnummer |
| `ID_INVOICE` | Rechnungsbezug |

### 8.2 Statuslogik Dokumente

Derzeit gelten folgende Statuswerte:

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |
| `2` | bearbeitet |
| `3` | gedruckt |

### 8.3 Dokumenttypen, Vorlagen und Textmodule

Weitere relevante Tabellen:

| Tabelle | Bedeutung |
|---|---|
| `LHD_SPA_DOCUMENT_TYPES` | Dokumenttypen |
| `LHD_SPA_DOCUMENT_TEMPLATES` | Dokumentvorlagen |
| `LHD_SPA_DOCUMENT_TEXTMODULES` | Textbausteine |
| `LHD_SPA_DOCUMENTS_EVENTS` | Zuordnung zwischen Dokumenten und Events |
| `LHD_SPA_DOCUMENT_NUMBERS` | Nummernkreis für Dokumente |

Fachlich wurde festgelegt: Grundsätzlich sollen alle Dokumente portalgeeignet abrufbar sein, sofern der Portalnutzer fachlich berechtigt ist. Jeder Benutzer sieht seine eigenen Dokumente.

Konsequenz: Die Portal- und REST-Logik benötigt keine künstliche fachliche Einschränkung auf bestimmte Dokumentarten. Entscheidend ist eine saubere Zugriffskontrolle.

## 9. Domäne Gebühren

### 9.1 Tabelle `LHD_SPA_CHARGES`

`LHD_SPA_CHARGES` enthält die Gebührenkonfiguration.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `DESCRIPTION` | Beschreibung |
| `ID_CHARGEGROUP` | Gebührengruppe |
| `VALID_FROM`, `VALID_UNTIL` | Gültigkeitszeitraum |
| `AMOUNT` | Betrag |
| `BRUTTO` | Brutto-Kennzeichen / Bruttowert gemäß bestehender Logik |
| `VAT` | Umsatzsteuer |
| `ID_FACILITYGROUP` | Sportanlagengruppe |
| `CALC_TIMESPAN` | Berechnungszeitraum |
| `CALC_FREQUENCY` | Berechnungshäufigkeit |
| `TIMEUNIT` | Zeiteinheit |
| `ID_CHARGETYPE` | Gebührentyp |
| `UNIT_SPLIT` | Teileinheitenaufteilung |
| `DAY_ACTIVE_FROM`, `DAY_ACTIVE_UNTIL` | Tagesbezogene Gültigkeit |
| `TIME_ACTIVE_FROM`, `TIME_ACTIVE_UNTIL` | Zeitbezogene Gültigkeit |
| `STANDARD` | Standardkennzeichen |

### 9.2 Weitere Gebührentabellen

| Tabelle | Bedeutung |
|---|---|
| `LHD_SPA_CHARGEGROUPS` | Gebührengruppen |
| `LHD_SPA_CHARGETYPES` | Gebührentypen |
| `LHD_SPA_CHARGE2FACILITYGROUP` | Zuordnung Gebühren zu Sportanlagengruppen |
| `LHD_SPA_EVENTCHARGES` | Zuordnung Gebühren zu Events |
| `LHD_SPA_EVENTCHARGES_HIST` | historische Informationen zu Eventgebühren |

Die Gebührenberechnung bleibt Bestandteil von SportFM und insbesondere der vorhandenen Logik in `PA_LHD_SPA` sowie der bestehenden Funktionalität um `p_get_charges`.

Das Portal darf Gebühren nur anzeigen oder anfragen, wenn SportFM diese berechnet oder freigibt. Eine zweite Gebührenlogik im Portal ist ausgeschlossen.

## 10. Domäne Rechnungen

### 10.1 Tabelle `LHD_SPA_INVOICES`

`LHD_SPA_INVOICES` enthält die Rechnungen und SAP-Bezüge.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `ID_USER` | Nutzerbezug |
| `DATE_BEGIN_INVOICE`, `DATE_END_INVOICE` | Abrechnungszeitraum |
| `STATE` | Rechnungsstatus |
| `USER1`, `DATE1` | Erstellung |
| `USER2`, `DATE2` | Änderung |
| `SAP_DOCUMENT_ID` | SAP-Belegnummer |
| `SAP_DOCUMENT_YEAR` | SAP-Belegjahr |
| `SAP_REVERSAL_REASONCODE` | SAP-Stornogrund |
| `SAP_REVERSAL_DOCUMENT_ID` | SAP-Stornobeleg |
| `SAP_REVERSAL_DOCUMENT_YEAR` | SAP-Stornobelegjahr |
| `SAP_REVERSALTYPE` | SAP-Stornoart |
| `SAP_USER1`, `SAP_DATE1` | SAP-Erstellung / Übergabe |
| `SAP_USER2`, `SAP_DATE2` | SAP-Änderung / Storno |
| `REMARK` | Bemerkung |

### 10.2 Statuslogik Rechnungen

Derzeit gelten folgende Statuswerte:

| Wert | Bedeutung |
|---:|---|
| `-1` | gelöscht |
| `1` | aktuell |

### 10.3 Rechnungspositionen / Gebühreninformationen

`LHD_SPA_INVOICE_CHARGEINFOS` enthält Gebühreninformationen für Rechnungen.

Erkennbare Spalten:

| Spalte | Bedeutung / Einordnung |
|---|---|
| `ID_CHARGE` | Gebühr |
| `DATE_BEGIN_INVOICE`, `DATE_END_INVOICE` | Abrechnungszeitraum |
| `DURATION` | Dauer |
| `COUNT_APPOINTMENTS` | Anzahl Termine |
| `COUNT_UNIT_AMOUNT` | Anzahl / Menge der Einheiten |
| `FACILITY_AMOUNT_ALL_UNIT` | Anlagenbetrag gesamte Einheit |
| `FACILITY_AMOUNT_PER_UNIT` | Anlagenbetrag je Einheit |
| `APPOINTMENT_AMOUNT_ALL_UNIT` | Terminbetrag gesamte Einheit |
| `APPOINTMENT_AMOUNT_PER_UNIT` | Terminbetrag je Einheit |
| `AMOUNT` | Betrag |
| `STATE` | Status |
| `ID_INVOICE` | Rechnungsbezug |

Fachlich wurde festgelegt: Jeder Portalnutzer sieht seine eigenen Rechnungen. Die bestehende SAP-Anbindung erfolgt bereits über einen API-Client, OAuth-Client und REST-API zu SAP. Für das Portal ist daher keine zweite SAP-Integration zu entwerfen.

## 11. Domäne Sportstruktur

Die Sportstruktur dient der fachlichen Klassifikation von Buchungen und Suchfunktionen.

| Tabelle | Bedeutung |
|---|---|
| `LHD_SPA_SPORTTYPES` | Sportarten |
| `LHD_SPA_SPORTCATEGORIES` | Sportkategorien |
| `LHD_SPA_SPORTGROUPS` | Sportgruppen |
| `LHD_SPA_SPORTSUBGROUPS` | Sportuntergruppen |
| `LHD_SPA_SPORTSCOMPLEXES` | Sportkomplexe |
| `LHD_SPA_FACILITY2COMPLEX` | Zuordnung Sportanlage zu Sportkomplex |
| `LHD_SPA_FACILITYGROUPS` | Sportanlagengruppen |

Das Lastenheft beschreibt RIB FM als führendes System für Stammdaten zu Nutzern, Gebäuden, Standorten und Sportanlagen. Die SportFM-Tabellen ergänzen diese Stammdaten um sportfachliche Klassifikationen und Zuordnungen.

## 12. Domäne System / Verwaltung

Weitere Tabellen unterstützen Betrieb, Nummernkreise und Verwaltung.

| Tabelle | Bedeutung |
|---|---|
| `LHD_SPA_BOOKING_NUMBERS` | Nummernkreis Buchungen |
| `LHD_SPA_DOCUMENT_NUMBERS` | Nummernkreis Dokumente |
| `LHD_SPA_CONTRACT_IDS` | Vertragszuordnung zu Sportanlagen / Schule / Nutzer |
| `LHD_SPA_LOGGING` | Protokollierung |
| `LHD_SPA_USER_SETTINGS` | Benutzereinstellungen |
| `LHD_SPA_VERSION` | Versionsinformationen |
| `LHD_SPA_CHANGEREQUESTS` | Änderungsanforderungen / interne Nachverfolgung |
| `LHD_SPA_TEXT` | Texte / Textmarken |

`LHD_SPA_LOGGING` enthält u. a. Datum, Benutzer, Modul, Funktion, Parameter und Nachricht. Für die Plattformarchitektur ist zu prüfen, ob dieses Logging für REST-Zugriffe erweitert oder durch ein zusätzliches API-Logging ergänzt wird.

## 13. Abgrenzungen und Nicht-Ziele

Dieses Kapitel beschreibt nicht:

- vollständige physische Datenbankdokumentation,
- vollständige ER-Diagramme,
- interne Implementierung der PL/SQL-Packages,
- Neuentwurf der Gebührenlogik,
- Neuentwurf der Occurrence-/Winner-Berechnung,
- Neuentwurf der SAP-Anbindung.

Diese Bereiche sind entweder bereits produktiv vorhanden oder werden in separaten Kapiteln bzw. technischen Konzepten behandelt.

## 14. Konsequenzen für REST-API und Portal

Aus dem Oracle-Datenmodell ergeben sich folgende verbindliche Konsequenzen:

1. Die REST-API darf keine rohe Tabellen-API werden.
2. `LHD_SPA_EVENTS` wird nicht direkt als generischer CRUD-Endpunkt veröffentlicht.
3. Fachoperationen müssen im Vordergrund stehen: Antrag stellen, Reservierung prüfen, Buchung erzeugen, Stornierung erzeugen, Gebühren berechnen, Dokument abrufen, Rechnung abrufen.
4. Portal und WPF dürfen keine eigene Wiederholungs-, Gebühren-, Stornierungs- oder Winner-Logik implementieren.
5. Kalender und freie Zeiten müssen auf der vorhandenen Occurrence-/Winner-Logik basieren.
6. Dokumente und Rechnungen werden nutzerbezogen über Berechtigungen bereitgestellt.
7. SAP bleibt über die bestehende REST/OAuth-Anbindung in SportFM angebunden.
8. Neue Portal-Anträge werden nicht direkt in die bestehenden Buchungstabellen geschrieben, sondern über definierte Fachprozesse in SportFM überführt.

## 15. Offene Punkte für spätere Detailkapitel

Die folgenden Punkte sind für dieses Kapitel nicht zu erfinden und später fachlich bzw. technisch gesondert zu klären:

| Punkt | Klärung |
|---|---|
| Portal-Antragsdatenmodell | Neues Datenmodell für Wizard-/Antragsframework erforderlich |
| Workflowstatus von Anträgen | gesondert im Kapitel Antrag / Workflow festzulegen |
| Mapping Antrag → Reservierung → Buchung | fachlich im Pflichtenheft und technisch in der API-Spezifikation zu beschreiben |
| API-Berechtigungsmodell | separates Kapitel Rollen und Berechtigungen |
| Portal-Benutzerverwaltung | separates Kapitel Benutzerverwaltung / Authentifizierung |
| Dokumentfreigabe / Sichtbarkeit | grundsätzlich alle eigenen Dokumente, technische Regeln dennoch festzulegen |
| Rechnungsanzeige / ePayment | eigenes Schnittstellen- und Prozesskapitel |

## 16. Vorläufige Bewertung

Das Oracle-Datenmodell zeigt eine bereits belastbare fachliche Struktur. Besonders stark sind:

- ein einheitliches Eventmodell für alle Belegungsarten,
- eine materialisierte Occurrence-Schicht für Performance,
- eine Winner-Logik für die resultierende Belegung,
- vorhandene Gebühren- und Rechnungsstrukturen,
- vorhandene Dokumenten- und Vorlagenstrukturen,
- vorhandene PL/SQL-Packages für zentrale Fachlogik,
- bestehende SAP-Integration über REST/OAuth.

Damit bestätigt das Datenmodell die strategische Projektausrichtung:

> SportFM wird nicht durch das Portal ersetzt. SportFM wird zur Plattform weiterentwickelt, deren bestehende Oracle- und PL/SQL-Fachlogik über eine moderne REST-API kontrolliert nutzbar gemacht wird.
