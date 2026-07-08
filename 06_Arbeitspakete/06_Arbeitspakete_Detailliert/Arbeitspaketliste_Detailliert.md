# Detaillierte Arbeitspakete

| Dokument | 06_Arbeitspakete/Arbeitspaketliste_Detailliert.md |
|----------|----------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |
| Grundlage | Feature-Matrix, Feature-Abhängigkeiten, Feature-Traceability, technische Feature-Zerlegung |

---

# 1 Ziel

Dieses Dokument überführt die technische Feature-Zerlegung in konkrete, kalkulierbare Arbeitspakete.

Die Arbeitspakete dienen als Grundlage für:

- Aufwandsschätzung
- Kalkulation
- Sprintplanung
- Projektcontrolling
- Abnahmeplanung
- Testplanung

---

# 2 Codierung

| Kürzel | Bedeutung |
|--------|-----------|
| AP | Arbeitspaket |
| BEN | Benutzer |
| VER | Vereine / Verbände |
| ANL | Sportanlagen |
| RES | Reservierung |
| ANT | Antrag / Wizard |
| BUC | Buchung |
| DOK | Dokumente |
| VERA | Veranstaltungen |
| POR | Portal |
| AUS | Auswertungen |
| SST | Schnittstellen |
| SYS | System / Plattform |

---

# 3 Arbeitspaket-Typen

| Typ | Bedeutung |
|-----|-----------|
| N | Neuentwicklung |
| E | Erweiterung bestehender Funktion |
| M | Migration |
| I | Integration |
| K | Konfiguration |
| T | Test / Qualitätssicherung |
| D | Dokumentation |

---

# 4 Statuswerte

| Status | Bedeutung |
|--------|-----------|
| Offen | noch nicht begonnen |
| In Arbeit | Umsetzung läuft |
| Fertig | umgesetzt und getestet |
| Zurückgestellt | fachlich oder technisch verschoben |
| Gesperrt | abhängig von offenem Vorgänger |

---

# 5 Domänenüberblick

| Datei | Inhalt |
|-------|--------|
| 01_AP_Benutzer.md | Benutzer, Rollen, Berechtigungen |
| 02_AP_Vereine.md | Vereine, Ansprechpartner, Vereinsdokumente |
| 03_AP_Sportanlagen.md | Sportanlagen, Sportflächen, Ausstattung, Karten |
| 04_AP_Reservierung.md | Reservierung, Konflikte, Prioritäten |
| 05_AP_Antrag_Wizard.md | Antragssystem und Wizard-Framework |
| 06_AP_Buchung.md | Buchung, Serien, Gebühren, Widerruf |
| 07_AP_Dokumente.md | Dokumente, Bescheide, PDF, Archiv |
| 08_AP_Veranstaltungen.md | Veranstaltungen, Sperrflächen, Kabinen |
| 09_AP_Portal.md | Portaloberflächen, Registrierung, Konto |
| 10_AP_Auswertungen.md | Berichte, Belegungsplan, Exporte |
| 11_AP_Schnittstellen.md | RIB FM, SAP, ePayBL, Themenstadtplan |
| 12_AP_System.md | REST-Grundsystem, Sicherheit, Logging, Betrieb |

---

# 6 Hinweis zur Kalkulation

Die hier enthaltenen Arbeitspakete enthalten noch keine Stundenwerte.

Die Bewertung erfolgt anschließend in `07_Kalkulation` über:

- Komplexität
- Risiko
- Entwicklungsaufwand
- Testaufwand
- Dokumentationsaufwand
- Puffer
