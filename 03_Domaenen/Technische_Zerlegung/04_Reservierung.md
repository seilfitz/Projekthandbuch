# Technische Zerlegung – Reservierung

| Dokument | 03_Domaenen/Technische_Zerlegung/04_Reservierung.md |
|----------|------------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Reservierung bildet die interne Planungsebene zwischen Antrag und verbindlicher Buchung ab. Reservierungen sind nicht abrechnungsrelevant und dienen der fachlichen Vorprüfung, Planung und Kollisionsbewertung.

---

# 2 Features

## FUN-RES-001 Reservierung anlegen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Reservierungsmaske / Arbeitskorb |
| API | POST `/api/reservierungen` |
| BL | Reservierung aus Antrag oder manuell erzeugen |
| DB | Reservierung, Zeitraum, Nutzer, Sportfläche |
| VAL | Zeitraum, Sportfläche, Antragsteller |
| BER | Sachbearbeitung |
| LOG | Anlage protokollieren |
| TEST | Anlage aus Antrag, manuelle Anlage |
| DOK | Reservierungsprozess |

**Abhängigkeiten:** Antrag, Sportanlagen, Nutzer  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-RES-002 Reservierung ändern

| Baustein | Zerlegung |
|----------|-----------|
| FE | Reservierung bearbeiten |
| API | PUT `/api/reservierungen/{id}` |
| BL | Zeitraum, Fläche, Status ändern |
| DB | Reservierung |
| VAL | Konfliktprüfung erneut auslösen |
| BER | Sachbearbeitung |
| LOG | Änderung protokollieren |
| TEST | Änderung mit/ohne Konflikt |
| DOK | Änderungsregeln |

**Abhängigkeiten:** FUN-RES-001  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-RES-003 Reservierung löschen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Reservierung löschen/stornieren |
| API | DELETE oder PATCH `/api/reservierungen/{id}` |
| BL | Fachliches Löschen, Status setzen |
| DB | Reservierungsstatus |
| VAL | nicht löschen, wenn Buchung erzeugt |
| BER | Sachbearbeitung |
| LOG | Löschung protokollieren |
| TEST | löschbar/nicht löschbar |
| DOK | Löschregeln |

**Abhängigkeiten:** FUN-RES-001  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-RES-004 Reservierung prüfen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Prüfansicht |
| API | POST `/api/reservierungen/{id}/pruefen` |
| BL | Vollständigkeit, Fachregeln, Zuständigkeit |
| DB | Reservierung, Antrag, Nutzer, Anlage |
| VAL | Pflichtdaten, Status |
| BER | zuständige Rolle |
| LOG | Prüfung protokollieren |
| TEST | vollständig/unvollständig |
| DOK | Prüfkriterien |

**Abhängigkeiten:** Antrag, Nutzer, Sportanlage  
**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-RES-005 Konfliktprüfung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Konfliktanzeige |
| API | POST `/api/reservierungen/konfliktpruefung` |
| BL | Abfrage vorhandene Buchungen, Sperrungen, Winner |
| DB | Events, Occurrences, Winner, Reservierungen |
| VAL | Zeitraum, Sportfläche |
| BER | Sachbearbeitung |
| LOG | Prüfergebnis protokollieren |
| TEST | Konfliktfall, konfliktfrei |
| DOK | Konfliktregeln |

**Abhängigkeiten:** Occurrence/Winner, Sportflächen  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-RES-006 Sperrzeiten berücksichtigen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Anzeige Sperrgründe |
| API | GET `/api/reservierungen/{id}/sperrzeiten` |
| BL | Sperrungen, Reinigung, Havarie, Schulbetrieb berücksichtigen |
| DB | Events, Eventtypen, Winner |
| VAL | Zeitraum |
| BER | Sachbearbeitung, Portal eingeschränkt |
| LOG | optional |
| TEST | Sperrzeit verhindert Reservierung |
| DOK | Sperrlogik |

**Abhängigkeiten:** Buchung/Eventtypen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-RES-007 Saisonlogik

| Baustein | Zerlegung |
|----------|-----------|
| FE | Saisonanzeige |
| API | GET `/api/saisons` / Prüfung in Reservierung |
| BL | Sommer/Winter, ESBZ-Eissaison, Schulsportzeiten |
| DB | Saisonkalender, Verfügbarkeitsregeln |
| VAL | Zeitraum in Saison |
| BER | Sachbearbeitung |
| LOG | Regelanwendung optional |
| TEST | saisonale Verfügbarkeit |
| DOK | Saisonregeln |

**Abhängigkeiten:** Sportanlagen, Fachregeln  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-RES-008 Prioritätsprüfung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Prioritätsanzeige |
| API | POST `/api/reservierungen/prioritaet-pruefen` |
| BL | Zugangssatzung, Nutzergruppe, Belegungsart |
| DB | Nutzer, Eventtyp, Prioritäten |
| VAL | Nutzergruppe erforderlich |
| BER | Sachbearbeitung |
| LOG | Entscheidung protokollieren |
| TEST | höhere/niedrigere Priorität |
| DOK | Prioritätsregeln |

**Abhängigkeiten:** Nutzergruppen, Eventtypen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-RES-009 Warteliste

| Baustein | Zerlegung |
|----------|-----------|
| FE | Wartelistenansicht |
| API | GET/POST `/api/warteliste` |
| BL | Warteliste aus Konfliktfällen |
| DB | Wartelisteneintrag |
| VAL | nur bei Konflikt |
| BER | Sachbearbeitung |
| LOG | Aufnahme/Entfernung protokollieren |
| TEST | Warteliste erzeugen |
| DOK | optionales Feature |

**Abhängigkeiten:** Konfliktprüfung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-RES-010 Reservierungshistorie

| Baustein | Zerlegung |
|----------|-----------|
| FE | Historienanzeige |
| API | GET `/api/reservierungen/{id}/historie` |
| BL | Status- und Änderungsverlauf |
| DB | Reservierungshistorie, Audit |
| VAL | keine |
| BER | Sachbearbeitung |
| LOG | zentraler Audit |
| TEST | Historieneinträge |
| DOK | Historienmodell |

**Abhängigkeiten:** Logging/Audit  
**Risiko:** Mittel  
**Komplexität:** M
