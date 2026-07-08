# Arbeitspaketliste

## Dokumentinformationen

| Merkmal | Wert |
|----------|------|
| Dokument | Arbeitspaketliste |
| Projekt | SportFM |
| Version | 1.0 |
| Status | Entwurf |

---

# Ziel

Die Arbeitspaketliste beschreibt den vollständigen Projektumfang der Version V1.

Sie bildet die Grundlage für

- Aufwandsschätzung
- Terminplanung
- Ressourcenplanung
- Projektcontrolling

Die Liste enthält ausschließlich Arbeitspakete der Version V1. Erweiterungen für V2 und V3 werden getrennt betrachtet und nicht Bestandteil der Hauptkalkulation.

---

# Grundsätze

Die Arbeitspakete

- orientieren sich an den Business-Domänen,
- vermeiden Doppelentwicklungen,
- berücksichtigen wiederverwendbare Komponenten,
- bilden die Grundlage der späteren Stundenkalkulation.

---

# V1-Arbeitspakete

| AP | Bereich | Inhalt |
|----|----------|---------|
| AP-001 | Projektgrundlagen | Pflichtenheft, Architektur, Planung |
| AP-002 | Plattformbasis | Solution, Konfiguration, Logging |
| AP-003 | REST-Grundsystem | Versionierung, DTOs, Fehlerformat |
| AP-004 | Authentication | Registrierung, Login, Passwort, 2FA |
| AP-005 | Context / Authorization | aktiver Kontext, Rollenprüfung |
| AP-006 | Organisation | Organisationseinheiten, Abteilungen, Mitgliedschaften |
| AP-007 | Application | Antrag, Entwurf, Einreichen |
| AP-008 | Wizard V1 | JSON, Felder, Dropdowns, Validierung |
| AP-009 | Workflow | Status, Aufgaben, Historie |
| AP-010 | Booking REST | Buchungen lesen |
| AP-011 | Availability | freie Zeiten |
| AP-012 | Calendar | Kalenderdaten |
| AP-013 | Document | Dokumentliste, Download |
| AP-014 | Invoice | Rechnungsliste, Details |
| AP-015 | Notification / Mail | Portalnachrichten, Mailqueue |
| AP-016 | Blazor UI | Benutzeroberfläche |
| AP-017 | Administration | Freigaben, Benutzer, Mailvorlagen |
| AP-018 | Audit / Logging | fachliches Audit, technische Logs |
| AP-019 | WPF-Migration | Vorbereitung / erste lesende Migration |
| AP-020 | Tests | Unit-, REST-, Portal- und Sicherheitstests |
| AP-021 | Betrieb | Deployment, Monitoring, Dokumentation |
| AP-024 | ReferenceData | Auswahldaten für Portal und Wizard |
| AP-025 | Upload | Dateien, Anlagen, Download |
| AP-027 | Arbeitskorb | Sachbearbeitung |
| AP-028 | Entscheidungen | Genehmigen, Ablehnen, Rückfragen |

---

# Wiederverwendbare Komponenten

Folgende Komponenten werden projektweit mehrfach verwendet und daher als gemeinsame technische Basis entwickelt.

| Komponente | Verwendung |
|------------|------------|
| REST-Grundsystem | alle REST-Endpunkte |
| Authentication | Portal und REST |
| Wizard V1 | digitale Anträge |
| Workflow | Anträge und Entscheidungen |
| ReferenceData | Auswahldaten |
| Upload | Dokumente und Anlagen |
| Audit / Logging | gesamtes System |

---

# Kalkulationsrelevante Festlegung

Für die Aufwandsschätzung wird ausschließlich der Umfang der Version V1 berücksichtigt.

Vorbereitungen für spätere Versionen werden dokumentiert, jedoch nicht Bestandteil der Hauptkalkulation.

Die eigentliche Stundenkalkulation erfolgt im Kapitel **07_Kalkulation** auf Basis dieser Arbeitspaketliste.