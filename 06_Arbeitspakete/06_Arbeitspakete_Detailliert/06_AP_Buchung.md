# Arbeitspakete – Buchung

| Dokument | 06_Arbeitspakete/BUC_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Buchung**.

---

# 2 Arbeitspakete

## Buchung erstellen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-001 | Oberfläche Buchung/Übernahme aus Reservierung erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Buchung/Übernahme aus Reservierung |
| AP-BUC-002 | REST-Endpunkt Buchung erstellen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Buchung |
| AP-BUC-003 | Businesslogik Buchung aus Reservierung/Antrag erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik Buchung aus Reservierung/Antrag erzeugen |
| AP-BUC-004 | Datenzugriff LHD_SPA_EVENTS anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_EVENTS anbinden |
| AP-BUC-005 | Validierung Zeitraum/Sportfläche/Eventtyp umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Zeitraum/Sportfläche/Eventtyp |
| AP-BUC-006 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-BUC-007 | Buchungserstellung protokollieren | N | siehe Feature-Abhängigkeiten | Buchungserstellung protokollieren |
| AP-BUC-008 | PA_LHD_SPA anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA anbinden |
| AP-BUC-009 | Tests Buchung aus Reservierung/manuell erstellen | T | siehe Feature-Abhängigkeiten | Tests Buchung aus Reservierung/manuell |
| AP-BUC-010 | Dokumentation Buchungsprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Buchungsprozess ergänzen |

## Buchung ändern

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-011 | Oberfläche Buchungsbearbeitung erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Buchungsbearbeitung |
| AP-BUC-012 | REST-Endpunkt Buchung ändern implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Buchung ändern |
| AP-BUC-013 | Businesslogik fachliche Änderung/Neuberechnung umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik fachliche Änderung/Neuberechnung |
| AP-BUC-014 | Datenzugriff LHD_SPA_EVENTS umsetzen | N | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_EVENTS |
| AP-BUC-015 | Validierung Status/Zeitraum umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Status/Zeitraum |
| AP-BUC-016 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-BUC-017 | Änderung protokollieren | N | siehe Feature-Abhängigkeiten | Änderung protokollieren |
| AP-BUC-018 | PA_LHD_SPA_OCC anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA_OCC anbinden |
| AP-BUC-019 | Tests Änderung mit Occurrence-Neuberechnung erstellen | T | siehe Feature-Abhängigkeiten | Tests Änderung mit Occurrence-Neuberechnung |
| AP-BUC-020 | Dokumentation Änderungsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Änderungsregeln ergänzen |

## Serienbuchung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-021 | Oberfläche Wiederholungsdialog erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Wiederholungsdialog |
| AP-BUC-022 | REST-Endpunkt Serienbuchung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Serienbuchung |
| AP-BUC-023 | Businesslogik wöchentlich/14-tägig/Muster umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik wöchentlich/14-tägig/Muster |
| AP-BUC-024 | Datenzugriff LHD_SPA_RECURRING_PATTERN anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_RECURRING_PATTERN anbinden |
| AP-BUC-025 | Validierung Muster/Zeitraum/Wochentage umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Muster/Zeitraum/Wochentage |
| AP-BUC-026 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-BUC-027 | Serienanlage protokollieren | N | siehe Feature-Abhängigkeiten | Serienanlage protokollieren |
| AP-BUC-028 | PA_LHD_SPA anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA anbinden |
| AP-BUC-029 | Tests Wiederholungen erstellen | T | siehe Feature-Abhängigkeiten | Tests Wiederholungen |
| AP-BUC-030 | Dokumentation Wiederholungslogik ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Wiederholungslogik ergänzen |

## Gebühren berechnen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-031 | Oberfläche Gebührenanzeige erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Gebührenanzeige |
| AP-BUC-032 | REST-Endpunkt Gebührenberechnung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Gebührenberechnung |
| AP-BUC-033 | Businesslogik Gebührenberechnung delegieren | N | siehe Feature-Abhängigkeiten | Businesslogik Gebührenberechnung delegieren |
| AP-BUC-034 | Datenzugriff LHD_SPA_CHARGES/EVENTCHARGES anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_CHARGES/EVENTCHARGES anbinden |
| AP-BUC-035 | Validierung Buchung/Gebührentyp umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Buchung/Gebührentyp |
| AP-BUC-036 | Berechtigung Portal nur Anzeige umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Portal nur Anzeige |
| AP-BUC-037 | Berechnung optional protokollieren | N | siehe Feature-Abhängigkeiten | Berechnung optional protokollieren |
| AP-BUC-038 | PA_LHD_SPA anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA anbinden |
| AP-BUC-039 | Tests verschiedene Gebührentypen erstellen | T | siehe Feature-Abhängigkeiten | Tests verschiedene Gebührentypen |
| AP-BUC-040 | Dokumentation Gebührenbezug ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Gebührenbezug ergänzen |

## Gebührenbefreiung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-041 | Oberfläche Gebührenbefreiung erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Gebührenbefreiung |
| AP-BUC-042 | REST-Endpunkt Gebührenbefreiung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Gebührenbefreiung |
| AP-BUC-043 | Businesslogik Befreiungsgrund/Freigabe umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Befreiungsgrund/Freigabe |
| AP-BUC-044 | Datenmodell Gebührenkennzeichen prüfen | N | siehe Feature-Abhängigkeiten | Datenmodell Gebührenkennzeichen prüfen |
| AP-BUC-045 | Validierung Grund erforderlich umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Grund erforderlich |
| AP-BUC-046 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-BUC-047 | Befreiung protokollieren | N | siehe Feature-Abhängigkeiten | Befreiung protokollieren |
| AP-BUC-048 | Tests Gebührenbefreiung erstellen | T | siehe Feature-Abhängigkeiten | Tests Gebührenbefreiung |
| AP-BUC-049 | Dokumentation Fachregel ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Fachregel ergänzen |

## Widerruf

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-050 | Oberfläche Widerrufsaktion erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Widerrufsaktion |
| AP-BUC-051 | REST-Endpunkt Widerruf implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Widerruf |
| AP-BUC-052 | Businesslogik Stornierung als Eventtyp erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik Stornierung als Eventtyp erzeugen |
| AP-BUC-053 | Datenzugriff LHD_SPA_EVENTS/Dokumentbezug umsetzen | D | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_EVENTS/Dokumentbezug |
| AP-BUC-054 | Validierung aktive Buchung/Widerrufsgrund umsetzen | N | siehe Feature-Abhängigkeiten | Validierung aktive Buchung/Widerrufsgrund |
| AP-BUC-055 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-BUC-056 | Widerruf protokollieren | N | siehe Feature-Abhängigkeiten | Widerruf protokollieren |
| AP-BUC-057 | PA_LHD_SPA anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA anbinden |
| AP-BUC-058 | Tests Stornierungs-Event erstellen | T | siehe Feature-Abhängigkeiten | Tests Stornierungs-Event |
| AP-BUC-059 | Dokumentation Widerrufsprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Widerrufsprozess ergänzen |

## Umbuchung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-060 | Oberfläche Umbuchungsdialog erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Umbuchungsdialog |
| AP-BUC-061 | REST-Endpunkt Umbuchung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Umbuchung |
| AP-BUC-062 | Businesslogik alte Buchung beenden/neue erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik alte Buchung beenden/neue erzeugen |
| AP-BUC-063 | Datenzugriff LHD_SPA_EVENTS umsetzen | N | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_EVENTS |
| AP-BUC-064 | Validierung neuer Zeitraum frei umsetzen | N | siehe Feature-Abhängigkeiten | Validierung neuer Zeitraum frei |
| AP-BUC-065 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-BUC-066 | Umbuchung protokollieren | N | siehe Feature-Abhängigkeiten | Umbuchung protokollieren |
| AP-BUC-067 | PA_LHD_SPA_OCC anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA_OCC anbinden |
| AP-BUC-068 | Tests Umbuchung konfliktfrei/mit Konflikt erstellen | T | siehe Feature-Abhängigkeiten | Tests Umbuchung konfliktfrei/mit Konflikt |
| AP-BUC-069 | Dokumentation Umbuchungsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Umbuchungsregeln ergänzen |

## Buchungshistorie

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-BUC-070 | Oberfläche Buchungshistorie erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Buchungshistorie |
| AP-BUC-071 | REST-Endpunkt Buchungshistorie implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Buchungshistorie |
| AP-BUC-072 | Businesslogik Verlauf aus Audit/Events/Dokumenten anzeigen | D | siehe Feature-Abhängigkeiten | Businesslogik Verlauf aus Audit/Events/Dokumenten anzeigen |
| AP-BUC-073 | Datenzugriff Audit/Events/Dokumente anbinden | D | siehe Feature-Abhängigkeiten | Datenzugriff Audit/Events/Dokumente anbinden |
| AP-BUC-074 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-BUC-075 | Tests Historie vollständig erstellen | T | siehe Feature-Abhängigkeiten | Tests Historie vollständig |
| AP-BUC-076 | Dokumentation Historienmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Historienmodell ergänzen |
