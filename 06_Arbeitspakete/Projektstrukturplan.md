# Projektstrukturplan

## Ziel

Der Projektstrukturplan (PSP) zerlegt das Projekt SportFM in fachlich und technisch zusammenhängende Teilprojekte. Er bildet die Grundlage für Arbeitspakete, Aufwandsschätzung, Terminplanung und Projektcontrolling.

Die Struktur orientiert sich an den Business-Domänen, den wiederverwendbaren technischen Assets sowie den Projektzielen des Lastenhefts.

---

# Ebene 1

```text
01 Projektgrundlagen
02 Plattform
03 REST API
04 Authentifizierung
05 Organisation
06 Antragssystem
07 Buchung
08 Dokumente
09 Rechnungen
10 Portal
11 Administration
12 Integration
13 Migration
14 Qualitätssicherung
15 Betrieb
```

---

# Ebene 2

## 01 Projektgrundlagen

```text
01.01 Analyse
01.02 Pflichtenheft
01.03 Architektur
01.04 Projektmanagement
```

## 02 Plattform

```text
02.01 Plattformbasis
02.02 Logging
02.03 Audit
02.04 Konfiguration
```

## 03 REST API

```text
03.01 REST-Grundsystem
03.02 DTOs
03.03 Fehlerbehandlung
03.04 Versionierung
```

## 04 Authentifizierung

```text
04.01 Anmeldung
04.02 Passwort
04.03 Rollen
04.04 Kontext
```

## 05 Organisation

```text
05.01 Organisationseinheiten
05.02 Benutzer
05.03 Mitgliedschaften
```

## 06 Antragssystem

```text
06.01 Antrag
06.02 Wizard
06.03 Workflow
06.04 Arbeitskorb
06.05 Entscheidungen
```

## 07 Buchung

```text
07.01 Buchung
07.02 Freie Zeiten
07.03 Kalender
```

## 08 Dokumente

```text
08.01 Dokumentenengine
08.02 Upload
08.03 Download
```

## 09 Rechnungen

```text
09.01 Gebühren
09.02 Rechnungen
09.03 Zahlungsvorbereitung
```

## 10 Portal

```text
10.01 Blazor UI
10.02 Dashboard
10.03 Suche
```

## 11 Administration

```text
11.01 Benutzerverwaltung
11.02 Rechte
11.03 Referenzdaten
```

## 12 Integration

```text
12.01 SAP
12.02 Formcycle
12.03 Cardo
12.04 Themenstadtplan
12.05 LSBS
```

## 13 Migration

```text
13.01 WPF
13.02 Datenmigration
```

## 14 Qualitätssicherung

```text
14.01 Unit-Tests
14.02 Integrationstests
14.03 Abnahmetests
14.04 Sicherheitstests
```

## 15 Betrieb

```text
15.01 Deployment
15.02 Monitoring
15.03 Dokumentation
```

---

# Grundsätze

- Zerlegung nach fachlichen Verantwortungsbereichen.
- Wiederverwendbare technische Assets werden eigenständig geplant.
- Die Arbeitspakete werden den PSP-Elementen eindeutig zugeordnet.
- Die Aufwandsschätzung erfolgt ausschließlich auf Ebene der Arbeitspakete.