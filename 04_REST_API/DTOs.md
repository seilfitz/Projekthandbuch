# DTOs

## Ziel dieses Dokuments

Dieses Dokument beschreibt die Grundsätze für Datenübertragungsobjekte der SportFM-REST-API.

DTOs definieren die äußere Vertragsstruktur der REST-API.

Sie sind nicht identisch mit Oracle-Tabellen, Views oder PL/SQL-Record-Strukturen.

## Grundsatz

Die REST-API veröffentlicht fachliche Datenmodelle.

Interne Tabellen- und Package-Strukturen bleiben verborgen.

```text
Oracle / PL-SQL
      ↓
interne Datenstruktur
      ↓
Mapping
      ↓
DTO
      ↓
REST-Client
```

## Leitprinzipien

| Nr. | Prinzip |
|---:|---|
| DTO-001 | DTOs bilden fachliche Sichtweisen ab, keine Tabellen. |
| DTO-002 | DTOs enthalten nur Daten, die für den jeweiligen Anwendungsfall benötigt werden. |
| DTO-003 | Listen-DTOs sind kompakt. Detail-DTOs enthalten fachliche Zusatzinformationen. |
| DTO-004 | Schreib-DTOs werden von Lese-DTOs getrennt. |
| DTO-005 | Portalinterne UI-Modelle sind keine REST-DTOs. |
| DTO-006 | Bestehende SportFM-Logik wird nicht durch DTOs neu definiert. |
| DTO-007 | IDs werden nur als technische Referenzen verwendet, nicht als fachliche Logik. |

## Benennung

DTOs werden nach fachlichem Zweck benannt.

| Muster | Verwendung |
|---|---|
| `...ListeDto` | kompakte Listenansicht |
| `...DetailDto` | Detailansicht |
| `...AnlageDto` | Anlage oder Unterobjekt |
| `...AnfrageDto` | Anfragekörper für POST/PUT |
| `...ErgebnisDto` | Ergebnis einer fachlichen Aktion |
| `...FilterDto` | Such- und Filterkriterien |

Beispiele:

```text
BuchungListeDto
BuchungDetailDto
AntragDetailDto
RechnungListeDto
GebuehrDetailDto
FreieZeitenFilterDto
```

## Sprache

Im Projekthandbuch werden deutsche Begriffe verwendet.

In der späteren technischen Umsetzung kann eine englische Codebenennung verwendet werden, sofern dies projekteinheitlich festgelegt wird.

Für dieses Kapitel gelten deutsche Fachbegriffe.

## Allgemeine DTO-Strukturen

### SeitenErgebnisDto

Für Listen mit Paging.

```text
SeitenErgebnisDto<T>

Elemente
Seite
Seitengroesse
Gesamtanzahl
HatWeitereSeite
```

### FehlerDto

Siehe `Fehlerformat.md`.

```text
FehlerDto

Fehlercode
Meldung
Details
KorrelationsId
Zeitpunkt
```

### AuswahlwertDto

Für einfache Auswahldaten.

```text
AuswahlwertDto

Id
Bezeichnung
Kurzbezeichnung
Aktiv
Sortierung
```

## Booking-DTOs

Booking beschreibt bestehende SportFM-Buchungen.

Die DTOs dienen der lesenden Bereitstellung im Portal.

Die bestehende Buchungslogik wird nicht neu modelliert.

### BuchungListeDto

```text
BuchungId
Sportanlage
Teileinheit
Belegungsart
Von
Bis
Status
```

### BuchungDetailDto

```text
BuchungId
Sportanlage
Sportkomplex
Teileinheit
Belegungsart
Zeitraum
Nutzer
Status
Dokumente
Gebuehren
Rechnungen
```

### BuchungFilterDto

```text
ZeitraumVon
ZeitraumBis
SportanlageId
SportkomplexId
BelegungsartId
Status
```

## Availability-DTOs

Availability beschreibt freie Zeiten und Belegungsinformationen.

Die Berechnung basiert auf der vorhandenen Occurrence- und Winner-Logik.

### FreieZeitenFilterDto

```text
ZeitraumVon
ZeitraumBis
UhrzeitVon
UhrzeitBis
SportanlageId
SportartId
StadtbezirkId
Wochentage
```

### FreieZeitDto

```text
SportanlageId
Sportanlage
TeileinheitId
Teileinheit
Datum
UhrzeitVon
UhrzeitBis
DauerMinuten
Verfuegbar
```

### BelegungDto

```text
SportanlageId
TeileinheitId
Datum
UhrzeitVon
UhrzeitBis
Belegungsart
Titel
Status
```

## Application-DTOs

Application beschreibt Portal-Anträge.

Ein Antrag ist nicht automatisch eine Buchung.

### AntragTypDto

```text
AntragTypId
Bezeichnung
Beschreibung
Aktiv
Version
```

### AntragListeDto

```text
AntragId
AntragTyp
Titel
Status
ErstelltAm
EingereichtAm
LetzteAenderungAm
```

### AntragDetailDto

```text
AntragId
AntragTyp
Status
Kontext
Daten
Dateien
Historie
Aufgaben
```

### AntragSpeichernAnfrageDto

```text
AntragId
AntragTypId
Daten
Dateien
```

### AntragEinreichenErgebnisDto

```text
AntragId
Status
EingereichtAm
Meldung
```

## Document-DTOs

Document beschreibt Dokumente aus SportFM und portalrelevante Dateiinformationen.

Dokumente werden nicht im Portal dupliziert.

### DokumentListeDto

```text
DokumentId
Titel
Dokumenttyp
Datum
Status
Dateiname
MimeType
```

### DokumentDetailDto

```text
DokumentId
Titel
Beschreibung
Dokumenttyp
Status
Dateiname
MimeType
Version
ErstelltAm
GeaendertAm
```

### DateiDto

```text
DateiId
Dateiname
MimeType
GroesseBytes
ErstelltAm
```

## Invoice-DTOs

Invoice beschreibt Rechnungen.

Rechnungen werden nicht im Portal erzeugt.

### RechnungListeDto

```text
RechnungId
Rechnungsnummer
ZeitraumVon
ZeitraumBis
Status
Betrag
Waehrung
Datum
```

### RechnungDetailDto

```text
RechnungId
Rechnungsnummer
ZeitraumVon
ZeitraumBis
Status
Betrag
Waehrung
Dokumente
Gebuehren
SapBezug
```

## Charge-DTOs

Charge beschreibt Gebühren und Gebührenpositionen.

Charge ist nicht Invoice.

### GebuehrListeDto

```text
GebuehrId
Beschreibung
ZeitraumVon
ZeitraumBis
Betrag
Waehrung
```

### GebuehrDetailDto

```text
GebuehrId
Beschreibung
Gebuehrengruppe
Gebuehrenart
ZeitraumVon
ZeitraumBis
Betrag
Brutto
Mehrwertsteuer
Berechnungsgrundlage
```

## ReferenceData-DTOs

ReferenceData beschreibt Auswahldaten und Stammdaten für Portal und REST-Clients.

### SportanlageAuswahlDto

```text
SportanlageId
Bezeichnung
Adresse
Stadtbezirk
Aktiv
```

### SportartAuswahlDto

```text
SportartId
Bezeichnung
Kategorie
Aktiv
```

### BelegungsartAuswahlDto

```text
BelegungsartId
Bezeichnung
Aktiv
```

## Authentifizierungs-DTOs

Die fachliche Ausarbeitung erfolgt in `Authentifizierung.md`.

### AnmeldungAnfrageDto

```text
Benutzername
Passwort
```

### AnmeldungErgebnisDto

```text
Zugriffstoken
Aktualisierungstoken
LaeuftAbAm
Benutzer
Kontexte
```

### KontextDto

```text
KontextId
OrganisationId
SportFmNutzerId
Rollen
Aktiv
```

## Pflichtangaben je DTO

Jedes DTO muss in der späteren Spezifikation mindestens folgende Angaben erhalten:

| Feld | Beschreibung |
|---|---|
| Name | eindeutiger DTO-Name |
| Zweck | fachlicher Zweck |
| Verwendet in | Endpunkte, die das DTO nutzen |
| Felder | Feldliste |
| Pflichtfelder | fachlich zwingende Felder |
| Datentypen | technische Datentypen |
| Beispiel | Beispielstruktur |
| Offene Punkte | noch zu klärende Felder |

## Abgrenzungen

### Facility und Booking

Facility beschreibt Sportanlagen, Sportkomplexe und Teileinheiten.

Booking beschreibt Buchungen und Belegungen.

Diese DTOs werden getrennt.

### Charge und Invoice

Charge beschreibt Gebühren und Gebührenpositionen.

Invoice beschreibt Rechnungen.

Diese DTOs werden getrennt.

### Application und Booking

Application beschreibt Anträge.

Booking beschreibt Buchungen.

Ein Antrag wird nicht automatisch als Buchung modelliert.

## Nicht-Ziele

DTOs dienen nicht dazu:

- Oracle-Tabellen vollständig nach außen zu geben
- bestehende PL/SQL-Logik nachzubauen
- Portal-UI-Zustände dauerhaft zu speichern
- Gebührenlogik im Portal abzubilden
- interne Statusmodelle ungeprüft zu veröffentlichen

## Offene Punkte

| ID | Frage |
|---|---|
| OF-DTO-001 | endgültige technische Datentypen festlegen |
| OF-DTO-002 | verbindliche JSON-Namenskonvention festlegen |
| OF-DTO-003 | Paging-Standard finalisieren |
| OF-DTO-004 | Sortier- und Filterstandard finalisieren |
| OF-DTO-005 | Beispielantworten je Endpunkt ergänzen |

## Nächster Schritt

Nach diesem Grundsatzdokument werden die DTOs je Endpunktgruppe schrittweise konkretisiert.

Die Detailausarbeitung erfolgt nicht durch Neuerfindung der Fachlogik, sondern aus den vorhandenen SportFM-Daten und Package-Rückgaben.