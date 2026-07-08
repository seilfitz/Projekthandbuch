# Technische Zerlegung – Schnittstellen

| Dokument | 03_Domaenen/Technische_Zerlegung/11_Schnittstellen.md |
|----------|--------------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Schnittstellen beschreibt die technische Zerlegung externer Integrationen. Bestehende Schnittstellen werden weiterverwendet, neue Schnittstellen werden über definierte Adapter angebunden.

---

# 2 Features

## FUN-SST-001 RIB FM

| Baustein | Zerlegung |
|----------|-----------|
| API | interne Adapter / Datenzugriff |
| BL | Stammdatenübernahme Nutzer, Sportanlagen |
| DB | RIB-FM-Bezüge |
| VAL | Dubletten, Pflichtdaten |
| BER | interne Systemrechte |
| LOG | Import/Sync protokollieren |
| SCH | RIB FM |
| TEST | Stammdatenabgleich |
| DOK | Datenhoheit |

**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-SST-002 Themenstadtplan

| Baustein | Zerlegung |
|----------|-----------|
| FE | Kartenintegration |
| API | GET `/api/karten/sportanlagen` |
| BL | Sportanlagen für Karte aufbereiten |
| DB | Koordinaten, Sportanlagen |
| VAL | Koordinaten vorhanden |
| BER | öffentlich |
| LOG | optional |
| SCH | Themenstadtplan |
| TEST | Kartenlink, Antrag aus Karte |
| DOK | Kartenintegration |

**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-SST-003 SAP

| Baustein | Zerlegung |
|----------|-----------|
| API | bestehender SAP-Client |
| BL | Rechnung/Buchung an SAP übergeben |
| DB | Rechnungen, SAP-Belegnummern |
| VAL | vollständige Rechnungsdaten |
| BER | technische Systemberechtigung |
| LOG | SAP-Aufruf, Ergebnis, Fehler |
| SCH | SAP FI REST API / OAuth |
| TEST | Übergabe, Fehlerfall |
| DOK | SAP-Prozess |

**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-SST-004 ePayBL

| Baustein | Zerlegung |
|----------|-----------|
| FE | Zahlungsstart im Portal |
| API | POST `/api/zahlungen/starten` |
| BL | Zahlungsanforderung, Rückmeldung verarbeiten |
| DB | Zahlung, Rechnungsbezug, Status |
| VAL | Rechnung zahlbar |
| BER | Rechnung gehört Portalnutzer |
| LOG | Zahlungsvorgänge revisionsfähig |
| SCH | ePayBL |
| TEST | Zahlungsstart, Rückmeldung, Abbruch |
| DOK | Zahlungsprozess |

**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-SST-005 LSBS

| Baustein | Zerlegung |
|----------|-----------|
| API | Import-/Sync-Endpunkt |
| BL | Mitgliederstatistik, Vereinsdaten übernehmen |
| DB | Verein, Mitgliedschaft, Nachweise |
| VAL | Datenformat, Vereinzuordnung |
| BER | technische Berechtigung |
| LOG | Importprotokoll |
| SCH | LSBS |
| TEST | Import gültig/ungültig |
| DOK | Importformat |

**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-SST-006 Formcycle

| Baustein | Zerlegung |
|----------|-----------|
| API | Import/Übergabe Onlineformular |
| BL | Formulardaten in Antrag/Wizard überführen |
| DB | Antrag, Feldwerte, Anhänge |
| VAL | Mapping, Pflichtfelder |
| BER | technische Berechtigung |
| LOG | Eingang protokollieren |
| SCH | Formcycle |
| TEST | Datenübernahme |
| DOK | Mappingbeschreibung |

**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-SST-007 Cardo

| Baustein | Zerlegung |
|----------|-----------|
| API | Karten-/Geodatenadapter |
| BL | Geodaten abrufen und darstellen |
| DB | Georeferenz |
| VAL | Koordinaten |
| BER | je Schnittstelle |
| LOG | optional |
| SCH | Cardo |
| TEST | Geodatenanzeige |
| DOK | Schnittstellenbeschreibung |

**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-SST-008 Schuldatenbank

| Baustein | Zerlegung |
|----------|-----------|
| API | Import-/Abgleichschnittstelle |
| BL | Schuldaten, Schulsportbezug, Ansprechpartner |
| DB | Schulen, Ansprechpartner, Sportanlagenbezug |
| VAL | Schul-ID, Zuordnung |
| BER | technische Berechtigung |
| LOG | Importprotokoll |
| SCH | Schuldatenbank |
| TEST | Datenabgleich |
| DOK | Importregeln |

**Risiko:** Mittel  
**Komplexität:** L
