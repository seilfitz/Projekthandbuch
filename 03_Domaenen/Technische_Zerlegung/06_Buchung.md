# Technische Zerlegung – Buchung

| Dokument | 03_Domaenen/Technische_Zerlegung/06_Buchung.md |
|----------|-----------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Buchung bildet den fachlichen Kern von SportFM. Buchungen werden weiterhin über bestehende SportFM-Logik und Oracle-Packages verarbeitet. REST und Portal dürfen keine eigene Buchungslogik duplizieren.

---

# 2 Features

## FUN-BUC-001 Buchung erstellen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Buchungsmaske / Übernahme aus Reservierung |
| API | POST `/api/buchungen` |
| BL | Buchung aus Reservierung/Antrag erzeugen |
| DB | LHD_SPA_EVENTS, Zuordnung Nutzer/Sportanlage |
| VAL | Zeitraum, Sportfläche, Eventtyp |
| BER | Sachbearbeitung |
| LOG | Buchungserstellung protokollieren |
| SCH | PA_LHD_SPA |
| TEST | Buchung aus Reservierung, manuelle Buchung |
| DOK | Buchungsprozess |

**Abhängigkeiten:** Reservierung, Nutzer, Sportanlage  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-BUC-002 Buchung ändern

| Baustein | Zerlegung |
|----------|-----------|
| FE | Buchungsbearbeitung |
| API | PUT `/api/buchungen/{id}` |
| BL | fachliche Änderung, Neuberechnung erforderlich |
| DB | LHD_SPA_EVENTS |
| VAL | Status aktiv, Zeitraum plausibel |
| BER | Sachbearbeitung |
| LOG | Änderung protokollieren |
| SCH | PA_LHD_SPA, PA_LHD_SPA_OCC |
| TEST | Änderung mit Occurrence-Neuberechnung |
| DOK | Änderungsregeln |

**Abhängigkeiten:** FUN-BUC-001  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-BUC-003 Serienbuchung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Serien-/Wiederholungsdialog |
| API | POST `/api/buchungen/serie` |
| BL | wöchentlich, 14-tägig, Muster erzeugen |
| DB | LHD_SPA_EVENTS, LHD_SPA_RECURRING_PATTERN |
| VAL | Muster, Zeitraum, Wochentage |
| BER | Sachbearbeitung |
| LOG | Serienanlage protokollieren |
| SCH | PA_LHD_SPA |
| TEST | wöchentlich, 14-tägig, Ferien/Feiertage |
| DOK | Wiederholungslogik |

**Abhängigkeiten:** PA_LHD_SPA  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-BUC-004 Gebühren berechnen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Gebührenanzeige |
| API | POST `/api/gebuehren/berechnen` |
| BL | Gebührenberechnung an bestehende Logik delegieren |
| DB | LHD_SPA_CHARGES, LHD_SPA_EVENTCHARGES |
| VAL | Buchung vollständig, Gebührentyp vorhanden |
| BER | Sachbearbeitung, Portal nur Anzeige |
| LOG | Berechnung optional protokollieren |
| SCH | PA_LHD_SPA |
| TEST | verschiedene Gebührentypen |
| DOK | Gebührenbezug |

**Abhängigkeiten:** Buchung, Gebührentabellen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-BUC-005 Gebührenbefreiung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Kennzeichnung Gebührenbefreiung |
| API | PATCH `/api/buchungen/{id}/gebuehrenbefreiung` |
| BL | Befreiungsgrund, Freigabe |
| DB | Buchung/Gebührenkennzeichen |
| VAL | Grund erforderlich |
| BER | berechtigte Sachbearbeitung |
| LOG | Befreiung protokollieren |
| TEST | Gebührenbefreiung wirkt |
| DOK | Fachregel |

**Abhängigkeiten:** Gebührenberechnung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-BUC-006 Widerruf

| Baustein | Zerlegung |
|----------|-----------|
| FE | Widerrufsaktion |
| API | POST `/api/buchungen/{id}/widerruf` |
| BL | Stornierung als eigene Buchung/Eventtyp erzeugen |
| DB | LHD_SPA_EVENTS, Dokumentbezug |
| VAL | Buchung aktiv, Widerrufsgrund |
| BER | Sachbearbeitung |
| LOG | Widerruf protokollieren |
| SCH | PA_LHD_SPA |
| TEST | Widerruf erzeugt Stornierungs-Event |
| DOK | Widerrufsprozess |

**Abhängigkeiten:** Eventtyp Stornierung, Dokumente  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-BUC-007 Umbuchung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Umbuchungsdialog |
| API | POST `/api/buchungen/{id}/umbuchen` |
| BL | alte Buchung beenden/stornieren, neue Buchung erzeugen |
| DB | LHD_SPA_EVENTS |
| VAL | neuer Zeitraum frei, alte Buchung aktiv |
| BER | Sachbearbeitung |
| LOG | Umbuchung protokollieren |
| SCH | PA_LHD_SPA_OCC |
| TEST | Umbuchung konfliktfrei/mit Konflikt |
| DOK | Umbuchungsregeln |

**Abhängigkeiten:** Konfliktprüfung, Buchung erstellen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-BUC-008 Buchungshistorie

| Baustein | Zerlegung |
|----------|-----------|
| FE | Historienansicht Buchung |
| API | GET `/api/buchungen/{id}/historie` |
| BL | Änderungs- und Statusverlauf anzeigen |
| DB | Audit, Events, Dokumente |
| VAL | keine |
| BER | Sachbearbeitung |
| LOG | Audit |
| TEST | Historie vollständig |
| DOK | Historienmodell |

**Abhängigkeiten:** Logging/Audit  
**Risiko:** Mittel  
**Komplexität:** M
