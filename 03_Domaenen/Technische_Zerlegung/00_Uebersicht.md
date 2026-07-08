# Übersicht Technische Zerlegung

| Dokument | 03_Domaenen/Technische_Zerlegung/00_Uebersicht.md |
|----------|---------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Zweck

Diese Übersicht fasst die technische Zerlegung aller Features zusammen. Die detaillierte Beschreibung erfolgt je Domäne in separaten Dateien.

---

# 2 Domänen

| Nr. | Datei | Domäne |
|----:|-------|--------|
| 01 | 01_Benutzer.md | Benutzer |
| 02 | 02_Vereine.md | Vereine / Verbände |
| 03 | 03_Sportanlagen.md | Sportanlagen |
| 04 | 04_Reservierung.md | Belegung / Reservierung |
| 05 | 05_Antrag.md | Antrag / Wizard |
| 06 | 06_Buchung.md | Buchung |
| 07 | 07_Dokumente.md | Dokumente |
| 08 | 08_Veranstaltungen.md | Veranstaltungen |
| 09 | 09_Portal.md | Onlineportal |
| 10 | 10_Auswertungen.md | Auswertungen |
| 11 | 11_Schnittstellen.md | Schnittstellen |
| 12 | 12_System.md | System / Plattform |

---

# 3 Standard-Zerlegung

| Baustein | Beschreibung |
|----------|--------------|
| FE | Oberfläche |
| API | REST-Endpunkt |
| BL | Businesslogik |
| DB | Datenmodell |
| VAL | Validierung |
| BER | Berechtigung |
| LOG | Protokollierung |
| SCH | Schnittstelle |
| TEST | Tests |
| DOK | Dokumentation |

---

# 4 Kritische Kernbereiche

| Bereich | Begründung |
|---------|------------|
| Antrag / Wizard | strategisches Framework |
| Workflow | Grundlage für Antragsbearbeitung |
| Reservierung | zentrale Planungslogik |
| Buchung | Kernlogik SportFM |
| Occurrence / Winner | performante Belegungslogik |
| Dokumente | Bescheide, PDFs, Portalbereitstellung |
| Schnittstellen | SAP, ePayBL, Themenstadtplan, Formcycle |
