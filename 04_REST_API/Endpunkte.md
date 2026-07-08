# Endpunkte

## Ziel dieses Dokuments

Dieses Dokument ist die zentrale Übersicht der REST-Endpunkte der SportFM-Plattform.

Die fachliche Detailbeschreibung erfolgt in den Dateien unter:

```text
04_REST_API/Endpunkte/
```

## Grundsatz

Die REST-API bildet fachliche Domänen ab.

Sie ist keine direkte Abbildung des Oracle-Datenmodells.

Nicht leitend sind Tabellen wie:

```text
LHD_SPA_EVENTS
LHD_SPA_OCC
LHD_SPA_OCC_WINNER
LHD_SPA_CHARGES
LHD_SPA_INVOICES
```

Leitend sind fachliche Schnittstellen wie:

```text
Booking
Availability
Application
Document
Invoice
Charge
ReferenceData
```

## Fachliche Endpunktgruppen

| Gruppe | Datei | Zweck | V1 |
|---|---|---|:---:|
| Authentication | `Authentifizierung.md` | Anmeldung, Token, 2FA, OAuth | ja |
| Booking | `Endpunkte/Booking.md` | Buchungen lesen | ja |
| Availability | `Endpunkte/Availability.md` | freie Zeiten und Belegung | ja |
| Application | `Endpunkte/Application.md` | Anträge, Entwürfe, Einreichung | ja |
| Document | `Endpunkte/Document.md` | Dokumente anzeigen und herunterladen | ja |
| Invoice | `Endpunkte/Invoice.md` | Rechnungen anzeigen | ja |
| Charge | `Endpunkte/Charge.md` | Gebühreninformationen anzeigen | ja |
| ReferenceData | `Endpunkte/ReferenceData.md` | Auswahldaten und Stammdaten | ja |

## V1-Schnittstellenlandkarte

```text
SportPortal
   ↓
REST-API
   ├─ Authentication
   ├─ Application
   ├─ Workflow
   ├─ Booking
   ├─ Availability
   ├─ Document
   ├─ Invoice
   ├─ Charge
   └─ ReferenceData
        ↓
SportFM / PL-SQL / Oracle
```

## Endpunktgruppe Authentication

Die Authentifizierung wird als Querschnittsfunktion dokumentiert.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| POST | `/auth/register` | Portalbenutzer registrieren | geplant |
| POST | `/auth/login` | Benutzer anmelden | geplant |
| POST | `/auth/refresh` | Zugriffstoken erneuern | geplant |
| POST | `/auth/logout` | Sitzung beenden | geplant |
| POST | `/auth/password-reset/request` | Passwort-Zurücksetzen anfordern | geplant |
| POST | `/auth/password-reset/confirm` | Passwort neu setzen | geplant |
| POST | `/auth/2fa/setup` | Zwei-Faktor-Authentifizierung einrichten | geplant |
| POST | `/auth/2fa/verify` | Zwei-Faktor-Code prüfen | geplant |

Detail: `Authentifizierung.md`

## Endpunktgruppe Application

Die Application-Endpunkte bilden die neue Antragsschicht des Portals ab.

Ein Antrag ist keine Buchung.

```text
Antrag
  ↓
Workflow
  ↓
fachliche Prüfung
  ↓
SportFM-Buchung
```

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/applications/types` | verfügbare Antragstypen laden | geplant |
| POST | `/applications` | Antrag anlegen | geplant |
| GET | `/applications` | eigene Anträge laden | geplant |
| GET | `/applications/{id}` | Antrag laden | geplant |
| PUT | `/applications/{id}` | Entwurf speichern | geplant |
| POST | `/applications/{id}/submit` | Antrag einreichen | geplant |
| GET | `/applications/{id}/history` | Antragshistorie laden | geplant |
| POST | `/applications/{id}/files` | Datei zum Antrag hochladen | geplant |

Detail: `Endpunkte/Application.md`

## Endpunktgruppe Booking

Booking stellt bestehende SportFM-Buchungen lesend bereit.

Die Buchungslogik wird nicht neu entworfen.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/bookings` | Buchungsliste für berechtigten Kontext laden | geplant |
| GET | `/bookings/{id}` | Buchungsdetails laden | geplant |
| GET | `/bookings/{id}/documents` | Dokumente zur Buchung laden | geplant |
| GET | `/bookings/{id}/charges` | Gebühren zur Buchung laden | geplant |
| GET | `/bookings/{id}/invoices` | Rechnungen zur Buchung laden | geplant |

Detail: `Endpunkte/Booking.md`

## Endpunktgruppe Availability

Availability stellt freie Zeiten und Belegungsinformationen bereit.

Die Ermittlung nutzt die bestehende Occurrence- und Winner-Logik.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/availability` | freie Zeiten suchen | geplant |
| GET | `/availability/occupancy` | Belegung für Zeitraum laden | geplant |
| GET | `/availability/facilities/{id}` | Verfügbarkeit einer Sportanlage laden | geplant |

Detail: `Endpunkte/Availability.md`

## Endpunktgruppe Document

Document stellt bestehende SportFM-Dokumente portalgeeignet bereit.

Dokumente werden nicht im Portal dupliziert.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/documents` | eigene Dokumente laden | geplant |
| GET | `/documents/{id}` | Dokumentmetadaten laden | geplant |
| GET | `/documents/{id}/content` | Dokument herunterladen | geplant |
| GET | `/bookings/{id}/documents` | Dokumente zu einer Buchung laden | geplant |
| GET | `/invoices/{id}/documents` | Dokumente zu einer Rechnung laden | geplant |

Detail: `Endpunkte/Document.md`

## Endpunktgruppe Invoice

Invoice stellt bestehende Rechnungen portalgeeignet bereit.

Rechnungen werden nicht im Portal erzeugt.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/invoices` | eigene Rechnungen laden | geplant |
| GET | `/invoices/{id}` | Rechnungsdetails laden | geplant |
| GET | `/invoices/{id}/documents` | Rechnungsdokumente laden | geplant |
| GET | `/invoices/{id}/charges` | Gebührenpositionen zur Rechnung laden | geplant |

Detail: `Endpunkte/Invoice.md`

## Endpunktgruppe Charge

Charge stellt Gebühreninformationen bereit.

Gebühren werden nicht im Portal berechnet.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/bookings/{id}/charges` | Gebühren zu einer Buchung laden | geplant |
| GET | `/invoices/{id}/charges` | Gebührenpositionen einer Rechnung laden | geplant |
| GET | `/charges/{id}` | Gebührenposition laden | geplant |

Detail: `Endpunkte/Charge.md`

## Endpunktgruppe ReferenceData

ReferenceData stellt Auswahldaten und Stammdaten bereit.

Die Daten stammen aus dem bestehenden SportFM-Datenbestand.

| Methode | Pfad | Zweck | Status |
|---|---|---|---|
| GET | `/reference/facilities` | Sportanlagen laden | geplant |
| GET | `/reference/sporttypes` | Sportarten laden | geplant |
| GET | `/reference/eventtypes` | Belegungsarten laden | geplant |
| GET | `/reference/documenttypes` | Dokumenttypen laden | offen |

Detail: `Endpunkte/ReferenceData.md`

## Abgrenzungen

### Facility und Booking

Facility beschreibt Sportanlagen, Sportkomplexe und Teileinheiten.

Booking beschreibt Buchungen und Belegungen.

Diese Domänen werden nicht vermischt.

### Charge und Invoice

Charge beschreibt Gebühren und Gebührenpositionen.

Invoice beschreibt Rechnungen und Abrechnungsobjekte.

Diese Domänen werden nicht vermischt.

### Application und Booking

Application beschreibt Anträge aus dem Portal.

Booking beschreibt bestehende bzw. durch SportFM erzeugte Buchungen.

Ein Antrag wird nicht automatisch zu einer Buchung.

## Statuswerte in diesem Dokument

| Status | Bedeutung |
|---|---|
| geplant | Bestandteil der V1-Planung |
| offen | fachlich oder technisch noch zu klären |
| später | nicht Bestandteil der V1 |
| ausgeschlossen | bewusst nicht vorgesehen |

## Nicht Bestandteil der V1

| Bereich | Grund |
|---|---|
| direkte Tabellen-API | widerspricht der Plattformarchitektur |
| Neuberechnung der Booking-Logik im REST | bestehende SportFM-Logik bleibt führend |
| Gebührenberechnung im Portal | Gebühren bleiben SportFM-Fachlogik |
| Rechnungserzeugung im Portal | Rechnungen bleiben SportFM-/SAP-Prozess |
| vollständige WPF-Ablösung | erfolgt schrittweise |

## Offene Punkte

| ID | Frage |
|---|---|
| OF-END-001 | finale Query-Parameter je Listenendpunkt festlegen |
| OF-END-002 | Paging- und Sortierstandard festlegen |
| OF-END-003 | DTOs je Endpunkt finalisieren |
| OF-END-004 | Performance-Ziele je Endpunktgruppe festlegen |
| OF-END-005 | OpenAPI-Beschreibung erzeugen |

## Nächster Schritt

Nach dieser Übersicht werden die Querschnittsdokumente weiter ausgearbeitet:

- `DTOs.md`
- `Fehlerformat.md`
- `Authentifizierung.md`
- `Versionierung.md`
- `Berechtigungen.md`