# Technische Zerlegung – Dokumente

| Dokument | 03_Domaenen/Technische_Zerlegung/07_Dokumente.md |
|----------|---------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Dokumente umfasst Vorlagen, Dokumentenerzeugung, PDF-Ausgabe, Portalbereitstellung, Archivierung und Download. Bestehende SportFM-Dokumentenstrukturen werden weiterverwendet.

---

# 2 Features

## FUN-DOK-001 Bescheid erzeugen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Aktion Bescheid erzeugen |
| API | POST `/api/dokumente/bescheid` |
| BL | Vorlage auswählen, Daten mappen, Dokument erzeugen |
| DB | LHD_SPA_DOCUMENTS, Templates |
| VAL | Buchung/Antrag vollständig |
| BER | Sachbearbeitung |
| LOG | Erzeugung protokollieren |
| TEST | Bescheid aus Buchung |
| DOK | Vorlagenbezug |

**Abhängigkeiten:** Buchung/Antrag, Vorlagen  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-DOK-002 Gebührenbescheid

| Baustein | Zerlegung |
|----------|-----------|
| FE | Gebührenbescheid erzeugen/anzeigen |
| API | POST `/api/dokumente/gebuehrenbescheid` |
| BL | Gebühreninformationen übernehmen |
| DB | LHD_SPA_DOCUMENTS, Gebühren |
| VAL | Gebühren berechnet |
| BER | Sachbearbeitung |
| LOG | Erzeugung protokollieren |
| TEST | Gebührenbescheid mit Beträgen |
| DOK | Gebührenbescheidprozess |

**Abhängigkeiten:** Gebührenberechnung  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-DOK-003 Widerrufsbescheid

| Baustein | Zerlegung |
|----------|-----------|
| FE | Widerrufsbescheid erzeugen |
| API | POST `/api/dokumente/widerruf` |
| BL | Widerrufsgrund, betroffene Buchungen |
| DB | LHD_SPA_DOCUMENTS, Events |
| VAL | Widerruf vorhanden |
| BER | Sachbearbeitung |
| LOG | Erzeugung protokollieren |
| TEST | Widerruf erzeugt Dokument |
| DOK | Widerrufsdokumente |

**Abhängigkeiten:** Widerruf/Stornierung  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-DOK-004 PDF erzeugen

| Baustein | Zerlegung |
|----------|-----------|
| FE | PDF-Vorschau/Download |
| API | GET `/api/dokumente/{id}/pdf` |
| BL | PDF erzeugen oder vorhandenes PDF liefern |
| DB | Dokumentinhalt, MimeType |
| VAL | Dokumentstatus |
| BER | Nutzerbezogener Zugriff |
| LOG | Download protokollieren |
| TEST | PDF abrufen, Rechteprüfung |
| DOK | PDF-Ausgabe |

**Abhängigkeiten:** Dokumentenspeicherung  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-DOK-005 Dokument archivieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Archivstatus anzeigen |
| API | PATCH `/api/dokumente/{id}/archivieren` |
| BL | Archivstatus setzen, Unveränderbarkeit beachten |
| DB | Dokumentstatus, Archivdatum |
| VAL | Status zulässig |
| BER | Sachbearbeitung/Admin |
| LOG | Archivierung protokollieren |
| TEST | Archivierung, gesperrtes Dokument |
| DOK | Archivregeln |

**Abhängigkeiten:** Dokumentstatus  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-DOK-006 Dokument suchen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Dokumentensuche |
| API | GET `/api/dokumente` |
| BL | Suche nach Nutzer, Buchung, Rechnung, Zeitraum |
| DB | LHD_SPA_DOCUMENTS |
| VAL | Filterparameter |
| BER | Benutzer sieht nur eigene Dokumente |
| LOG | optional |
| TEST | Suche, Berechtigung |
| DOK | Suchparameter |

**Abhängigkeiten:** Berechtigungen  
**Risiko:** Mittel  
**Komplexität:** M
