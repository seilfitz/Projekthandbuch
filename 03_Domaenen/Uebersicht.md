# Domänenübersicht

| Feld | Wert |
|---|---|
| Kapitel | 03 – Domänen |
| Dokument | Übersicht |
| Status | Konsolidierter Arbeitsstand |
| Leitquellen | `Quellen/2026-07-05_Snapshot1.txt`, Domänendokumente in `03_Domaenen/`, fachliche Bestätigungen und DDL-Auswertungen |

---

## 1 Zweck

Diese Übersicht ordnet die Domänen des SportFM-Projekthandbuchs fachlich ein.

Sie zeigt, welche Domänen Bestandslogik freilegen, welche Domänen neu aufgebaut werden und welche Domänen als Querschnitts- oder Integrationsdomänen wirken.

Die Detailbeschreibung erfolgt in den jeweiligen Domänendokumenten.

---

## 2 Domänenlandkarte

```text
Portal / Blazor
  ↓
Dashboard
  ↓
Authentication ─ PortalUser ─ Organisation ─ Context
  ↓
Application ─ Wizard ─ Workflow
  ↓
Facility ─ Availability ─ Booking
  ↓
Charge ─ Invoice ─ Document
  ↓
Upload / Notification / Administration
```

---

## 3 Domänen nach fachlicher Rolle

### 3.1 Portal- und Benutzergrundlagen

| Domäne | Typ | Kurzbeschreibung |
|---|---|---|
| `Authentication` | Neuentwicklung | Anmeldung, Registrierung, Token, Passwortprozesse, technische Identität |
| `PortalUser` | Neuentwicklung | fachliches Benutzerprofil, Einstellungen, Favoriten, Zustimmungen |
| `Organisation` | Neuentwicklung / Erweiterung | Organisationen, Abteilungen, Mitgliedschaften, Rollenbezug |
| `Context` | Neuentwicklung | aktiver fachlicher Arbeitsraum und Sichtbarkeitskontext |
| `Administration` | Neuentwicklung / Erweiterung | Verwaltung, Konfiguration, administrative Sichten und Audit |

### 3.2 Antrag und Bearbeitung

| Domäne | Typ | Kurzbeschreibung |
|---|---|---|
| `Application` | Neuentwicklung | Onlineanträge, Entwürfe, Antragseinreichung, Antragsdaten |
| `Wizard` | Neuentwicklung | dynamische Antragserfassung, Schritte, Felder, Validierungen |
| `Workflow` | Neuentwicklung / Erweiterung | Bearbeitung, Rückfragen, Statuswechsel, Aufgaben |
| `Upload` | Neuentwicklung | Dateiannahme, technische Prüfung, Zuordnung, Übergabe an Document |

### 3.3 Sportstätten, Verfügbarkeit und Buchung

| Domäne | Typ | Kurzbeschreibung |
|---|---|---|
| `Facility` | Bestandsdomäne / REST-Freilegung | Sportkomplexe, Sportanlagen, Teileinheiten, FacilityGroups |
| `Availability` | Bestand / Erweiterung | freie Zeiten, Kalender, Belegungen, Konfliktprüfung |
| `Booking` | Bestandsdomäne / REST-Freilegung | Events, Buchungen, Occurrences, Winner, Kalenderlogik, Sportreferenzen |

### 3.4 Gebühren, Rechnungen und Dokumente

| Domäne | Typ | Kurzbeschreibung |
|---|---|---|
| `Charge` | Bestandsdomäne / REST-Freilegung | Gebührenstammdaten, Gebührentypen, EventCharges, Gebührenvorschau, InvoiceChargeInfos |
| `Invoice` | Bestandsdomäne / Erweiterung | Rechnungen, Zahlstatus, SAP-Bezug, Rechnungs-PDF über Document |
| `Document` | Bestandsdomäne / Erweiterung | Dokumente, Dokumenttypen, Vorlagenbestand, PDF-Inhalte, Download |

### 3.5 Querschnitt

| Domäne | Typ | Kurzbeschreibung |
|---|---|---|
| `Dashboard` | Neuentwicklung | kontextbezogene Startseite, Aggregation vorhandener Services |
| `Notification` | Neuentwicklung | Portalnachrichten, MailQueue, Ereignisbenachrichtigungen |

---

## 4 Bestandsdomänen

Diese Domänen besitzen führende Oracle- und PL/SQL-Bestandslogik. Sie werden nicht neu erfunden, sondern über REST und Portal kontrolliert freigelegt.

| Domäne | Führender Bestand | Projektwirkung |
|---|---|---|
| `Booking` | `LHD_SPA_EVENTS`, `LHD_SPA_OCC*`, `PA_LHD_SPA`, `PA_LHD_SPA_OCC` | REST- und Portal-Freilegung, keine neue Buchungslogik |
| `Facility` | Sportstätten- und FacilityGroup-Stammdaten | lesende Stammdatenbereitstellung |
| `Charge` | `LHD_SPA_CHARGES`, `LHD_SPA_EVENTCHARGES`, `PA_LHD_SPA.p_get_charges` | Gebührenanzeige, keine neue Gebührenberechnung |
| `Invoice` | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICE_CHARGEINFOS` | Rechnungsliste, Details, Zahlstatus, PDF-Bezug |
| `Document` | `LHD_SPA_DOCUMENTS`, Dokumenttypen, Vorlagen, Textbausteine | Dokumentliste, Download, Upload-Übernahme |
| `Availability` | Occurrence-/Winner-Bestand | freie Zeiten und Kalender, keine eigene Buchungserzeugung |

---

## 5 Neuentwicklungsdomänen

Diese Domänen entstehen für das neue Portal bzw. die neue Plattformschicht.

| Domäne | Hauptzweck |
|---|---|
| `Authentication` | technische Identität und Zugriff |
| `PortalUser` | fachliches Benutzerprofil |
| `Context` | Arbeits- und Sichtbarkeitsraum |
| `Application` | Onlineanträge |
| `Wizard` | dynamische Erfassung |
| `Workflow` | Bearbeitungssteuerung und Rückfragen |
| `Upload` | kontrollierter Dateiupload |
| `Notification` | Portal- und Mailbenachrichtigung |
| `Dashboard` | Aggregation für die Startseite |
| `Administration` | Verwaltung und Konfiguration |

---

## 6 Zentrale fachliche Abgrenzungen

| Abgrenzung | Festlegung |
|---|---|
| Facility vs. Booking | Facility beschreibt Sportstätten. Booking beschreibt die konkrete Nutzung / Buchung. Sportarten, Sportgruppen, Sportuntergruppen und Sportkategorien gehören zu Booking / Event. |
| Availability vs. Booking | Availability liest Belegungen und freie Zeiten. Booking bleibt führend für Buchungen, Occurrences und Winner. |
| Charge vs. Invoice | Charge beschreibt Gebühren und Gebühreninformationen. Invoice beschreibt Rechnungen und Zahlstatus. |
| Upload vs. Document | Upload prüft und übergibt Dateien. Document verwaltet Dokumente dauerhaft. |
| Authentication vs. Context | Authentication identifiziert Benutzer. Context bestimmt den fachlichen Sichtbarkeitsraum. |
| Dashboard vs. Fachdomänen | Dashboard aggregiert nur. Es besitzt keine eigene Fachlogik und keine eigene Persistenz V1. |

---

## 7 Wichtigste Domänenbeziehungen

| Ausgang | Ziel | Beziehung |
|---|---|---|
| `Authentication` | `PortalUser` | technische Identität wird mit fachlichem Profil verknüpft |
| `PortalUser` | `Organisation` | Mitgliedschaften und organisatorische Zuordnungen |
| `Organisation` | `Context` | verfügbare Kontexte werden aus Mitgliedschaften abgeleitet |
| `Context` | alle Fachdomänen | Sichtbarkeit und Zugriff werden kontextbezogen begrenzt |
| `Application` | `Wizard` | Wizard erfasst Antragsdaten |
| `Application` | `Workflow` | eingereichte Anträge gehen in Bearbeitung |
| `Application` | `Upload` | Anlagen werden hochgeladen und zugeordnet |
| `Application` | `Charge` | Gebührenhinweise können angezeigt werden |
| `Facility` | `Availability` | Facility liefert Anlagen und Teileinheiten für Verfügbarkeiten |
| `Availability` | `Booking` | Availability nutzt Belegungen / Winner aus Booking |
| `Booking` | `Charge` | Buchungen / Events besitzen Gebührenbezüge |
| `Charge` | `Invoice` | rechnungsrelevante Gebühreninformationen fließen in Rechnungen |
| `Invoice` | `Document` | Rechnungs-PDF / Gebührenbescheid wird über Document bereitgestellt |
| `Workflow` | `Notification` | Rückfragen und Entscheidungen erzeugen Benachrichtigungen |
| `Document` | `Notification` | neue Dokumente können Benachrichtigungen erzeugen |
| `Dashboard` | mehrere Domänen | Startseite aggregiert Aufgaben, Anträge, Dokumente, Rechnungen und Benachrichtigungen |

---

## 8 Status der Domänendokumente

| Dokument | Status |
|---|---|
| `Administration.md` | ausgearbeitet |
| `Application.md` | ausgearbeitet |
| `Authentication.md` | ausgearbeitet |
| `Availability.md` | ausgearbeitet |
| `Booking.md` | ausgearbeitet |
| `Charge.md` | ausgearbeitet |
| `Context.md` | ausgearbeitet |
| `Dashboard.md` | ausgearbeitet |
| `Document.md` | ausgearbeitet |
| `Facility.md` | ausgearbeitet |
| `Invoice.md` | ausgearbeitet |
| `Notification.md` | ausgearbeitet |
| `Organisation.md` | ausgearbeitet |
| `PortalUser.md` | ausgearbeitet |
| `Upload.md` | ausgearbeitet |
| `Wizard.md` | ausgearbeitet |
| `Workflow.md` | ausgearbeitet |

---

## 9 Offene fachliche Ergänzungen

| Thema | Einordnung |
|---|---|
| Reporting | aktuell nicht als eigene Fachdomäne ausgearbeitet; Dashboard deckt nur Startseitenaggregation ab |
| Audit / Logging | in mehreren Domänen referenziert; kann bei Bedarf als eigene Querschnittsdomäne ergänzt werden |
| Integration | SAP, ePayBL, Mail und sonstige externe Systeme können bei Bedarf als eigene technische Domäne ergänzt werden |
| Suche | Suchfunktionen sind derzeit in Facility, Availability, Booking, Document und Invoice domänenspezifisch beschrieben |

---

## 10 Änderungsauswirkungen

Änderungen an dieser Übersicht wirken sich aus auf:

- alle Dokumente in `03_Domaenen/`,
- `04_REST_API/Endpunkte.md`,
- `04_REST_API/DTOs.md`,
- `05_Datenmodell/Tabellen.md`,
- `05_Datenmodell/Packages.md`,
- `06_Arbeitspakete/Arbeitspaketliste.md`,
- `07_Kalkulation/Aufwandsschaetzung.md`,
- `09_Testkonzept/Testfaelle.md`,
- `12_Offene_Punkte/Offene_Punkte.md`.

---

## 11 Ergebnis

Die Domänenlandschaft für SportFM ist damit fachlich vollständig beschrieben.

Die zentralen Bestandsdomänen bleiben führend und werden über REST, DTOs und Portaloberflächen freigelegt.

Die Neuentwicklungsdomänen bilden die Portal-, Kontext-, Antrags-, Workflow-, Upload-, Benachrichtigungs- und Administrationsschicht.
