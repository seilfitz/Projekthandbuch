# Arbeitspakete – Reservierung

| Dokument | 06_Arbeitspakete/RES_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Reservierung**.

---

# 2 Arbeitspakete

## Reservierung anlegen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-001 | Oberfläche Reservierungsanlage/Arbeitskorb erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Reservierungsanlage/Arbeitskorb |
| AP-RES-002 | REST-Endpunkt Reservierung anlegen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Reservierung anlegen |
| AP-RES-003 | Businesslogik Reservierung aus Antrag/manuell erstellen | N | siehe Feature-Abhängigkeiten | Businesslogik Reservierung aus Antrag/manuell |
| AP-RES-004 | Datenmodell Reservierung erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Reservierung |
| AP-RES-005 | Validierung Zeitraum/Sportfläche/Nutzer umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Zeitraum/Sportfläche/Nutzer |
| AP-RES-006 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-RES-007 | Anlage protokollieren | N | siehe Feature-Abhängigkeiten | Anlage protokollieren |
| AP-RES-008 | Tests Reservierung anlegen erstellen | T | siehe Feature-Abhängigkeiten | Tests Reservierung anlegen |
| AP-RES-009 | Dokumentation Reservierungsprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Reservierungsprozess ergänzen |

## Reservierung ändern

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-010 | Oberfläche Reservierung bearbeiten erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Reservierung bearbeiten |
| AP-RES-011 | REST-Endpunkt Reservierung ändern implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Reservierung ändern |
| AP-RES-012 | Businesslogik Änderung und Neuberechnung auslösen | N | siehe Feature-Abhängigkeiten | Businesslogik Änderung und Neuberechnung auslösen |
| AP-RES-013 | Datenmodell Reservierung prüfen | N | siehe Feature-Abhängigkeiten | Datenmodell Reservierung prüfen |
| AP-RES-014 | Konfliktprüfung bei Änderung anbinden | I | siehe Feature-Abhängigkeiten | Konfliktprüfung bei Änderung anbinden |
| AP-RES-015 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-016 | Änderungen protokollieren | N | siehe Feature-Abhängigkeiten | Änderungen protokollieren |
| AP-RES-017 | Tests Änderung mit/ohne Konflikt erstellen | T | siehe Feature-Abhängigkeiten | Tests Änderung mit/ohne Konflikt |
| AP-RES-018 | Dokumentation Änderungsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Änderungsregeln ergänzen |

## Reservierung löschen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-019 | Oberfläche Reservierung löschen/stornieren erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Reservierung löschen/stornieren |
| AP-RES-020 | REST-Endpunkt fachliches Löschen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt fachliches Löschen |
| AP-RES-021 | Businesslogik Status/Löschlogik umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Status/Löschlogik |
| AP-RES-022 | Datenmodell Reservierungsstatus erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Reservierungsstatus |
| AP-RES-023 | Validierung nicht löschen nach Buchung umsetzen | N | siehe Feature-Abhängigkeiten | Validierung nicht löschen nach Buchung |
| AP-RES-024 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-025 | Löschung protokollieren | N | siehe Feature-Abhängigkeiten | Löschung protokollieren |
| AP-RES-026 | Tests löschbar/nicht löschbar erstellen | T | siehe Feature-Abhängigkeiten | Tests löschbar/nicht löschbar |
| AP-RES-027 | Dokumentation Löschregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Löschregeln ergänzen |

## Reservierung prüfen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-028 | Oberfläche Prüfansicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Prüfansicht |
| AP-RES-029 | REST-Endpunkt Reservierung prüfen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Reservierung prüfen |
| AP-RES-030 | Businesslogik Vollständigkeit/Fachregeln/Zuständigkeit umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Vollständigkeit/Fachregeln/Zuständigkeit |
| AP-RES-031 | Datenzugriff Antrag/Nutzer/Anlage anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Antrag/Nutzer/Anlage anbinden |
| AP-RES-032 | Validierung Pflichtdaten/Status umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Pflichtdaten/Status |
| AP-RES-033 | Berechtigung zuständige Rolle umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung zuständige Rolle |
| AP-RES-034 | Prüfung protokollieren | N | siehe Feature-Abhängigkeiten | Prüfung protokollieren |
| AP-RES-035 | Tests vollständig/unvollständig erstellen | T | siehe Feature-Abhängigkeiten | Tests vollständig/unvollständig |
| AP-RES-036 | Dokumentation Prüfkriterien ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Prüfkriterien ergänzen |

## Konfliktprüfung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-037 | Oberfläche Konfliktanzeige erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Konfliktanzeige |
| AP-RES-038 | REST-Endpunkt Konfliktprüfung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Konfliktprüfung |
| AP-RES-039 | Businesslogik Events/OCC/Winner/Reservierungen prüfen | N | siehe Feature-Abhängigkeiten | Businesslogik Events/OCC/Winner/Reservierungen prüfen |
| AP-RES-040 | Datenzugriff Occurrence/Winner optimieren | N | siehe Feature-Abhängigkeiten | Datenzugriff Occurrence/Winner optimieren |
| AP-RES-041 | Validierung Zeitraum/Sportfläche umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Zeitraum/Sportfläche |
| AP-RES-042 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-043 | Prüfergebnis protokollieren | N | siehe Feature-Abhängigkeiten | Prüfergebnis protokollieren |
| AP-RES-044 | Tests konfliktfrei/Konfliktfall erstellen | T | siehe Feature-Abhängigkeiten | Tests konfliktfrei/Konfliktfall |
| AP-RES-045 | Dokumentation Konfliktregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Konfliktregeln ergänzen |

## Sperrzeiten berücksichtigen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-046 | Oberfläche Sperrgründe anzeigen erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Sperrgründe anzeigen |
| AP-RES-047 | REST-Endpunkt Sperrzeiten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Sperrzeiten |
| AP-RES-048 | Businesslogik Sperrung/Reinigung/Havarie/Schulbetrieb berücksichtigen | N | siehe Feature-Abhängigkeiten | Businesslogik Sperrung/Reinigung/Havarie/Schulbetrieb berücksichtigen |
| AP-RES-049 | Datenzugriff Eventtypen/Winner anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Eventtypen/Winner anbinden |
| AP-RES-050 | Zeitraum validieren | N | siehe Feature-Abhängigkeiten | Zeitraum validieren |
| AP-RES-051 | Portal-Sicht einschränken | N | siehe Feature-Abhängigkeiten | Portal-Sicht einschränken |
| AP-RES-052 | Tests Sperrzeit verhindert Reservierung erstellen | T | siehe Feature-Abhängigkeiten | Tests Sperrzeit verhindert Reservierung |
| AP-RES-053 | Dokumentation Sperrlogik ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Sperrlogik ergänzen |

## Saisonlogik

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-054 | Oberfläche Saisonanzeige erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Saisonanzeige |
| AP-RES-055 | REST-Endpunkte Saisoninformationen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Saisoninformationen |
| AP-RES-056 | Businesslogik Sommer/Winter/ESBZ/Schulsport umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Sommer/Winter/ESBZ/Schulsport |
| AP-RES-057 | Datenmodell Saisonkalender/Regeln erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Saisonkalender/Regeln |
| AP-RES-058 | Zeitraum in Saison validieren | N | siehe Feature-Abhängigkeiten | Zeitraum in Saison validieren |
| AP-RES-059 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-060 | Tests saisonale Verfügbarkeit erstellen | T | siehe Feature-Abhängigkeiten | Tests saisonale Verfügbarkeit |
| AP-RES-061 | Dokumentation Saisonregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Saisonregeln ergänzen |

## Prioritätsprüfung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-062 | Oberfläche Prioritätsanzeige erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Prioritätsanzeige |
| AP-RES-063 | REST-Endpunkt Prioritätsprüfung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Prioritätsprüfung |
| AP-RES-064 | Businesslogik Zugangssatzung/Nutzergruppe/Belegungsart umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Zugangssatzung/Nutzergruppe/Belegungsart |
| AP-RES-065 | Datenzugriff Nutzer/Eventtypen/Prioritäten anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Nutzer/Eventtypen/Prioritäten anbinden |
| AP-RES-066 | Nutzergruppe validieren | N | siehe Feature-Abhängigkeiten | Nutzergruppe validieren |
| AP-RES-067 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-RES-068 | Entscheidung protokollieren | N | siehe Feature-Abhängigkeiten | Entscheidung protokollieren |
| AP-RES-069 | Tests höhere/niedrigere Priorität erstellen | T | siehe Feature-Abhängigkeiten | Tests höhere/niedrigere Priorität |
| AP-RES-070 | Dokumentation Prioritätsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Prioritätsregeln ergänzen |

## Warteliste

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-071 | Oberfläche Warteliste erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Warteliste |
| AP-RES-072 | REST-Endpunkte Warteliste implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Warteliste |
| AP-RES-073 | Businesslogik Warteliste aus Konfliktfällen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Warteliste aus Konfliktfällen |
| AP-RES-074 | Datenmodell Wartelisteneintrag erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Wartelisteneintrag |
| AP-RES-075 | Validierung nur bei Konflikt umsetzen | N | siehe Feature-Abhängigkeiten | Validierung nur bei Konflikt |
| AP-RES-076 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-077 | Aufnahme/Entfernung protokollieren | N | siehe Feature-Abhängigkeiten | Aufnahme/Entfernung protokollieren |
| AP-RES-078 | Tests Warteliste erstellen | T | siehe Feature-Abhängigkeiten | Tests Warteliste |
| AP-RES-079 | Dokumentation Wartelistenregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Wartelistenregeln ergänzen |

## Reservierungshistorie

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-RES-080 | Oberfläche Historie erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Historie |
| AP-RES-081 | REST-Endpunkt Reservierungshistorie implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Reservierungshistorie |
| AP-RES-082 | Businesslogik Status-/Änderungsverlauf anzeigen | N | siehe Feature-Abhängigkeiten | Businesslogik Status-/Änderungsverlauf anzeigen |
| AP-RES-083 | Datenmodell Historie/Audit anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Historie/Audit anbinden |
| AP-RES-084 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-RES-085 | Tests Historieneinträge erstellen | T | siehe Feature-Abhängigkeiten | Tests Historieneinträge |
| AP-RES-086 | Dokumentation Historienmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Historienmodell ergänzen |
