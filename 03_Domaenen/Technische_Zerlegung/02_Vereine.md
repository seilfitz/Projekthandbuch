# Technische Zerlegung – Vereine und Verbände

| Dokument | 03_Domaenen/Technische_Zerlegung/02_Vereine.md |
|----------|-------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Vereine/Verbände verwaltet organisationsbezogene Daten im Portal und stellt die Verbindung zu SportFM-Nutzern her. Im Portal können mehrere Ansprechpartner und berechtigte Personen einem SportFM-Nutzer zugeordnet werden.

---

# 2 Features

## FUN-VER-001 Verein anlegen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Anlageformular Verein/Organisation |
| API | POST `/api/vereine` |
| BL | Organisation anlegen, SportFM-Nutzer-Zuordnung vorbereiten |
| DB | Portalorganisation, Zuordnung SportFM-Nutzer |
| VAL | Name, Rechtsform, E-Mail, Dublettenprüfung |
| BER | Sachbearbeitung / Admin |
| LOG | Anlage protokollieren |
| TEST | gültige Anlage, Dublette |
| DOK | Vereinsanlage |

**Abhängigkeiten:** Benutzer, Stammdaten Nutzer  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-VER-002 Verein bearbeiten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Vereinsprofil bearbeiten |
| API | PUT `/api/vereine/{id}` |
| BL | Stammdaten ändern, prüfpflichtige Felder markieren |
| DB | Organisation, Änderungsstatus |
| VAL | Pflichtfelder, Plausibilität |
| BER | Vereinsadministrator / Sachbearbeitung |
| LOG | Änderungen historisieren |
| TEST | erlaubte/verbotene Änderung |
| DOK | Bearbeitungsregeln |

**Abhängigkeiten:** FUN-VER-001  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-VER-003 Ansprechpartner verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Ansprechpartnerliste |
| API | GET/POST/PUT/DELETE `/api/vereine/{id}/ansprechpartner` |
| BL | Ansprechpartner, Funktionen, Kontaktarten |
| DB | Ansprechpartner, Kontaktinformationen |
| VAL | E-Mail, Telefonnummer, Pflichtrollen |
| BER | Vereinsadministrator / Sachbearbeitung |
| LOG | Änderungen protokollieren |
| TEST | hinzufügen, ändern, entfernen |
| DOK | Ansprechpartnermodell |

**Abhängigkeiten:** FUN-VER-001, FUN-BEN-001  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-VER-004 Mitgliedschaften verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Mitgliedschaftsdaten anzeigen/pflegen |
| API | GET/PUT `/api/vereine/{id}/mitgliedschaften` |
| BL | Verbandsmitgliedschaften, LSBS-Bezug |
| DB | Mitgliedschaft, Verband |
| VAL | Zeitraum, Verband gültig |
| BER | Sachbearbeitung / Verein eingeschränkt |
| LOG | Änderungen protokollieren |
| SCH | LSBS optional |
| TEST | manuelle Pflege, Schnittstellenimport |
| DOK | Datenherkunft dokumentieren |

**Abhängigkeiten:** LSBS-Schnittstelle optional  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-VER-005 Vereinsdokumente verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Dokumentenbereich Verein |
| API | GET/POST `/api/vereine/{id}/dokumente` |
| BL | Dokumente hochladen, typisieren, freigeben |
| DB | Dokument, Dokumenttyp, Bezug Organisation |
| VAL | Dateityp, Größe, Pflichtdokumente |
| BER | Verein / Sachbearbeitung |
| LOG | Upload und Download protokollieren |
| TEST | Upload, Download, Rechteprüfung |
| DOK | Dokumenttypen |

**Abhängigkeiten:** Dokumentendomäne  
**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-VER-006 Vereinssuche

| Baustein | Zerlegung |
|----------|-----------|
| FE | Suchmaske / Ergebnisliste |
| API | GET `/api/vereine` |
| BL | Filterung, Sortierung, Paging |
| DB | Suchindex / Organisation |
| VAL | Filterparameter |
| BER | öffentliche oder interne Sicht regeln |
| LOG | optional Suchprotokoll |
| TEST | Filter, Paging, Berechtigungen |
| DOK | Suchparameter |

**Abhängigkeiten:** FUN-VER-001  
**Risiko:** Niedrig  
**Komplexität:** S
