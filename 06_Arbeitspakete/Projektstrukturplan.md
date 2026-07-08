# Projektstrukturplan

## Dokumentinformationen

| Merkmal | Wert |
|----------|------|
| Dokument | Projektstrukturplan |
| Projekt | SportFM |
| Version | 2.0 |
| Status | Entwurf |

---

# Ziel

Der Projektstrukturplan (PSP) zerlegt das Projekt SportFM in fachlich zusammenhängende Teilprojekte. Die Struktur orientiert sich konsequent an den Business-Domänen des Systems und nicht an technischen Schichten.

Der PSP bildet die Grundlage für

- Pflichtenheft
- Architektur
- Arbeitspakete
- Aufwandsschätzung
- Terminplanung
- Projektcontrolling

Jeder Knoten des PSP besitzt später eine eindeutige Zuordnung zu Anforderungen, Arbeitspaketen und Testfällen.

---

# Strukturierungsprinzip

Das Projekt wird in vier Ebenen gegliedert.

```text
Projekt

    ↓

Business Domain

    ↓

Komponente / Asset

    ↓

Arbeitspaket
```

Dadurch entstehen keine Doppelentwicklungen und wiederverwendbare Komponenten können projektweit genutzt werden.

---

# Ebene 1 – Business Domains

```text
01 Projektgrundlagen

02 Plattform

03 REST API

04 Authentifizierung

05 Portal

06 Organisation

07 Antragssystem

08 Workflow

09 Buchung

10 Belegung

11 Dokumente

12 Rechnungen

13 Administration

14 Integration

15 Migration

16 Qualitätssicherung

17 Betrieb
```

---

# Ebene 2

## 01 Projektgrundlagen

```text
01.01 Analyse

01.02 Projektplanung

01.03 Pflichtenheft

01.04 Architektur

01.05 Projektsteuerung
```

---

## 02 Plattform

```text
02.01 Plattformbasis

02.02 Logging

02.03 Audit

02.04 Konfiguration

02.05 Referenzdaten
```

---

## 03 REST API

```text
03.01 REST-Grundsystem

03.02 DTOs

03.03 Fehlerbehandlung

03.04 Versionierung
```

---

## 04 Authentifizierung

```text
04.01 Registrierung

04.02 Anmeldung

04.03 Passwort

04.04 Rollen

04.05 Kontext
```

---

## 05 Portal

```text
05.01 Dashboard

05.02 Navigation

05.03 Suche

05.04 Benutzerprofil
```

---

## 06 Organisation

```text
06.01 Organisationseinheiten

06.02 Abteilungen

06.03 Benutzer

06.04 Mitgliedschaften
```

---

## 07 Antragssystem

```text
07.01 Antrag

07.02 Wizard

07.03 Upload

07.04 Arbeitskorb
```

---

## 08 Workflow

```text
08.01 Status

08.02 Aufgaben

08.03 Entscheidungen

08.04 Historie
```

---

## 09 Buchung

```text
09.01 Planung

09.02 Reservierung

09.03 Buchung

09.04 Serien
```

---

## 10 Belegung

```text
10.01 Kalender

10.02 Freie Zeiten

10.03 Kollisionsprüfung

10.04 Prioritätslogik
```

---

## 11 Dokumente

```text
11.01 Dokumentenengine

11.02 Dokumentvorlagen

11.03 PDF

11.04 Download
```

---

## 12 Rechnungen

```text
12.01 Gebühren

12.02 Rechnungen

12.03 SAP-Vorbereitung
```

---

## 13 Administration

```text
13.01 Benutzerverwaltung

13.02 Berechtigungen

13.03 Mailvorlagen
```

---

## 14 Integration

```text
14.01 SAP

14.02 Formcycle

14.03 Cardo

14.04 Themenstadtplan

14.05 Sportpark-App
```

---

## 15 Migration

```text
15.01 WPF

15.02 Datenmigration

15.03 Bestandsdaten
```

---

## 16 Qualitätssicherung

```text
16.01 Unit-Tests

16.02 Integrationstests

16.03 Sicherheitstests

16.04 Abnahmetests
```

---

## 17 Betrieb

```text
17.01 Deployment

17.02 Monitoring

17.03 Dokumentation

17.04 Betriebsübergabe
```

---

# Wiederverwendbare Assets

Der Snapshot identifiziert Komponenten, die domänenübergreifend mehrfach eingesetzt werden. Diese werden als eigenständige technische Assets behandelt und separat kalkuliert.

| Asset | Verwendet in |
|---------|--------------|
| Wizard | Antragssystem |
| Workflow | Antragssystem, Buchung |
| Dokumentenengine | Dokumente, Rechnungen |
| Authentifizierung | Portal, REST |
| Logging | gesamtes System |
| Dashboard | Portal |
| Reporting | Portal, Administration |
| Upload-Komponente | Antragssystem, Dokumente |
| Kalender-Komponente | Belegung, Buchung |

Die erstmalige Entwicklung dieser Assets verursacht den Hauptaufwand. Weitere Funktionen können diese Komponenten wiederverwenden und dadurch mit deutlich geringerem Aufwand umgesetzt werden.

---

# Zusammenhang mit den Arbeitspaketen

Aus jeder Business Domain entstehen die zugehörigen Arbeitspakete.

Beispiel:

```text
07 Antragssystem

    ↓

Wizard

    ↓

AP-008 Wizard V1

    ↓

AP-008.001 Wizard Framework

AP-008.002 JSON-Konfiguration

AP-008.003 Validierungen

AP-008.004 Upload

AP-008.005 Übergabe SportFM

AP-008.006 Tests
```

Diese vierstufige Struktur bildet die Grundlage für die detaillierte Arbeitspaketplanung, die Aufwandsschätzung und die vollständige Rückverfolgbarkeit zwischen Anforderungen, Implementierung und Testfällen.