# Technische Zerlegung – Auswertungen

| Dokument | 03_Domaenen/Technische_Zerlegung/10_Auswertungen.md |
|----------|------------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Auswertungen umfasst Belegungspläne, Tages-/Wochen-/Monatsansichten, Gebührenübersichten und Exporte. Ziel ist eine wiederverwendbare Reporting-Grundlage.

---

# 2 Features

## FUN-AUS-001 Belegungsplan

| Baustein | Zerlegung |
|----------|-----------|
| FE | Belegungsplanansicht |
| API | GET `/api/auswertungen/belegungsplan` |
| BL | Daten aus Buchung/Winner aufbereiten |
| DB | Events, Occurrences, Winner |
| VAL | Zeitraum, Anlage |
| BER | rollenabhängig |
| LOG | Export optional protokollieren |
| TEST | Plan für Anlage/Zeitraum |
| DOK | Belegungsplanlogik |

**Abhängigkeiten:** Buchung, Occurrence/Winner  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-AUS-002 Tagesbelegung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Tagesansicht |
| API | GET `/api/auswertungen/tagesbelegung` |
| BL | Tagesdaten aufbereiten |
| DB | Winner/Occurrences |
| VAL | Datum, Anlage |
| BER | rollenabhängig |
| LOG | optional |
| TEST | Tagesbelegung korrekt |
| DOK | Tagesansicht |

**Abhängigkeiten:** Buchung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-AUS-003 Wochenbelegung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Wochenansicht |
| API | GET `/api/auswertungen/wochenbelegung` |
| BL | Wochenraster erzeugen |
| DB | Winner/Occurrences |
| VAL | Kalenderwoche |
| BER | rollenabhängig |
| LOG | optional |
| TEST | Wochenraster |
| DOK | Wochenansicht |

**Abhängigkeiten:** Buchung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-AUS-004 Monatsübersicht

| Baustein | Zerlegung |
|----------|-----------|
| FE | Monatskalender |
| API | GET `/api/auswertungen/monatsuebersicht` |
| BL | Monatsdaten aggregieren |
| DB | Winner/Occurrences |
| VAL | Monat/Jahr |
| BER | rollenabhängig |
| LOG | optional |
| TEST | Monatsdaten |
| DOK | Monatsübersicht |

**Abhängigkeiten:** Buchung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-AUS-005 Gebührenübersicht

| Baustein | Zerlegung |
|----------|-----------|
| FE | Gebührenübersicht |
| API | GET `/api/auswertungen/gebuehren` |
| BL | Gebühren je Nutzer/Zeitraum aggregieren |
| DB | Gebühren, Buchungen, Rechnungen |
| VAL | Zeitraum, Nutzer |
| BER | Sachbearbeitung |
| LOG | Export optional |
| TEST | Gebührenübersicht |
| DOK | Gebührenbericht |

**Abhängigkeiten:** Gebühren, Rechnungen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-AUS-006 Export Excel

| Baustein | Zerlegung |
|----------|-----------|
| FE | Exportbutton |
| API | GET `/api/auswertungen/{id}/excel` |
| BL | Excel generieren |
| DB | Berichtsdaten |
| VAL | Exportformat |
| BER | rollenabhängig |
| LOG | Export protokollieren |
| TEST | Datei erzeugen |
| DOK | Exportformate |

**Abhängigkeiten:** Reporting-Grundlage  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-AUS-007 Export PDF

| Baustein | Zerlegung |
|----------|-----------|
| FE | PDF-Export |
| API | GET `/api/auswertungen/{id}/pdf` |
| BL | PDF generieren |
| DB | Berichtsdaten |
| VAL | Exportformat |
| BER | rollenabhängig |
| LOG | Export protokollieren |
| TEST | PDF erzeugen |
| DOK | PDF-Export |

**Abhängigkeiten:** Reporting-Grundlage, PDF-Komponente  
**Risiko:** Mittel  
**Komplexität:** M
