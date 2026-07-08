# Arbeitspakete Übersicht

## Dokumentinformationen

| Merkmal | Wert |
|----------|------|
| Dokument | Arbeitspakete Übersicht |
| Projekt | SportFM |
| Version | 1.0 |
| Status | Entwurf |

---

# Ziel

Dieses Dokument fasst sämtliche Arbeitspakete der Version V1 in einer kompakten Übersicht zusammen.

Im Gegensatz zur **Arbeitspaketliste** dient dieses Dokument nicht der detaillierten Beschreibung einzelner Arbeitspakete, sondern der Einordnung des Projektumfangs nach fachlichen Komponenten. Es bildet die zentrale Übersicht für Projektleitung, Aufwandsschätzung und Projektcontrolling.

Die Zusammenstellung orientiert sich an der im Snapshot konsolidierten Komponentenstruktur und vermeidet bewusst redundante Informationen. :contentReference[oaicite:0]{index=0}

---

# Übersicht V1

| AP | Komponente | Oracle | REST | Blazor UI | Status |
|----|------------|:------:|:----:|:---------:|:------:|
| AP-001 | Projektgrundlagen | – | – | – | Neu |
| AP-002 | Plattformbasis | – | ✔ | – | Neu |
| AP-003 | REST-Grundsystem | – | ✔ | – | Neu |
| AP-004 | Authentication | ✔ | ✔ | ✔ | Neu |
| AP-005 | Context / Authorization | ✔ | ✔ | ✔ | Neu |
| AP-006 | Organisation | ✔ | ✔ | ✔ | Neu |
| AP-007 | Application | ✔ | ✔ | ✔ | Neu |
| AP-008 | Wizard V1 | ✔ | ✔ | ✔ | Neu |
| AP-009 | Workflow | ✔ | ✔ | ✔ | Neu |
| AP-010 | Booking REST | ✔ | ✔ | – | Erweiterung |
| AP-011 | Availability | ✔ | ✔ | ✔ | Erweiterung |
| AP-012 | Calendar | ✔ | ✔ | ✔ | Erweiterung |
| AP-013 | Document | ✔ | ✔ | ✔ | Erweiterung |
| AP-014 | Invoice | ✔ | ✔ | ✔ | Erweiterung |
| AP-015 | Notification / Mail | ✔ | ✔ | ✔ | Neu |
| AP-016 | Blazor UI | – | ✔ | ✔ | Neu |
| AP-017 | Administration | ✔ | ✔ | ✔ | Neu |
| AP-018 | Audit / Logging | ✔ | ✔ | – | Neu |
| AP-019 | WPF-Migration | ✔ | ✔ | – | Erweiterung |
| AP-020 | Tests | ✔ | ✔ | ✔ | Neu |
| AP-021 | Betrieb | ✔ | ✔ | ✔ | Neu |
| AP-024 | ReferenceData | ✔ | ✔ | – | Erweiterung |
| AP-025 | Upload | ✔ | ✔ | ✔ | Neu |
| AP-027 | Arbeitskorb | ✔ | ✔ | ✔ | Neu |
| AP-028 | Entscheidungen | ✔ | ✔ | ✔ | Neu |

---

# Gruppierung nach Komponenten

## Plattform

| AP | Komponente |
|----|------------|
| AP-001 | Projektgrundlagen |
| AP-002 | Plattformbasis |
| AP-003 | REST-Grundsystem |
| AP-018 | Audit / Logging |

---

## Sicherheit

| AP | Komponente |
|----|------------|
| AP-004 | Authentication |
| AP-005 | Context / Authorization |

---

## Organisationsverwaltung

| AP | Komponente |
|----|------------|
| AP-006 | Organisation |
| AP-017 | Administration |

---

## Antragssystem

| AP | Komponente |
|----|------------|
| AP-007 | Application |
| AP-008 | Wizard V1 |
| AP-009 | Workflow |
| AP-027 | Arbeitskorb |
| AP-028 | Entscheidungen |

---

## Buchung

| AP | Komponente |
|----|------------|
| AP-010 | Booking REST |
| AP-011 | Availability |
| AP-012 | Calendar |

---

## Dokumente und Kommunikation

| AP | Komponente |
|----|------------|
| AP-013 | Document |
| AP-014 | Invoice |
| AP-015 | Notification / Mail |
| AP-025 | Upload |

---

## Benutzeroberfläche

| AP | Komponente |
|----|------------|
| AP-016 | Blazor UI |

---

## Integration und Migration

| AP | Komponente |
|----|------------|
| AP-019 | WPF-Migration |
| AP-024 | ReferenceData |

---

## Qualität und Betrieb

| AP | Komponente |
|----|------------|
| AP-020 | Tests |
| AP-021 | Betrieb |

---

# Komponentenübersicht

Die Snapshot-Analyse hat gezeigt, dass sich das Projekt auf wenige zentrale, wiederverwendbare Komponenten konzentriert. Diese Komponenten bilden die Grundlage für die spätere Aufwandsschätzung und verhindern redundante Entwicklungen. :contentReference[oaicite:1]{index=1}

| Komponente | Beschreibung |
|------------|--------------|
| Plattformbasis | Technische Basis der Anwendung |
| REST-Grundsystem | Gemeinsame Infrastruktur aller REST-Endpunkte |
| Authentication | Anmeldung, Registrierung und 2FA |
| Context | Aktiver SportFM-Kontext und Berechtigungen |
| Organisation | Verwaltung von Organisationseinheiten und Mitgliedschaften |
| Application | Digitale Antragserfassung |
| Wizard V1 | Konfigurierbare Antragsführung |
| Workflow | Bearbeitungs- und Entscheidungsprozesse |
| Booking | Zugriff auf bestehende Buchungslogik |
| Availability | Ermittlung freier Belegungszeiten |
| Calendar | Kalender- und Belegungsansichten |
| Document | Dokumentenverwaltung |
| Invoice | Rechnungsübersicht |
| Notification | Benachrichtigungen und Mailversand |
| Upload | Verwaltung von Dateianhängen |
| Blazor UI | Benutzeroberfläche des Portals |
| Administration | Verwaltungsfunktionen |
| Audit / Logging | Fachliche und technische Protokollierung |
| ReferenceData | Bereitstellung von Auswahldaten |

---

# Verwendung

Die Arbeitspakete Übersicht dient als Einstieg in die Projektplanung.

Die Detailinformationen befinden sich in:

- **Projektstrukturplan.md**
- **Arbeitspaketliste.md**
- **Abhaengigkeiten.md**

Die eigentliche Aufwandsschätzung erfolgt anschließend im Kapitel **07_Kalkulation** auf Basis dieser konsolidierten Übersicht.