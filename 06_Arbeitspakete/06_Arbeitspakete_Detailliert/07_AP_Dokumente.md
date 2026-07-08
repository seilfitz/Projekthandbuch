# Arbeitspakete – Dokumente

| Dokument | 06_Arbeitspakete/DOK_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Dokumente**.

---

# 2 Arbeitspakete

## Bescheid erzeugen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-001 | Oberfläche Bescheid erzeugen erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Bescheid erzeugen |
| AP-DOK-002 | REST-Endpunkt Bescheid implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Bescheid |
| AP-DOK-003 | Businesslogik Vorlage auswählen/Daten mappen/Dokument erzeugen | D | siehe Feature-Abhängigkeiten | Businesslogik Vorlage auswählen/Daten mappen/Dokument erzeugen |
| AP-DOK-004 | Datenzugriff LHD_SPA_DOCUMENTS/Templates anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_DOCUMENTS/Templates anbinden |
| AP-DOK-005 | Validierung Buchung/Antrag vollständig umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Buchung/Antrag vollständig |
| AP-DOK-006 | Berechtigung Sachbearbeitung umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung |
| AP-DOK-007 | Erzeugung protokollieren | N | siehe Feature-Abhängigkeiten | Erzeugung protokollieren |
| AP-DOK-008 | Tests Bescheid aus Buchung erstellen | T | siehe Feature-Abhängigkeiten | Tests Bescheid aus Buchung |
| AP-DOK-009 | Dokumentation Vorlagenbezug ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Vorlagenbezug ergänzen |

## Gebührenbescheid

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-010 | Oberfläche Gebührenbescheid erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Gebührenbescheid |
| AP-DOK-011 | REST-Endpunkt Gebührenbescheid implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Gebührenbescheid |
| AP-DOK-012 | Businesslogik Gebühreninformationen übernehmen | N | siehe Feature-Abhängigkeiten | Businesslogik Gebühreninformationen übernehmen |
| AP-DOK-013 | Datenzugriff Dokumente/Gebühren anbinden | D | siehe Feature-Abhängigkeiten | Datenzugriff Dokumente/Gebühren anbinden |
| AP-DOK-014 | Validierung Gebühren berechnet umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Gebühren berechnet |
| AP-DOK-015 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-DOK-016 | Erzeugung protokollieren | N | siehe Feature-Abhängigkeiten | Erzeugung protokollieren |
| AP-DOK-017 | Tests Gebührenbescheid erstellen | T | siehe Feature-Abhängigkeiten | Tests Gebührenbescheid |
| AP-DOK-018 | Dokumentation Gebührenbescheidprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Gebührenbescheidprozess ergänzen |

## Widerrufsbescheid

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-019 | Oberfläche Widerrufsbescheid erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Widerrufsbescheid |
| AP-DOK-020 | REST-Endpunkt Widerrufsdokument implementieren | D | siehe Feature-Abhängigkeiten | REST-Endpunkt Widerrufsdokument |
| AP-DOK-021 | Businesslogik Widerrufsgrund/betroffene Buchungen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Widerrufsgrund/betroffene Buchungen |
| AP-DOK-022 | Datenzugriff Dokumente/Events anbinden | D | siehe Feature-Abhängigkeiten | Datenzugriff Dokumente/Events anbinden |
| AP-DOK-023 | Validierung Widerruf vorhanden umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Widerruf vorhanden |
| AP-DOK-024 | Berechtigungen umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigungen |
| AP-DOK-025 | Erzeugung protokollieren | N | siehe Feature-Abhängigkeiten | Erzeugung protokollieren |
| AP-DOK-026 | Tests Widerruf erzeugt Dokument erstellen | T | siehe Feature-Abhängigkeiten | Tests Widerruf erzeugt Dokument |
| AP-DOK-027 | Dokumentation Widerrufsdokumente ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Widerrufsdokumente ergänzen |

## PDF erzeugen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-028 | Oberfläche PDF-Vorschau/Download erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche PDF-Vorschau/Download |
| AP-DOK-029 | REST-Endpunkt PDF implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt PDF |
| AP-DOK-030 | Businesslogik PDF erzeugen/vorhandenes PDF liefern | N | siehe Feature-Abhängigkeiten | Businesslogik PDF erzeugen/vorhandenes PDF liefern |
| AP-DOK-031 | Datenzugriff Dokumentinhalt/MimeType anbinden | D | siehe Feature-Abhängigkeiten | Datenzugriff Dokumentinhalt/MimeType anbinden |
| AP-DOK-032 | Validierung Dokumentstatus umsetzen | D | siehe Feature-Abhängigkeiten | Validierung Dokumentstatus |
| AP-DOK-033 | Nutzerbezogenen Zugriff umsetzen | N | siehe Feature-Abhängigkeiten | Nutzerbezogenen Zugriff |
| AP-DOK-034 | Download protokollieren | N | siehe Feature-Abhängigkeiten | Download protokollieren |
| AP-DOK-035 | Tests PDF/Rechteprüfung erstellen | T | siehe Feature-Abhängigkeiten | Tests PDF/Rechteprüfung |
| AP-DOK-036 | Dokumentation PDF-Ausgabe ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation PDF-Ausgabe ergänzen |

## Dokument archivieren

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-037 | Oberfläche Archivstatus erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Archivstatus |
| AP-DOK-038 | REST-Endpunkt Dokument archivieren implementieren | D | siehe Feature-Abhängigkeiten | REST-Endpunkt Dokument archivieren |
| AP-DOK-039 | Businesslogik Archivstatus/Unveränderbarkeit umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Archivstatus/Unveränderbarkeit |
| AP-DOK-040 | Datenmodell Archivdatum/Status prüfen | N | siehe Feature-Abhängigkeiten | Datenmodell Archivdatum/Status prüfen |
| AP-DOK-041 | Validierung Status zulässig umsetzen | N | siehe Feature-Abhängigkeiten | Validierung Status zulässig |
| AP-DOK-042 | Berechtigung Sachbearbeitung/Admin umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Sachbearbeitung/Admin |
| AP-DOK-043 | Archivierung protokollieren | N | siehe Feature-Abhängigkeiten | Archivierung protokollieren |
| AP-DOK-044 | Tests Archivierung/gesperrtes Dokument erstellen | T | siehe Feature-Abhängigkeiten | Tests Archivierung/gesperrtes Dokument |
| AP-DOK-045 | Dokumentation Archivregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Archivregeln ergänzen |

## Dokument suchen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-DOK-046 | Oberfläche Dokumentensuche erstellen | D | siehe Feature-Abhängigkeiten | Oberfläche Dokumentensuche |
| AP-DOK-047 | REST-Endpunkt Dokumentensuche implementieren | D | siehe Feature-Abhängigkeiten | REST-Endpunkt Dokumentensuche |
| AP-DOK-048 | Businesslogik Suche nach Nutzer/Buchung/Rechnung/Zeitraum umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Suche nach Nutzer/Buchung/Rechnung/Zeitraum |
| AP-DOK-049 | Datenzugriff LHD_SPA_DOCUMENTS optimieren | N | siehe Feature-Abhängigkeiten | Datenzugriff LHD_SPA_DOCUMENTS optimieren |
| AP-DOK-050 | Filterparameter validieren | N | siehe Feature-Abhängigkeiten | Filterparameter validieren |
| AP-DOK-051 | Benutzer sieht nur eigene Dokumente umsetzen | D | siehe Feature-Abhängigkeiten | Benutzer sieht nur eigene Dokumente |
| AP-DOK-052 | Tests Suche/Berechtigung erstellen | T | siehe Feature-Abhängigkeiten | Tests Suche/Berechtigung |
| AP-DOK-053 | Dokumentation Suchparameter ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Suchparameter ergänzen |
