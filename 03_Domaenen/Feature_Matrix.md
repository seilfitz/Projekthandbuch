# Feature-Matrix

| Dokument | 03_Domaenen/Feature_Matrix.md |
|-----------|-------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |
| Grundlage | Lastenheft SportFM, Domänenmodell, Datenmodell, REST-API, Arbeitspakete |

---

# 1 Ziel

Die Feature-Matrix bildet die vollständige funktionale Abdeckung aller Domänen des Systems SportFM ab.

Sie dient als Bindeglied zwischen

- Anforderungen
- Domänenmodell
- Datenmodell
- REST-API
- Arbeitspaketen
- Testfällen
- Aufwandsschätzung.

Jedes Feature besitzt eine eindeutige ID und kann später direkt einem Arbeitspaket sowie einem Testfall zugeordnet werden.

---

# 2 Feature-Klassifizierung

| Kategorie | Beschreibung |
|------------|--------------|
| Stammdaten | Verwaltung fachlicher Grunddaten |
| Fachprozess | Unterstützung eines Geschäftsprozesses |
| Workflow | Status- und Prozesssteuerung |
| Dokument | Dokumentenerzeugung und Dokumentenverwaltung |
| Suche | Recherche- und Filterfunktionen |
| Auswertung | Listen, Statistiken und Berichte |
| Schnittstelle | Datenaustausch mit Fremdsystemen |
| Administration | Konfiguration und Systemverwaltung |

---

# 3 Feature-Matrix

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-BEN-001 | Benutzer | Benutzer anlegen | Stammdaten | Muss |
| FUN-BEN-002 | Benutzer | Benutzer bearbeiten | Stammdaten | Muss |
| FUN-BEN-003 | Benutzer | Benutzer deaktivieren | Stammdaten | Muss |
| FUN-BEN-004 | Benutzer | Rollen verwalten | Administration | Muss |
| FUN-BEN-005 | Benutzer | Berechtigungen verwalten | Administration | Muss |
| FUN-BEN-006 | Benutzer | Passwort zurücksetzen | Administration | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-VER-001 | Vereine | Verein anlegen | Stammdaten | Muss |
| FUN-VER-002 | Vereine | Verein bearbeiten | Stammdaten | Muss |
| FUN-VER-003 | Vereine | Ansprechpartner verwalten | Stammdaten | Muss |
| FUN-VER-004 | Vereine | Mitgliedschaften verwalten | Stammdaten | Soll |
| FUN-VER-005 | Vereine | Vereinsdokumente verwalten | Dokument | Soll |
| FUN-VER-006 | Vereine | Vereinssuche | Suche | Muss |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-ANL-001 | Sportanlagen | Sportanlage anlegen | Stammdaten | Muss |
| FUN-ANL-002 | Sportanlagen | Sportanlage bearbeiten | Stammdaten | Muss |
| FUN-ANL-003 | Sportanlagen | Sportflächen verwalten | Stammdaten | Muss |
| FUN-ANL-004 | Sportanlagen | Ausstattung verwalten | Stammdaten | Soll |
| FUN-ANL-005 | Sportanlagen | Bilder verwalten | Dokument | Soll |
| FUN-ANL-006 | Sportanlagen | Kartenposition verwalten | Schnittstelle | Muss |
| FUN-ANL-007 | Sportanlagen | Verfügbarkeiten anzeigen | Suche | Muss |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-RES-001 | Reservierung | Reservierung anlegen | Fachprozess | Muss |
| FUN-RES-002 | Reservierung | Reservierung ändern | Fachprozess | Muss |
| FUN-RES-003 | Reservierung | Reservierung löschen | Fachprozess | Muss |
| FUN-RES-004 | Reservierung | Reservierung prüfen | Workflow | Muss |
| FUN-RES-005 | Reservierung | Konfliktprüfung | Workflow | Muss |
| FUN-RES-006 | Reservierung | Sperrzeiten berücksichtigen | Workflow | Muss |
| FUN-RES-007 | Reservierung | Saisonlogik | Workflow | Muss |
| FUN-RES-008 | Reservierung | Prioritätsprüfung | Workflow | Muss |
| FUN-RES-009 | Reservierung | Warteliste | Workflow | Soll |
| FUN-RES-010 | Reservierung | Reservierungshistorie | Dokument | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-ANT-001 | Antrag | Antrag erstellen | Fachprozess | Muss |
| FUN-ANT-002 | Antrag | Antrag bearbeiten | Fachprozess | Muss |
| FUN-ANT-003 | Antrag | Antrag validieren | Workflow | Muss |
| FUN-ANT-004 | Antrag | Anhänge hochladen | Dokument | Muss |
| FUN-ANT-005 | Antrag | Statuswechsel | Workflow | Muss |
| FUN-ANT-006 | Antrag | Antrag archivieren | Dokument | Soll |
| FUN-ANT-007 | Antrag | Historie | Dokument | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-BUC-001 | Buchung | Buchung erstellen | Fachprozess | Muss |
| FUN-BUC-002 | Buchung | Buchung ändern | Fachprozess | Muss |
| FUN-BUC-003 | Buchung | Serienbuchung | Fachprozess | Muss |
| FUN-BUC-004 | Buchung | Gebühren berechnen | Fachprozess | Muss |
| FUN-BUC-005 | Buchung | Gebührenbefreiung | Fachprozess | Soll |
| FUN-BUC-006 | Buchung | Widerruf | Workflow | Muss |
| FUN-BUC-007 | Buchung | Umbuchung | Workflow | Soll |
| FUN-BUC-008 | Buchung | Buchungshistorie | Dokument | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-DOK-001 | Dokumente | Bescheid erzeugen | Dokument | Muss |
| FUN-DOK-002 | Dokumente | Gebührenbescheid | Dokument | Muss |
| FUN-DOK-003 | Dokumente | Widerrufsbescheid | Dokument | Muss |
| FUN-DOK-004 | Dokumente | PDF erzeugen | Dokument | Muss |
| FUN-DOK-005 | Dokumente | Dokument archivieren | Dokument | Muss |
| FUN-DOK-006 | Dokumente | Dokument suchen | Suche | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-VERA-001 | Veranstaltungen | Veranstaltung anlegen | Fachprozess | Muss |
| FUN-VERA-002 | Veranstaltungen | Veranstaltungsplanung | Fachprozess | Muss |
| FUN-VERA-003 | Veranstaltungen | Sperrflächen erzeugen | Workflow | Muss |
| FUN-VERA-004 | Veranstaltungen | Kabinenplanung | Workflow | Muss |
| FUN-VERA-005 | Veranstaltungen | Veranstaltungslisten | Auswertung | Soll |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-POR-001 | Portal | Registrierung | Fachprozess | Muss |
| FUN-POR-002 | Portal | Anmeldung | Fachprozess | Muss |
| FUN-POR-003 | Portal | Freie Zeiten suchen | Suche | Muss |
| FUN-POR-004 | Portal | Sportanlagen anzeigen | Suche | Muss |
| FUN-POR-005 | Portal | Vereinsdaten anzeigen | Suche | Soll |
| FUN-POR-006 | Portal | Veranstaltungen anzeigen | Suche | Soll |
| FUN-POR-007 | Portal | Online-Antrag | Fachprozess | Muss |
| FUN-POR-008 | Portal | Benutzerkonto | Stammdaten | Muss |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-AUS-001 | Auswertungen | Belegungsplan | Auswertung | Muss |
| FUN-AUS-002 | Auswertungen | Tagesbelegung | Auswertung | Muss |
| FUN-AUS-003 | Auswertungen | Wochenbelegung | Auswertung | Muss |
| FUN-AUS-004 | Auswertungen | Monatsübersicht | Auswertung | Soll |
| FUN-AUS-005 | Auswertungen | Gebührenübersicht | Auswertung | Soll |
| FUN-AUS-006 | Auswertungen | Export Excel | Auswertung | Muss |
| FUN-AUS-007 | Auswertungen | Export PDF | Auswertung | Muss |

---

| ID | Domäne | Feature | Kategorie | Priorität |
|----|---------|----------|-----------|-----------|
| FUN-SST-001 | Schnittstellen | RIB FM | Schnittstelle | Muss |
| FUN-SST-002 | Schnittstellen | Themenstadtplan | Schnittstelle | Muss |
| FUN-SST-003 | Schnittstellen | SAP | Schnittstelle | Muss |
| FUN-SST-004 | Schnittstellen | ePayBL | Schnittstelle | Muss |
| FUN-SST-005 | Schnittstellen | LSBS | Schnittstelle | Soll |
| FUN-SST-006 | Schnittstellen | Formcycle | Schnittstelle | Muss |
| FUN-SST-007 | Schnittstellen | Cardo | Schnittstelle | Soll |
| FUN-SST-008 | Schnittstellen | Schuldatenbank | Schnittstelle | Soll |

---

# 4 Abdeckung der Projektartefakte

| Artefakt | Verknüpfung |
|-----------|-------------|
| Anforderungen | ANF-xxx |
| Domänen | DOM-xxx |
| Features | FUN-xxx |
| REST-API | API-xxx |
| Datenmodell | TAB-xxx |
| Arbeitspakete | AP-xxx |
| Testfälle | TF-xxx |

---

# 5 Verwendung

Die Feature-Matrix dient als zentrale Referenz für die weitere Projektplanung.

Sie bildet die Grundlage für

- die Zerlegung der Features in implementierbare Arbeitspakete,
- die Ableitung der REST-Endpunkte,
- die Zuordnung zu Datenbanktabellen,
- die Erstellung der Testfälle,
- die Aufwandsschätzung sowie
- die spätere Fortschrittskontrolle während der Umsetzung.

Jedes Feature muss eindeutig mindestens einer Domäne, einem Arbeitspaket und einem Testfall zugeordnet werden.