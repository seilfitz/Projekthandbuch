# Technische Zerlegung – Veranstaltungen

| Dokument | 03_Domaenen/Technische_Zerlegung/08_Veranstaltungen.md |
|----------|---------------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Veranstaltungen behandelt veranstaltungsbezogene Belegungen, Sperrflächen, Kabinenplanung, Veranstaltungslisten und besondere Abhängigkeiten zwischen Sportflächen.

---

# 2 Features

## FUN-VERA-001 Veranstaltung anlegen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Veranstaltungsmaske |
| API | POST `/api/veranstaltungen` |
| BL | Veranstaltung als Event/Buchung erzeugen |
| DB | LHD_SPA_EVENTS, Eventtyp Veranstaltung |
| VAL | Zeitraum, Fläche, Titel |
| BER | Sachbearbeitung/Eventmanagement |
| LOG | Anlage protokollieren |
| TEST | Veranstaltung anlegen |
| DOK | Veranstaltungsmodell |

**Abhängigkeiten:** Buchung/Eventtyp  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-VERA-002 Veranstaltungsplanung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Planungsansicht |
| API | GET/PUT `/api/veranstaltungen/{id}/planung` |
| BL | Nebenflächen, Ansprechpartner, Zusatzinformationen |
| DB | Veranstaltungserweiterung |
| VAL | Pflichtinformationen je Veranstaltungsart |
| BER | Eventmanagement/Sachbearbeitung |
| LOG | Änderungen protokollieren |
| TEST | Zusatzdaten speichern |
| DOK | Planungsfelder |

**Abhängigkeiten:** FUN-VERA-001  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-VERA-003 Sperrflächen erzeugen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Sperrflächen auswählen |
| API | POST `/api/veranstaltungen/{id}/sperrflaechen` |
| BL | Sperrungen als Events erzeugen |
| DB | LHD_SPA_EVENTS, Eventtyp Sperrung |
| VAL | benachbarte Flächen, Zeitraum |
| BER | Sachbearbeitung |
| LOG | Sperrung protokollieren |
| SCH | PA_LHD_SPA_OCC |
| TEST | Sperrflächen wirken in Winner |
| DOK | Sperrlogik |

**Abhängigkeiten:** Buchung, Occurrence/Winner  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-VERA-004 Kabinenplanung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Kabinenzuordnung |
| API | GET/PUT `/api/veranstaltungen/{id}/kabinen` |
| BL | Kabinenbedarf, Konflikte, Zuordnung |
| DB | Kabinen, Zuordnung, Zeitraum |
| VAL | Kabine verfügbar |
| BER | Sachbearbeitung/Eventmanagement |
| LOG | Zuordnung protokollieren |
| TEST | Kabinenkonflikt |
| DOK | Kabinenmodell |

**Abhängigkeiten:** Stammdaten Kabinen, Veranstaltungen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-VERA-005 Veranstaltungslisten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Listenansicht / Export |
| API | GET `/api/veranstaltungen/liste` |
| BL | Filter, Sortierung, Export |
| DB | Events, Zusatzdaten |
| VAL | Zeitraum |
| BER | Sachbearbeitung / Portal ggf. eingeschränkt |
| LOG | Export optional protokollieren |
| TEST | Liste, Excel/PDF |
| DOK | Listenformate |

**Abhängigkeiten:** Auswertungen  
**Risiko:** Mittel  
**Komplexität:** M
