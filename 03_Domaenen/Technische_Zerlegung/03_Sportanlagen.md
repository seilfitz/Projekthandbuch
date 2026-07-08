# Technische Zerlegung – Sportanlagen

| Dokument | 03_Domaenen/Technische_Zerlegung/03_Sportanlagen.md |
|----------|------------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Sportanlagen stellt Stammdaten, Sportflächen, Ausstattung, Karteninformationen und Verfügbarkeiten bereit. Sie ist Grundlage für freie Zeiten, Reservierung, Buchung, Portal und Themenstadtplan.

---

# 2 Features

## FUN-ANL-001 Sportanlage anlegen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Anlageformular Sportanlage |
| API | POST `/api/sportanlagen` |
| BL | Sportanlage anlegen, RIB-FM-Bezug herstellen |
| DB | Sportanlage, Adresse, Betreiber |
| VAL | Pflichtfelder, Dubletten |
| BER | Sachbearbeitung / Admin |
| LOG | Anlage protokollieren |
| SCH | RIB FM |
| TEST | Anlage, Dublette |
| DOK | Stammdatenmodell |

**Abhängigkeiten:** RIB FM  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-ANL-002 Sportanlage bearbeiten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Bearbeitungsmaske |
| API | PUT `/api/sportanlagen/{id}` |
| BL | Stammdatenänderung, Freigabe für Portal |
| DB | Sportanlage |
| VAL | Pflichtfelder, Status |
| BER | Sachbearbeitung |
| LOG | Änderung protokollieren |
| TEST | Änderung, Rechte |
| DOK | Änderungsregeln |

**Abhängigkeiten:** FUN-ANL-001  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-ANL-003 Sportflächen verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Sportflächenverwaltung |
| API | GET/POST/PUT `/api/sportanlagen/{id}/sportflaechen` |
| BL | Teileinheiten, Flächen, Nutzbarkeit |
| DB | Sportfläche, Teileinheit |
| VAL | Flächenbezeichnung, Zuordnung |
| BER | Sachbearbeitung |
| LOG | Änderungen protokollieren |
| TEST | Fläche hinzufügen, ändern, deaktivieren |
| DOK | Flächenmodell |

**Abhängigkeiten:** FUN-ANL-001  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-ANL-004 Ausstattung verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Ausstattungsliste |
| API | GET/PUT `/api/sportanlagen/{id}/ausstattung` |
| BL | Ausstattungskategorien, Merkmale |
| DB | Ausstattung, Zuordnung Sportanlage/Sportfläche |
| VAL | Kategorie gültig |
| BER | Sachbearbeitung |
| LOG | Änderung protokollieren |
| TEST | Ausstattung pflegen |
| DOK | Ausstattungskatalog |

**Abhängigkeiten:** FUN-ANL-003  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-ANL-005 Bilder verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Bildgalerie / Upload |
| API | GET/POST/DELETE `/api/sportanlagen/{id}/bilder` |
| BL | Bilder speichern, sortieren, Portal-Freigabe |
| DB | Dokument/Bild, Metadaten |
| VAL | Dateityp, Größe |
| BER | Sachbearbeitung |
| LOG | Upload/Löschung protokollieren |
| TEST | Upload, Löschung, Anzeige |
| DOK | Medienregeln |

**Abhängigkeiten:** Dokumente/Upload  
**Risiko:** Niedrig  
**Komplexität:** M

---

## FUN-ANL-006 Kartenposition verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Kartenposition anzeigen/bearbeiten |
| API | GET/PUT `/api/sportanlagen/{id}/kartenposition` |
| BL | Koordinaten, TSP-ID, Portalverlinkung |
| DB | Geo-Koordinaten, Kartenreferenz |
| VAL | Koordinatenformat |
| BER | Sachbearbeitung |
| LOG | Änderung protokollieren |
| SCH | Themenstadtplan / Cardo |
| TEST | Kartenanzeige, Link |
| DOK | Schnittstellenbezug |

**Abhängigkeiten:** Themenstadtplan  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-ANL-007 Verfügbarkeiten anzeigen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Verfügbarkeitsanzeige |
| API | GET `/api/sportanlagen/{id}/verfuegbarkeiten` |
| BL | Abfrage Occurrence/Winner, freie Zeitfenster |
| DB | Events, Occurrences, Winner |
| VAL | Zeitraum, Filter |
| BER | öffentliche/interne Sicht |
| LOG | optional Suchprotokoll |
| TEST | freie/belegte Zeiten |
| DOK | Suchlogik |

**Abhängigkeiten:** Reservierung, Buchung, Occurrence/Winner  
**Risiko:** Hoch  
**Komplexität:** XL
