# Arbeitspakete – Schnittstellen

| Dokument | 06_Arbeitspakete/SST_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Schnittstellen**.

---

# 2 Arbeitspakete

## RIB FM

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-001 | Adapterkonzept RIB FM erstellen | I | siehe Feature-Abhängigkeiten | Adapterkonzept RIB FM |
| AP-SST-002 | Stammdatenabgleich Nutzer/Sportanlagen implementieren | N | siehe Feature-Abhängigkeiten | Stammdatenabgleich Nutzer/Sportanlagen |
| AP-SST-003 | Datenbezüge RIB FM dokumentieren | D | siehe Feature-Abhängigkeiten | Datenbezüge RIB FM dokumentieren |
| AP-SST-004 | Dubletten-/Pflichtdatenvalidierung umsetzen | N | siehe Feature-Abhängigkeiten | Dubletten-/Pflichtdatenvalidierung |
| AP-SST-005 | Systemberechtigungen definieren | N | siehe Feature-Abhängigkeiten | Systemberechtigungen definieren |
| AP-SST-006 | Import/Sync protokollieren | N | siehe Feature-Abhängigkeiten | Import/Sync protokollieren |
| AP-SST-007 | Tests Stammdatenabgleich erstellen | T | siehe Feature-Abhängigkeiten | Tests Stammdatenabgleich |
| AP-SST-008 | Dokumentation Datenhoheit ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Datenhoheit ergänzen |

## Themenstadtplan

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-009 | Kartenintegration Portal umsetzen | N | siehe Feature-Abhängigkeiten | Kartenintegration Portal |
| AP-SST-010 | REST-Endpunkt Kartendaten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Kartendaten |
| AP-SST-011 | Businesslogik Sportanlagen für Karte aufbereiten | N | siehe Feature-Abhängigkeiten | Businesslogik Sportanlagen für Karte aufbereiten |
| AP-SST-012 | Koordinaten/Sportanlagen anbinden | I | siehe Feature-Abhängigkeiten | Koordinaten/Sportanlagen anbinden |
| AP-SST-013 | Koordinatenvalidierung umsetzen | N | siehe Feature-Abhängigkeiten | Koordinatenvalidierung |
| AP-SST-014 | öffentliche Sicht absichern | N | siehe Feature-Abhängigkeiten | öffentliche Sicht absichern |
| AP-SST-015 | Tests Kartenlink/Antrag aus Karte erstellen | T | siehe Feature-Abhängigkeiten | Tests Kartenlink/Antrag aus Karte |
| AP-SST-016 | Dokumentation Kartenintegration ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Kartenintegration ergänzen |

## SAP

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-017 | Bestehenden SAP-Client prüfen | I | siehe Feature-Abhängigkeiten | Bestehenden SAP-Client prüfen |
| AP-SST-018 | Übergabe Rechnung/Buchung an SAP anbinden | I | siehe Feature-Abhängigkeiten | Übergabe Rechnung/Buchung an SAP anbinden |
| AP-SST-019 | SAP-Belegnummern/Rückmeldungen speichern | I | siehe Feature-Abhängigkeiten | SAP-Belegnummern/Rückmeldungen speichern |
| AP-SST-020 | Vollständige Rechnungsdaten validieren | N | siehe Feature-Abhängigkeiten | Vollständige Rechnungsdaten validieren |
| AP-SST-021 | technische Systemberechtigung umsetzen | N | siehe Feature-Abhängigkeiten | technische Systemberechtigung |
| AP-SST-022 | SAP-Aufruf/Ergebnis/Fehler protokollieren | I | siehe Feature-Abhängigkeiten | SAP-Aufruf/Ergebnis/Fehler protokollieren |
| AP-SST-023 | Tests Übergabe/Fehlerfall erstellen | T | siehe Feature-Abhängigkeiten | Tests Übergabe/Fehlerfall |
| AP-SST-024 | Dokumentation SAP-Prozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation SAP-Prozess ergänzen |

## ePayBL

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-025 | Zahlungsstart im Portal erstellen | N | siehe Feature-Abhängigkeiten | Zahlungsstart im Portal |
| AP-SST-026 | REST-Endpunkt Zahlung starten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Zahlung starten |
| AP-SST-027 | Businesslogik Zahlungsanforderung/Rückmeldung umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Zahlungsanforderung/Rückmeldung |
| AP-SST-028 | Datenmodell Zahlung/Rechnungsbezug/Status erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Zahlung/Rechnungsbezug/Status |
| AP-SST-029 | Validierung Rechnung zahlbar umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Rechnung zahlbar |
| AP-SST-030 | Berechtigung Rechnung gehört Portalnutzer umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Rechnung gehört Portalnutzer |
| AP-SST-031 | Zahlungsvorgänge revisionsfähig protokollieren | N | siehe Feature-Abhängigkeiten | Zahlungsvorgänge revisionsfähig protokollieren |
| AP-SST-032 | Tests Zahlungsstart/Rückmeldung/Abbruch erstellen | T | siehe Feature-Abhängigkeiten | Tests Zahlungsstart/Rückmeldung/Abbruch |
| AP-SST-033 | Dokumentation Zahlungsprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Zahlungsprozess ergänzen |

## LSBS

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-034 | Import-/Sync-Endpunkt LSBS implementieren | I | siehe Feature-Abhängigkeiten | Import-/Sync-Endpunkt LSBS |
| AP-SST-035 | Businesslogik Mitgliederstatistik/Vereinsdaten übernehmen | N | siehe Feature-Abhängigkeiten | Businesslogik Mitgliederstatistik/Vereinsdaten übernehmen |
| AP-SST-036 | Datenmodell Verein/Mitgliedschaft/Nachweise anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Verein/Mitgliedschaft/Nachweise anbinden |
| AP-SST-037 | Datenformat/Vereinzuordnung validieren | N | siehe Feature-Abhängigkeiten | Datenformat/Vereinzuordnung validieren |
| AP-SST-038 | technische Berechtigung umsetzen | N | siehe Feature-Abhängigkeiten | technische Berechtigung |
| AP-SST-039 | Importprotokoll erstellen | N | siehe Feature-Abhängigkeiten | Importprotokoll |
| AP-SST-040 | Tests Import gültig/ungültig erstellen | T | siehe Feature-Abhängigkeiten | Tests Import gültig/ungültig |
| AP-SST-041 | Dokumentation Importformat ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Importformat ergänzen |

## Formcycle

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-042 | Import-/Übergabeschnittstelle Formcycle implementieren | I | siehe Feature-Abhängigkeiten | Import-/Übergabeschnittstelle Formcycle |
| AP-SST-043 | Businesslogik Formulardaten in Antrag/Wizard überführen | N | siehe Feature-Abhängigkeiten | Businesslogik Formulardaten in Antrag/Wizard überführen |
| AP-SST-044 | Datenmodell Antrag/Feldwerte/Anhänge anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Antrag/Feldwerte/Anhänge anbinden |
| AP-SST-045 | Mapping/Pflichtfelder validieren | N | siehe Feature-Abhängigkeiten | Mapping/Pflichtfelder validieren |
| AP-SST-046 | technische Berechtigung umsetzen | N | siehe Feature-Abhängigkeiten | technische Berechtigung |
| AP-SST-047 | Eingang protokollieren | N | siehe Feature-Abhängigkeiten | Eingang protokollieren |
| AP-SST-048 | Tests Datenübernahme erstellen | T | siehe Feature-Abhängigkeiten | Tests Datenübernahme |
| AP-SST-049 | Dokumentation Mappingbeschreibung ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Mappingbeschreibung ergänzen |

## Cardo

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-050 | Geodatenadapter Cardo implementieren | I | siehe Feature-Abhängigkeiten | Geodatenadapter Cardo |
| AP-SST-051 | Businesslogik Geodaten abrufen/darstellen | N | siehe Feature-Abhängigkeiten | Businesslogik Geodaten abrufen/darstellen |
| AP-SST-052 | Georeferenz anbinden | I | siehe Feature-Abhängigkeiten | Georeferenz anbinden |
| AP-SST-053 | Koordinaten validieren | N | siehe Feature-Abhängigkeiten | Koordinaten validieren |
| AP-SST-054 | Schnittstellenberechtigung klären | I | siehe Feature-Abhängigkeiten | Schnittstellenberechtigung klären |
| AP-SST-055 | Abrufe optional protokollieren | N | siehe Feature-Abhängigkeiten | Abrufe optional protokollieren |
| AP-SST-056 | Tests Geodatenanzeige erstellen | T | siehe Feature-Abhängigkeiten | Tests Geodatenanzeige |
| AP-SST-057 | Dokumentation Schnittstelle ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Schnittstelle ergänzen |

## Schuldatenbank

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SST-058 | Import-/Abgleichschnittstelle Schuldatenbank implementieren | N | siehe Feature-Abhängigkeiten | Import-/Abgleichschnittstelle Schuldatenbank |
| AP-SST-059 | Businesslogik Schuldaten/Schulsportbezug/Ansprechpartner übernehmen | N | siehe Feature-Abhängigkeiten | Businesslogik Schuldaten/Schulsportbezug/Ansprechpartner übernehmen |
| AP-SST-060 | Datenmodell Schulen/Ansprechpartner/Sportanlagenbezug anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Schulen/Ansprechpartner/Sportanlagenbezug anbinden |
| AP-SST-061 | Schul-ID/Zuordnung validieren | N | siehe Feature-Abhängigkeiten | Schul-ID/Zuordnung validieren |
| AP-SST-062 | technische Berechtigung umsetzen | N | siehe Feature-Abhängigkeiten | technische Berechtigung |
| AP-SST-063 | Importprotokoll erstellen | N | siehe Feature-Abhängigkeiten | Importprotokoll |
| AP-SST-064 | Tests Datenabgleich erstellen | T | siehe Feature-Abhängigkeiten | Tests Datenabgleich |
| AP-SST-065 | Dokumentation Importregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Importregeln ergänzen |
