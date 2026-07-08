# Arbeitspakete – Auswertungen

| Dokument | 06_Arbeitspakete/AUS_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Auswertungen**.

---

# 2 Arbeitspakete

## Belegungsplan

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-001 | Oberfläche Belegungsplan erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Belegungsplan |
| AP-AUS-002 | REST-Endpunkt Belegungsplan implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Belegungsplan |
| AP-AUS-003 | Businesslogik Daten aus Buchung/Winner aufbereiten | N | siehe Feature-Abhängigkeiten | Businesslogik Daten aus Buchung/Winner aufbereiten |
| AP-AUS-004 | Datenzugriff Events/OCC/Winner optimieren | N | siehe Feature-Abhängigkeiten | Datenzugriff Events/OCC/Winner optimieren |
| AP-AUS-005 | Zeitraum/Anlage validieren | N | siehe Feature-Abhängigkeiten | Zeitraum/Anlage validieren |
| AP-AUS-006 | Rollenabhängige Sicht umsetzen | N | siehe Feature-Abhängigkeiten | Rollenabhängige Sicht |
| AP-AUS-007 | Export optional protokollieren | N | siehe Feature-Abhängigkeiten | Export optional protokollieren |
| AP-AUS-008 | Tests Plan für Anlage/Zeitraum erstellen | T | siehe Feature-Abhängigkeiten | Tests Plan für Anlage/Zeitraum |
| AP-AUS-009 | Dokumentation Belegungsplanlogik ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Belegungsplanlogik ergänzen |

## Tagesbelegung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-010 | Oberfläche Tagesansicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Tagesansicht |
| AP-AUS-011 | REST-Endpunkt Tagesbelegung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Tagesbelegung |
| AP-AUS-012 | Businesslogik Tagesdaten aufbereiten | N | siehe Feature-Abhängigkeiten | Businesslogik Tagesdaten aufbereiten |
| AP-AUS-013 | Datenzugriff Winner/OCC anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Winner/OCC anbinden |
| AP-AUS-014 | Datum/Anlage validieren | N | siehe Feature-Abhängigkeiten | Datum/Anlage validieren |
| AP-AUS-015 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-AUS-016 | Tests Tagesbelegung korrekt erstellen | T | siehe Feature-Abhängigkeiten | Tests Tagesbelegung korrekt |
| AP-AUS-017 | Dokumentation Tagesansicht ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Tagesansicht ergänzen |

## Wochenbelegung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-018 | Oberfläche Wochenansicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Wochenansicht |
| AP-AUS-019 | REST-Endpunkt Wochenbelegung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Wochenbelegung |
| AP-AUS-020 | Businesslogik Wochenraster erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik Wochenraster erzeugen |
| AP-AUS-021 | Datenzugriff Winner/OCC anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Winner/OCC anbinden |
| AP-AUS-022 | Kalenderwoche validieren | N | siehe Feature-Abhängigkeiten | Kalenderwoche validieren |
| AP-AUS-023 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-AUS-024 | Tests Wochenraster erstellen | T | siehe Feature-Abhängigkeiten | Tests Wochenraster |
| AP-AUS-025 | Dokumentation Wochenansicht ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Wochenansicht ergänzen |

## Monatsübersicht

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-026 | Oberfläche Monatskalender erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Monatskalender |
| AP-AUS-027 | REST-Endpunkt Monatsübersicht implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Monatsübersicht |
| AP-AUS-028 | Businesslogik Monatsdaten aggregieren | N | siehe Feature-Abhängigkeiten | Businesslogik Monatsdaten aggregieren |
| AP-AUS-029 | Datenzugriff Winner/OCC anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Winner/OCC anbinden |
| AP-AUS-030 | Monat/Jahr validieren | N | siehe Feature-Abhängigkeiten | Monat/Jahr validieren |
| AP-AUS-031 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-AUS-032 | Tests Monatsdaten erstellen | T | siehe Feature-Abhängigkeiten | Tests Monatsdaten |
| AP-AUS-033 | Dokumentation Monatsübersicht ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Monatsübersicht ergänzen |

## Gebührenübersicht

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-034 | Oberfläche Gebührenübersicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Gebührenübersicht |
| AP-AUS-035 | REST-Endpunkt Gebührenübersicht implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Gebührenübersicht |
| AP-AUS-036 | Businesslogik Gebühren je Nutzer/Zeitraum aggregieren | N | siehe Feature-Abhängigkeiten | Businesslogik Gebühren je Nutzer/Zeitraum aggregieren |
| AP-AUS-037 | Datenzugriff Gebühren/Buchungen/Rechnungen anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Gebühren/Buchungen/Rechnungen anbinden |
| AP-AUS-038 | Zeitraum/Nutzer validieren | N | siehe Feature-Abhängigkeiten | Zeitraum/Nutzer validieren |
| AP-AUS-039 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-AUS-040 | Export optional protokollieren | N | siehe Feature-Abhängigkeiten | Export optional protokollieren |
| AP-AUS-041 | Tests Gebührenübersicht erstellen | T | siehe Feature-Abhängigkeiten | Tests Gebührenübersicht |
| AP-AUS-042 | Dokumentation Gebührenbericht ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Gebührenbericht ergänzen |

## Export Excel

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-043 | Exportbutton Excel einbauen | N | siehe Feature-Abhängigkeiten | Exportbutton Excel einbauen |
| AP-AUS-044 | REST-Endpunkt Excel-Export implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Excel-Export |
| AP-AUS-045 | Businesslogik Excel generieren umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Excel generieren |
| AP-AUS-046 | Berichtsdaten anbinden | I | siehe Feature-Abhängigkeiten | Berichtsdaten anbinden |
| AP-AUS-047 | Exportformat validieren | N | siehe Feature-Abhängigkeiten | Exportformat validieren |
| AP-AUS-048 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-AUS-049 | Export protokollieren | N | siehe Feature-Abhängigkeiten | Export protokollieren |
| AP-AUS-050 | Tests Datei erzeugen erstellen | T | siehe Feature-Abhängigkeiten | Tests Datei erzeugen |
| AP-AUS-051 | Dokumentation Exportformate ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Exportformate ergänzen |

## Export PDF

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-AUS-052 | PDF-Export einbauen | N | siehe Feature-Abhängigkeiten | PDF-Export einbauen |
| AP-AUS-053 | REST-Endpunkt PDF-Export implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt PDF-Export |
| AP-AUS-054 | Businesslogik PDF generieren umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik PDF generieren |
| AP-AUS-055 | Berichtsdaten anbinden | I | siehe Feature-Abhängigkeiten | Berichtsdaten anbinden |
| AP-AUS-056 | Exportformat validieren | N | siehe Feature-Abhängigkeiten | Exportformat validieren |
| AP-AUS-057 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-AUS-058 | Export protokollieren | N | siehe Feature-Abhängigkeiten | Export protokollieren |
| AP-AUS-059 | Tests PDF erzeugen erstellen | T | siehe Feature-Abhängigkeiten | Tests PDF erzeugen |
| AP-AUS-060 | Dokumentation PDF-Export ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation PDF-Export ergänzen |
