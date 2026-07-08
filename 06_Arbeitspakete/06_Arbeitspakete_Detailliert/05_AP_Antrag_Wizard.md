# Arbeitspakete – Antrag und Wizard

| Dokument | 06_Arbeitspakete/ANT_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Antrag und Wizard**.

---

# 2 Arbeitspakete

## Antrag erstellen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-001 | Oberfläche Antragstypauswahl/Wizard-Start erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Antragstypauswahl/Wizard-Start |
| AP-ANT-002 | REST-Endpunkt Antrag erstellen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Antrag |
| AP-ANT-003 | Businesslogik Antrag initialisieren/Entwurfsstatus setzen | N | siehe Feature-Abhängigkeiten | Businesslogik Antrag initialisieren/Entwurfsstatus setzen |
| AP-ANT-004 | Datenmodell Antrag/Antragstyp/Status erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Antrag/Antragstyp/Status |
| AP-ANT-005 | Validierung Antragstyp und Nutzerberechtigung umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Antragstyp und Nutzerberechtigung |
| AP-ANT-006 | Berechtigung Portalnutzer umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Portalnutzer |
| AP-ANT-007 | Antragserstellung protokollieren | N | siehe Feature-Abhängigkeiten | Antragserstellung protokollieren |
| AP-ANT-008 | Tests Antrag erstellen erstellen | T | siehe Feature-Abhängigkeiten | Tests Antrag |
| AP-ANT-009 | Dokumentation Antragserstellung ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Antragserstellung ergänzen |

## Antrag bearbeiten

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-010 | Oberfläche Wizard-Schritte erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Wizard-Schritte |
| AP-ANT-011 | REST-Endpunkt Antrag bearbeiten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Antrag bearbeiten |
| AP-ANT-012 | Businesslogik Zwischenspeicherung/Feldwerte umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Zwischenspeicherung/Feldwerte |
| AP-ANT-013 | Datenmodell Antrag-Feldwerte erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Antrag-Feldwerte |
| AP-ANT-014 | Validierung Feldtypen umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Feldtypen |
| AP-ANT-015 | Berechtigungen Antragsteller/Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen Antragsteller/Sachbearbeitung |
| AP-ANT-016 | Änderung protokollieren | N | siehe Feature-Abhängigkeiten | Änderung protokollieren |
| AP-ANT-017 | Tests Entwurf/gesperrter Status erstellen | T | siehe Feature-Abhängigkeiten | Tests Entwurf/gesperrter Status |
| AP-ANT-018 | Dokumentation Bearbeitungsstatus ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Bearbeitungsstatus ergänzen |

## Antrag validieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-019 | Oberfläche Validierungsanzeige je Schritt erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Validierungsanzeige je Schritt |
| AP-ANT-020 | REST-Endpunkt Antrag validieren implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Antrag validieren |
| AP-ANT-021 | Businesslogik dynamische Feld-/Fachregeln umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik dynamische Feld-/Fachregeln |
| AP-ANT-022 | Datenmodell Wizard-Regeln anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Wizard-Regeln anbinden |
| AP-ANT-023 | Validierungsengine erstellen | N | siehe Feature-Abhängigkeiten | Validierungsengine |
| AP-ANT-024 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-ANT-025 | Tests Pflichtfeld/bedingte Felder/Fachregeln erstellen | T | siehe Feature-Abhängigkeiten | Tests Pflichtfeld/bedingte Felder/Fachregeln |
| AP-ANT-026 | Dokumentation Validierungsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Validierungsregeln ergänzen |

## Anhänge hochladen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-027 | Upload-Komponente im Wizard erstellen | N | siehe Feature-Abhängigkeiten | Upload-Komponente im Wizard |
| AP-ANT-028 | REST-Endpunkt Anhänge implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Anhänge |
| AP-ANT-029 | Businesslogik Datei speichern/Kategorie zuordnen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Datei speichern/Kategorie zuordnen |
| AP-ANT-030 | Datenmodell Anhang/Dokumentbezug erstellen | D | siehe Feature-Abhängigkeiten | Datenmodell Anhang/Dokumentbezug |
| AP-ANT-031 | Dateityp/Größe/Pflichtanhang validieren | N | siehe Feature-Abhängigkeiten | Dateityp/Größe/Pflichtanhang validieren |
| AP-ANT-032 | Berechtigungen Antragsteller/Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen Antragsteller/Sachbearbeitung |
| AP-ANT-033 | Upload/Download protokollieren | N | siehe Feature-Abhängigkeiten | Upload/Download protokollieren |
| AP-ANT-034 | Tests Upload gültig/ungültig erstellen | T | siehe Feature-Abhängigkeiten | Tests Upload gültig/ungültig |
| AP-ANT-035 | Dokumentation Upload-Regeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Upload-Regeln ergänzen |

## Statuswechsel

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-036 | Oberfläche Statusaktionen erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Statusaktionen |
| AP-ANT-037 | REST-Endpunkt Statuswechsel implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Statuswechsel |
| AP-ANT-038 | Businesslogik Workflow-Übergänge ausführen | N | siehe Feature-Abhängigkeiten | Businesslogik Workflow-Übergänge ausführen |
| AP-ANT-039 | Datenmodell Antragstatus/Historie erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Antragstatus/Historie |
| AP-ANT-040 | Validierung erlaubter Übergang umsetzen | N | siehe Feature-Abhängigkeiten | Validierung erlaubter Übergang |
| AP-ANT-041 | Rollenabhängige Statusaktionen umsetzen | N | siehe Feature-Abhängigkeiten | Rollenabhängige Statusaktionen |
| AP-ANT-042 | Statuswechsel protokollieren | N | siehe Feature-Abhängigkeiten | Statuswechsel protokollieren |
| AP-ANT-043 | Tests erlaubter/verbotener Wechsel erstellen | T | siehe Feature-Abhängigkeiten | Tests erlaubter/verbotener Wechsel |
| AP-ANT-044 | Dokumentation Statusmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Statusmodell ergänzen |

## Antrag archivieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-045 | Oberfläche Archivstatus erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Archivstatus |
| AP-ANT-046 | REST-Endpunkt Archivierung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Archivierung |
| AP-ANT-047 | Businesslogik Archivierung nach Abschluss umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Archivierung nach Abschluss |
| AP-ANT-048 | Datenmodell Archivstatus/Aufbewahrungsfrist erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Archivstatus/Aufbewahrungsfrist |
| AP-ANT-049 | Validierung nur abgeschlossene Anträge umsetzen | N | siehe Feature-Abhängigkeiten | Validierung nur abgeschlossene Anträge |
| AP-ANT-050 | Berechtigung Sachbearbeitung/Admin umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung/Admin |
| AP-ANT-051 | Archivierung protokollieren | N | siehe Feature-Abhängigkeiten | Archivierung protokollieren |
| AP-ANT-052 | Tests Archivierung zulässig/unzulässig erstellen | T | siehe Feature-Abhängigkeiten | Tests Archivierung zulässig/unzulässig |
| AP-ANT-053 | Dokumentation Aufbewahrungsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Aufbewahrungsregeln ergänzen |

## Antragshistorie

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-054 | Oberfläche Historienansicht erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Historienansicht |
| AP-ANT-055 | REST-Endpunkt Antragshistorie implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Antragshistorie |
| AP-ANT-056 | Businesslogik Ereignisse bündeln/anzeigen | N | siehe Feature-Abhängigkeiten | Businesslogik Ereignisse bündeln/anzeigen |
| AP-ANT-057 | Datenmodell Antragshistorie/Audit anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Antragshistorie/Audit anbinden |
| AP-ANT-058 | Berechtigte Sicht Antragsteller/Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigte Sicht Antragsteller/Sachbearbeitung |
| AP-ANT-059 | Tests Historieneinträge sichtbar erstellen | T | siehe Feature-Abhängigkeiten | Tests Historieneinträge sichtbar |
| AP-ANT-060 | Dokumentation Historienmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Historienmodell ergänzen |

## Antragstyp konfigurieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-061 | Oberfläche Antragstyp-Konfiguration erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Antragstyp-Konfiguration |
| AP-ANT-062 | REST-Endpunkte Antragstyp-Konfiguration implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Antragstyp-Konfiguration |
| AP-ANT-063 | Businesslogik Versionierung/Freigabe von Antragstypen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Versionierung/Freigabe von Antragstypen |
| AP-ANT-064 | Datenmodell Antragstyp/Version/Status erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Antragstyp/Version/Status |
| AP-ANT-065 | Validierung eindeutige Kennung/aktive Version umsetzen | N | siehe Feature-Abhängigkeiten | Validierung eindeutige Kennung/aktive Version |
| AP-ANT-066 | Berechtigung Systemadministrator umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Systemadministrator |
| AP-ANT-067 | Konfigurationsänderung protokollieren | N | siehe Feature-Abhängigkeiten | Konfigurationsänderung protokollieren |
| AP-ANT-068 | Tests neue/aktive Version erstellen | T | siehe Feature-Abhängigkeiten | Tests neue/aktive Version |
| AP-ANT-069 | Dokumentation Konfigurationsschema ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Konfigurationsschema ergänzen |

## Schritte konfigurieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-070 | Oberfläche Schritteditor erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Schritteditor |
| AP-ANT-071 | REST-Endpunkte Schritte implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Schritte |
| AP-ANT-072 | Businesslogik Reihenfolge/Sichtbarkeit/Bedingungen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Reihenfolge/Sichtbarkeit/Bedingungen |
| AP-ANT-073 | Datenmodell Wizard-Schritte erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Wizard-Schritte |
| AP-ANT-074 | Validierung Schrittfolge umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Schrittfolge |
| AP-ANT-075 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-ANT-076 | Änderungen protokollieren | N | siehe Feature-Abhängigkeiten | Änderungen protokollieren |
| AP-ANT-077 | Tests Schrittfolge/bedingte Sichtbarkeit erstellen | T | siehe Feature-Abhängigkeiten | Tests Schrittfolge/bedingte Sichtbarkeit |
| AP-ANT-078 | Dokumentation Schrittmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Schrittmodell ergänzen |

## Felder konfigurieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-079 | Oberfläche Feldeditor erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Feldeditor |
| AP-ANT-080 | REST-Endpunkte Felder implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Felder |
| AP-ANT-081 | Businesslogik Feldtypen/Optionen/Pflichtfeldregeln umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Feldtypen/Optionen/Pflichtfeldregeln |
| AP-ANT-082 | Datenmodell Wizard-Felder/Optionen erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Wizard-Felder/Optionen |
| AP-ANT-083 | Validierung technische Namen/Feldtypen umsetzen | N | siehe Feature-Abhängigkeiten | Validierung technische Namen/Feldtypen |
| AP-ANT-084 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-ANT-085 | Änderungen protokollieren | N | siehe Feature-Abhängigkeiten | Änderungen protokollieren |
| AP-ANT-086 | Tests Feldtypen erstellen | T | siehe Feature-Abhängigkeiten | Tests Feldtypen |
| AP-ANT-087 | Dokumentation Feldtypenkatalog ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Feldtypenkatalog ergänzen |

## Regeln konfigurieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-ANT-088 | Oberfläche Regeleditor erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Regeleditor |
| AP-ANT-089 | REST-Endpunkte Regeln implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Regeln |
| AP-ANT-090 | Businesslogik Sichtbarkeit/Pflichtfelder/Abhängigkeiten umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Sichtbarkeit/Pflichtfelder/Abhängigkeiten |
| AP-ANT-091 | Datenmodell Regeldefinition erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Regeldefinition |
| AP-ANT-092 | Validierung Regel syntaktisch/fachlich umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Regel syntaktisch/fachlich |
| AP-ANT-093 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-ANT-094 | Regeländerungen protokollieren | N | siehe Feature-Abhängigkeiten | Regeländerungen protokollieren |
| AP-ANT-095 | Tests Regel greift/greift nicht erstellen | T | siehe Feature-Abhängigkeiten | Tests Regel greift/greift nicht |
| AP-ANT-096 | Dokumentation Regelmodell ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Regelmodell ergänzen |
