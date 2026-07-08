# Abhängigkeiten der Arbeitspakete

## Dokumentinformationen

| Merkmal | Wert |
|---|---|
| Dokument | Abhängigkeiten der Arbeitspakete |
| Projekt | SportFM |
| Version | 1.0 |
| Status | Entwurf |

---

# Ziel

Dieses Dokument beschreibt die fachlichen und technischen Abhängigkeiten der V1-Arbeitspakete.

Die Abhängigkeiten dienen als Grundlage für:

- Umsetzungsreihenfolge
- Projektplan
- kritischen Pfad
- Aufwandsschätzung
- Projektcontrolling

Die Abhängigkeiten beziehen sich ausschließlich auf den V1-Umfang.

---

# Grundlogik

Die Umsetzung folgt der im Snapshot festgelegten Schichtenfolge:

```text
Blazor UI
   ↓
REST API
   ↓
SportFM / Oracle / PL/SQL
```

Daraus ergeben sich drei zentrale Abhängigkeitsregeln:

1. Fachliche Portal-Funktionen benötigen vorher REST-Grundsystem, Authentifizierung und Kontext.
2. Antrag, Wizard, Workflow, Upload und Arbeitskorb müssen vor Entscheidungen und SportFM-Übergabe stehen.
3. Tests und Betrieb folgen erst nach den fachlichen Arbeitspaketen.

---

# Abhängigkeiten V1

| AP | Arbeitspaket | Abhängig von | Begründung |
|---|---|---|---|
| AP-001 | Projektgrundlagen | – | Startpunkt für Pflichtenheft, Architektur und Planung |
| AP-002 | Plattformbasis | AP-001 | Solution, Konfiguration und Logging benötigen Projektgrundlagen |
| AP-003 | REST-Grundsystem | AP-002 | REST-Versionierung, DTOs und Fehlerformat bauen auf der Plattformbasis auf |
| AP-004 | Authentifizierung | AP-003 | Registrierung, Login, Passwort und 2FA benötigen REST-Grundsystem |
| AP-005 | Kontext / Berechtigung | AP-004 | Rollenprüfung setzt authentifizierte Benutzer voraus |
| AP-006 | Organisation | AP-005 | Organisationen, Abteilungen und Mitgliedschaften benötigen Kontext- und Rechteprüfung |
| AP-024 | Referenzdaten | AP-003 | Auswahldaten für Portal und Wizard werden über REST bereitgestellt |
| AP-025 | Upload | AP-003, AP-004, AP-005 | Upload benötigt REST, Benutzeridentität und Berechtigungen |
| AP-008 | Wizard V1 | AP-003, AP-024 | Wizard benötigt REST-Grundsystem und Referenzdaten für Auswahlfelder |
| AP-009 | Workflow | AP-002, AP-005 | Status, Aufgaben und Historie benötigen Plattformbasis und Berechtigungen |
| AP-007 | Antrag | AP-006, AP-008, AP-009, AP-024, AP-025 | Antrag nutzt Organisation, Wizard, Workflow, Referenzdaten und Upload |
| AP-027 | Arbeitskorb | AP-007, AP-009, AP-005 | Sachbearbeitung benötigt Antrag, Workflow und Berechtigung |
| AP-028 | Entscheidungen | AP-027, AP-009 | Genehmigen, Ablehnen und Rückfragen erfolgen aus dem Arbeitskorb heraus |
| AP-034 | Einreichen | AP-007, AP-008, AP-009, AP-025 | Einreichen benötigt Antrag, Wizard, Workflow und Anlagen |
| AP-035 | Rückfragen | AP-007, AP-009, AP-015, AP-025 | Rückfragen benötigen Antrag, Workflow, Benachrichtigung und ggf. Nachreichungen |
| AP-036 | Historie | AP-007, AP-018 | Antragshistorie benötigt Antrag und fachliches Audit |
| AP-037 | Folgeaktionen | AP-009, AP-013, AP-015, AP-018 | Folgeaktionen erzeugen Dokumente, Benachrichtigungen und Audit-Einträge |
| AP-010 | Booking REST | AP-003 | Lesender Zugriff auf Buchungen benötigt REST-Grundsystem |
| AP-011 | Availability | AP-010 | Freie Zeiten bauen auf vorhandener Buchungs- und Occurrence-Logik auf |
| AP-012 | Calendar | AP-010, AP-011 | Kalender nutzt Buchungen und Verfügbarkeits-/Belegungsdaten |
| AP-013 | Document | AP-003, AP-005, AP-018 | Dokumentliste und Download benötigen REST, Berechtigung und Audit |
| AP-014 | Invoice | AP-003, AP-005, AP-013 | Rechnungsliste benötigt REST, Berechtigung und Dokumentbezug |
| AP-015 | Benachrichtigung / Mail | AP-002, AP-009 | Portalnachrichten und Mailqueue hängen an Plattformbasis und Workflow-Ereignissen |
| AP-018 | Audit / Logging | AP-002, AP-003 | Fachliches Audit und technische Logs bauen auf Plattform und REST auf |
| AP-016 | Blazor UI | AP-003, AP-004, AP-005, AP-006, AP-007, AP-011, AP-012, AP-013, AP-014, AP-015 | Oberfläche benötigt die fachlichen REST-Funktionen |
| AP-017 | Administration | AP-004, AP-005, AP-006, AP-015, AP-024 | Freigaben, Benutzer, Mailvorlagen und Referenzdaten benötigen Auth, Rechte und Organisation |
| AP-029 | Integration | AP-003, AP-010, AP-030 | Integration benötigt REST, Buchungszugriff und Mapping |
| AP-030 | Mapping | AP-006, AP-007, AP-010, AP-024 | Mapping verbindet Organisation, Antrag, Buchung und Referenzdaten |
| AP-038 | SportFM-Übergabe | AP-007, AP-009, AP-010, AP-028, AP-030, AP-034 | Übergabe genehmigter Anträge benötigt Antrag, Workflow, Entscheidung, Mapping und Einreichen |
| AP-019 | WPF-Migration Einstieg | AP-003, AP-010, AP-011, AP-012, AP-013, AP-014 | Erste lesende Migration benötigt die lesenden REST-Funktionen |
| AP-020 | Tests | AP-004 bis AP-019, AP-024, AP-025, AP-027, AP-028, AP-029, AP-030, AP-034 bis AP-038 | Tests erfolgen gegen die fachlich fertiggestellten V1-Komponenten |
| AP-021 | Betrieb | AP-002, AP-003, AP-020 | Deployment, Monitoring und Betriebsdokumentation folgen nach Plattform, REST und Tests |

---

# Kritischer Pfad V1

Der fachliche Hauptpfad für V1 ist:

```text
AP-001 Projektgrundlagen
   ↓
AP-002 Plattformbasis
   ↓
AP-003 REST-Grundsystem
   ↓
AP-004 Authentifizierung
   ↓
AP-005 Kontext / Berechtigung
   ↓
AP-006 Organisation
   ↓
AP-008 Wizard V1
   ↓
AP-009 Workflow
   ↓
AP-007 Antrag
   ↓
AP-034 Einreichen
   ↓
AP-027 Arbeitskorb
   ↓
AP-028 Entscheidungen
   ↓
AP-030 Mapping
   ↓
AP-038 SportFM-Übergabe
   ↓
AP-020 Tests
   ↓
AP-021 Betrieb
```

---

# Parallelisierbare Arbeitspakete

Folgende Arbeitspakete können nach Abschluss von Plattformbasis und REST-Grundsystem teilweise parallel laufen:

| Arbeitspaket | Parallel möglich mit |
|---|---|
| AP-008 Wizard V1 | AP-009 Workflow, AP-024 Referenzdaten, AP-025 Upload |
| AP-010 Booking REST | AP-013 Document, AP-014 Invoice |
| AP-011 Availability | AP-012 Calendar |
| AP-015 Benachrichtigung / Mail | AP-017 Administration |
| AP-018 Audit / Logging | nahezu alle fachlichen AP |
| AP-016 Blazor UI | nach Vorliegen stabiler REST-Endpunkte schrittweise |

---

# Entfernte oder zusammengeführte Arbeitspakete

Folgende Arbeitspakete werden nicht separat weitergeführt, weil sie im Snapshot bereits bereinigt bzw. zusammengeführt wurden:

| Entfernt / zusammengeführt | Ziel |
|---|---|
| AP-022 Projektdokumentation | AP-001 / AP-021 |
| AP-023 Integration allgemein | AP-029 Integration |
| AP-026 Application → Workflow | AP-034 / AP-009 |
| doppelte Wizard-Arbeitspakete | AP-008 |
| doppelte Benachrichtigungs-Arbeitspakete | AP-015 |

---

# Hinweise für die Projektplanung

- AP-001 bis AP-005 sind zwingende Startvoraussetzungen.
- AP-007, AP-008 und AP-009 bilden den Kern des Antragssystems.
- AP-027, AP-028, AP-034, AP-035, AP-036, AP-037 und AP-038 bilden den fachlichen Bearbeitungsfluss.
- AP-010 bis AP-014 stellen bestehende SportFM-Fachlogik über REST bereit.
- AP-020 und AP-021 dürfen nicht zu früh gestartet werden; sie hängen vom stabilen V1-Funktionsumfang ab.
