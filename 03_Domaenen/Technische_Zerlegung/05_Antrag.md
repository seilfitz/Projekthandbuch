# Technische Zerlegung – Antrag und Wizard

| Dokument | 03_Domaenen/Technische_Zerlegung/05_Antrag.md |
|----------|-----------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Antrag bildet die Online-Antragstellung über ein konfigurierbares Wizard-Framework ab. Antragstypen sollen künftig möglichst durch Konfiguration und nicht durch Programmierung erstellt werden.

---

# 2 Features

## FUN-ANT-001 Antrag erstellen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Wizard-Start, Antragstypauswahl |
| API | POST `/api/antraege` |
| BL | Antrag initialisieren, Entwurfsstatus setzen |
| DB | Antrag, Antragstyp, Status |
| VAL | Antragstyp gültig, Nutzer berechtigt |
| BER | angemeldeter Portalnutzer |
| LOG | Antragserstellung protokollieren |
| TEST | Antrag erstellen, nicht berechtigter Nutzer |
| DOK | Antragserstellung |

**Abhängigkeiten:** Benutzer, Portalnutzer, Wizard-Konfiguration  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-ANT-002 Antrag bearbeiten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Wizard-Schritte bearbeiten |
| API | PUT `/api/antraege/{id}` |
| BL | Zwischenspeicherung, Feldwerte speichern |
| DB | Antrag, Feldwerte |
| VAL | Feldtyp, Pflichtfeld erst bei Einreichung |
| BER | Antragsteller oder Sachbearbeitung |
| LOG | Änderung protokollieren |
| TEST | Bearbeitung Entwurf, gesperrter Status |
| DOK | Bearbeitungsstatus |

**Abhängigkeiten:** FUN-ANT-001  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-ANT-003 Antrag validieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Validierungsanzeige je Schritt |
| API | POST `/api/antraege/{id}/validieren` |
| BL | Feldregeln, Pflichtfelder, fachliche Plausibilität |
| DB | Wizard-Regeln, Antrag |
| VAL | dynamische Regelprüfung |
| BER | Antragsteller/Sachbearbeitung |
| LOG | Validierungsfehler optional |
| TEST | Pflichtfeld, bedingte Felder, Fachregeln |
| DOK | Validierungsregeln |

**Abhängigkeiten:** Wizard-Regeln  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-ANT-004 Anhänge hochladen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Upload-Komponente |
| API | POST `/api/antraege/{id}/anhaenge` |
| BL | Datei speichern, Kategorie zuordnen |
| DB | Anhang, Dokumentbezug |
| VAL | Dateityp, Größe, Pflichtanhang |
| BER | Antragsteller/Sachbearbeitung |
| LOG | Upload/Download protokollieren |
| TEST | gültige/ungültige Datei |
| DOK | Upload-Regeln |

**Abhängigkeiten:** Dokumente/Upload  
**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-ANT-005 Statuswechsel

| Baustein | Zerlegung |
|----------|-----------|
| FE | Statusanzeige / Statusaktionen |
| API | PATCH `/api/antraege/{id}/status` |
| BL | Workflow-Übergänge prüfen und ausführen |
| DB | Antragstatus, Historie |
| VAL | erlaubter Übergang |
| BER | rollenabhängige Statusaktion |
| LOG | Statuswechsel protokollieren |
| TEST | erlaubter/verbotener Wechsel |
| DOK | Statusmodell |

**Abhängigkeiten:** Workflow  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-ANT-006 Antrag archivieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Archivstatus anzeigen |
| API | PATCH `/api/antraege/{id}/archivieren` |
| BL | Archivierung nach Abschluss |
| DB | Archivstatus, Aufbewahrungsfrist |
| VAL | nur abgeschlossene Anträge |
| BER | Sachbearbeitung/Admin |
| LOG | Archivierung protokollieren |
| TEST | Archivierung zulässig/unzulässig |
| DOK | Aufbewahrungsregeln |

**Abhängigkeiten:** Statusmodell, Aufbewahrungsfristen  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-ANT-007 Historie

| Baustein | Zerlegung |
|----------|-----------|
| FE | Historienansicht |
| API | GET `/api/antraege/{id}/historie` |
| BL | Ereignisse bündeln und anzeigen |
| DB | Antragshistorie, Audit |
| VAL | keine |
| BER | Antragsteller eingeschränkt, Sachbearbeitung vollständig |
| LOG | Bestandteil Audit |
| TEST | Historieneinträge sichtbar |
| DOK | Historienmodell |

**Abhängigkeiten:** Logging/Audit  
**Risiko:** Mittel  
**Komplexität:** M

---

# 3 Zusätzliche Kernbausteine Wizard-Framework

## FUN-ANT-WIZ-001 Antragstyp konfigurieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Konfigurationsoberfläche Antragstyp |
| API | POST/PUT `/api/wizard/antragstypen` |
| BL | Antragstypdefinition verwalten |
| DB | Antragstyp, Version, Status |
| VAL | eindeutige Kennung, Versionierung |
| BER | Systemadministrator |
| LOG | Konfigurationsänderung protokollieren |
| TEST | neue Version, aktive Version |
| DOK | Konfigurationsschema |

**Risiko:** Hoch  
**Komplexität:** XXL

---

## FUN-ANT-WIZ-002 Schritte konfigurieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Schritteditor |
| API | POST/PUT `/api/wizard/schritte` |
| BL | Reihenfolge, Sichtbarkeit, Bedingungen |
| DB | Wizard-Schritte |
| VAL | eindeutige Reihenfolge |
| BER | Systemadministrator |
| LOG | Änderung protokollieren |
| TEST | Schrittfolge, bedingte Sichtbarkeit |
| DOK | Schrittmodell |

**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-ANT-WIZ-003 Felder konfigurieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Feldeditor |
| API | POST/PUT `/api/wizard/felder` |
| BL | Feldtypen, Optionen, Pflichtfeldregeln |
| DB | Wizard-Felder, Optionen |
| VAL | Feldtyp gültig, technische Namen eindeutig |
| BER | Systemadministrator |
| LOG | Änderung protokollieren |
| TEST | Text, Datum, Auswahl, Upload |
| DOK | Feldtypenkatalog |

**Risiko:** Hoch  
**Komplexität:** XXL

---

## FUN-ANT-WIZ-004 Regeln konfigurieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Regeleditor |
| API | POST/PUT `/api/wizard/regeln` |
| BL | Sichtbarkeit, Pflichtfelder, Abhängigkeiten |
| DB | Regeldefinition |
| VAL | Regel syntaktisch/fachlich gültig |
| BER | Systemadministrator |
| LOG | Regeländerung protokollieren |
| TEST | Regel greift / greift nicht |
| DOK | Regelmodell |

**Risiko:** Hoch  
**Komplexität:** XXL
