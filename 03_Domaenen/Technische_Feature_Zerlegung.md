# Technische Feature-Zerlegung

| Dokument | 03_Domaenen/Technische_Feature_Zerlegung.md |
|----------|----------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |
| Grundlage | Feature_Zerlegung.md, Feature_Matrix.md, Feature_Abhaengigkeiten.md, Feature_Traceability.md |

---

# 1 Ziel

Dieses Dokument beschreibt die technische Zerlegung der fachlichen Features in umsetzbare technische Bausteine.

Die Zerlegung bildet die direkte Grundlage für:

- Erstellung detaillierter Arbeitspakete
- Aufwandsschätzung
- Sprintplanung
- REST-API-Ausarbeitung
- Datenbank-/Migrationsplanung
- Testfallkatalog
- Projektcontrolling

---

# 2 Zerlegungsschema

Jedes Feature wird in technische Bausteine zerlegt.

| Baustein | Bedeutung |
|----------|-----------|
| FE | Frontend / Blazor / WPF-Oberfläche |
| API | REST-Endpunkt / Controller |
| BL | Businesslogik / Anwendungsschicht |
| DB | Datenmodell / Tabellen / Migration |
| VAL | Validierung / Plausibilisierung |
| BER | Berechtigung / Rollenprüfung |
| LOG | Logging / Audit / Nachvollziehbarkeit |
| SCH | Schnittstelle / Fremdsystem |
| TEST | Unit-, Integrations- und Abnahmetests |
| DOK | Dokumentation |

---

# 3 Arbeitspaket-Ableitung

Aus jedem Feature entstehen später Arbeitspakete nach folgendem Muster:

```text
FUN-ANT-001 Antrag erstellen
    ↓
AP-ANT-001-FE    Oberfläche Antrag erstellen
AP-ANT-001-API   REST-Endpunkt Antrag erstellen
AP-ANT-001-BL    Fachlogik Antrag erstellen
AP-ANT-001-DB    Tabellen / Migration
AP-ANT-001-VAL   Validierung
AP-ANT-001-BER   Berechtigungen
AP-ANT-001-LOG   Protokollierung
AP-ANT-001-TEST  Tests
AP-ANT-001-DOK   Dokumentation
```

---

# 4 Bewertungsfelder je Feature

| Feld | Beschreibung |
|------|--------------|
| Feature-ID | Eindeutige Feature-Kennung |
| Domäne | Zugehörige Domäne |
| Technische Bausteine | FE, API, BL, DB, VAL, BER, LOG, SCH, TEST, DOK |
| Abhängigkeiten | Fachliche und technische Vorgänger |
| Risiko | Niedrig / Mittel / Hoch |
| Komplexität | XS / S / M / L / XL / XXL |
| Hinweis | Besondere Umsetzungsaspekte |

---

# 5 Komplexitätsklassen

| Klasse | Richtwert |
|--------|-----------|
| XS | sehr klein |
| S | klein |
| M | mittel |
| L | groß |
| XL | sehr groß |
| XXL | strategisches Kernfeature / Framework |

Die Klassen sind noch keine Aufwandsschätzung. Sie dienen nur der Vorbereitung der Kalkulation.

---

# 6 Dokumentenstruktur

Die technische Zerlegung wird domänenweise geführt:

```text
03_Domaenen/Technische_Zerlegung/
├── 00_Uebersicht.md
├── 01_Benutzer.md
├── 02_Vereine.md
├── 03_Sportanlagen.md
├── 04_Reservierung.md
├── 05_Antrag.md
├── 06_Buchung.md
├── 07_Dokumente.md
├── 08_Veranstaltungen.md
├── 09_Portal.md
├── 10_Auswertungen.md
├── 11_Schnittstellen.md
└── 12_System.md
```

---

# 7 Nächster Schritt

Aus dieser technischen Feature-Zerlegung werden anschließend die finalen Arbeitspakete im Kapitel `06_Arbeitspakete` erzeugt.

Dabei wird jede Zeile dieses Dokuments in ein oder mehrere konkrete Arbeitspakete überführt.
