# Arbeitspakete – Veranstaltungen

| Dokument | 06_Arbeitspakete/VERA_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Veranstaltungen**.

---

# 2 Arbeitspakete

## Veranstaltung anlegen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-VERA-001 | Oberfläche Veranstaltungsmaske erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Veranstaltungsmaske |
| AP-VERA-002 | REST-Endpunkt Veranstaltung anlegen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Veranstaltung anlegen |
| AP-VERA-003 | Businesslogik Veranstaltung als Event/Buchung erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik Veranstaltung als Event/Buchung erzeugen |
| AP-VERA-004 | Datenzugriff LHD_SPA_EVENTS/Eventtyp Veranstaltung anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_EVENTS/Eventtyp Veranstaltung anbinden |
| AP-VERA-005 | Validierung Zeitraum/Fläche/Titel umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Zeitraum/Fläche/Titel |
| AP-VERA-006 | Berechtigung Eventmanagement/Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Eventmanagement/Sachbearbeitung |
| AP-VERA-007 | Anlage protokollieren | N | siehe Feature-Abhängigkeiten | Anlage protokollieren |
| AP-VERA-008 | Tests Veranstaltung anlegen erstellen | T | siehe Feature-Abhängigkeiten | Tests Veranstaltung anlegen |
| AP-VERA-009 | Dokumentation Veranstaltungsmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Veranstaltungsmodell ergänzen |

## Veranstaltungsplanung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-VERA-010 | Oberfläche Planungsansicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Planungsansicht |
| AP-VERA-011 | REST-Endpunkte Veranstaltungsplanung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Veranstaltungsplanung |
| AP-VERA-012 | Businesslogik Nebenflächen/Ansprechpartner/Zusatzinformationen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Nebenflächen/Ansprechpartner/Zusatzinformationen |
| AP-VERA-013 | Datenmodell Veranstaltungserweiterung erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Veranstaltungserweiterung |
| AP-VERA-014 | Pflichtinformationen je Veranstaltungsart validieren | N | siehe Feature-Abhängigkeiten | Pflichtinformationen je Veranstaltungsart validieren |
| AP-VERA-015 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-VERA-016 | Änderungen protokollieren | N | siehe Feature-Abhängigkeiten | Änderungen protokollieren |
| AP-VERA-017 | Tests Zusatzdaten speichern erstellen | T | siehe Feature-Abhängigkeiten | Tests Zusatzdaten speichern |
| AP-VERA-018 | Dokumentation Planungsfelder ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Planungsfelder ergänzen |

## Sperrflächen erzeugen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-VERA-019 | Oberfläche Sperrflächen auswählen erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Sperrflächen auswählen |
| AP-VERA-020 | REST-Endpunkt Sperrflächen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Sperrflächen |
| AP-VERA-021 | Businesslogik Sperrungen als Events erzeugen | N | siehe Feature-Abhängigkeiten | Businesslogik Sperrungen als Events erzeugen |
| AP-VERA-022 | Datenzugriff Eventtyp Sperrung anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Eventtyp Sperrung anbinden |
| AP-VERA-023 | Validierung benachbarte Flächen/Zeitraum umsetzen | N | siehe Feature-Abhängigkeiten | Validierung benachbarte Flächen/Zeitraum |
| AP-VERA-024 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-VERA-025 | Sperrung protokollieren | N | siehe Feature-Abhängigkeiten | Sperrung protokollieren |
| AP-VERA-026 | PA_LHD_SPA_OCC anbinden | I | siehe Feature-Abhängigkeiten | PA_LHD_SPA_OCC anbinden |
| AP-VERA-027 | Tests Sperrflächen wirken in Winner erstellen | T | siehe Feature-Abhängigkeiten | Tests Sperrflächen wirken in Winner |
| AP-VERA-028 | Dokumentation Sperrlogik ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Sperrlogik ergänzen |

## Kabinenplanung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-VERA-029 | Oberfläche Kabinenzuordnung erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Kabinenzuordnung |
| AP-VERA-030 | REST-Endpunkte Kabinen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Kabinen |
| AP-VERA-031 | Businesslogik Kabinenbedarf/Konflikte/Zuordnung umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Kabinenbedarf/Konflikte/Zuordnung |
| AP-VERA-032 | Datenmodell Kabinen/Zuordnung/Zeitraum erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Kabinen/Zuordnung/Zeitraum |
| AP-VERA-033 | Validierung Kabinenverfügbarkeit umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Kabinenverfügbarkeit |
| AP-VERA-034 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-VERA-035 | Zuordnung protokollieren | N | siehe Feature-Abhängigkeiten | Zuordnung protokollieren |
| AP-VERA-036 | Tests Kabinenkonflikt erstellen | T | siehe Feature-Abhängigkeiten | Tests Kabinenkonflikt |
| AP-VERA-037 | Dokumentation Kabinenmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Kabinenmodell ergänzen |

## Veranstaltungslisten

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-VERA-038 | Oberfläche Veranstaltungslisten erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Veranstaltungslisten |
| AP-VERA-039 | REST-Endpunkt Veranstaltungslisten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Veranstaltungslisten |
| AP-VERA-040 | Businesslogik Filter/Sortierung/Export umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Filter/Sortierung/Export |
| AP-VERA-041 | Datenzugriff Events/Zusatzdaten anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Events/Zusatzdaten anbinden |
| AP-VERA-042 | Validierung Zeitraum umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Zeitraum |
| AP-VERA-043 | Berechtigte Portal-/interne Sicht umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigte Portal-/interne Sicht |
| AP-VERA-044 | Export optional protokollieren | N | siehe Feature-Abhängigkeiten | Export optional protokollieren |
| AP-VERA-045 | Tests Liste/Excel/PDF erstellen | T | siehe Feature-Abhängigkeiten | Tests Liste/Excel/PDF |
| AP-VERA-046 | Dokumentation Listenformate ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Listenformate ergänzen |
