# Technische Zerlegung – Portal

| Dokument | 03_Domaenen/Technische_Zerlegung/09_Portal.md |
|----------|----------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Portal umfasst die sichtbaren Funktionen des Onlineportals. Das Portal dient als Client der SportFM-Plattform und enthält keine eigene Fachlogik für Buchungen, Gebühren oder Winner-Berechnung.

---

# 2 Features

## FUN-POR-001 Registrierung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Registrierungsformular |
| API | POST `/api/auth/registrierung` |
| BL | Registrierung, Mailbestätigung, Freischaltung |
| DB | Portalbenutzer, Registrierungsstatus |
| VAL | E-Mail, Passwort, Pflichtdaten |
| BER | öffentlich |
| LOG | Registrierung protokollieren |
| TEST | Registrierung, Dublette |
| DOK | Registrierungsprozess |

**Abhängigkeiten:** Benutzer, Mailversand  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-POR-002 Anmeldung

| Baustein | Zerlegung |
|----------|-----------|
| FE | Loginseite |
| API | POST `/api/auth/anmeldung` |
| BL | Anmeldung, Token, 2FA optional |
| DB | Benutzer, Loginstatus |
| VAL | Zugangsdaten |
| BER | öffentlich |
| LOG | Loginversuche protokollieren |
| TEST | erfolgreicher/fehlgeschlagener Login |
| DOK | Anmeldeprozess |

**Abhängigkeiten:** Benutzer, Authentifizierung  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-POR-003 Freie Zeiten suchen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Suchmaske freie Zeiten |
| API | GET `/api/freie-zeiten` |
| BL | Suche an Availability/Occurrence delegieren |
| DB | LHD_SPA_OCC, LHD_SPA_OCC_WINNER, Sportanlagen |
| VAL | Zeitraum, Uhrzeit, Sportart |
| BER | öffentlich oder angemeldet je Sicht |
| LOG | Suchanfragen optional |
| TEST | freie Zeiten, belegte Zeiten |
| DOK | Suchparameter |

**Abhängigkeiten:** Sportanlagen, Buchung, Occurrence/Winner  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-POR-004 Sportanlagen anzeigen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Sportanlagenliste / Detailseite |
| API | GET `/api/sportanlagen` |
| BL | Portalgeeignete Sportanlagendaten liefern |
| DB | Sportanlagen, Ausstattung, Bilder |
| VAL | Filterparameter |
| BER | öffentlich |
| LOG | optional |
| TEST | Liste, Detail, Filter |
| DOK | Portalansicht Sportanlagen |

**Abhängigkeiten:** Sportanlagen  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-POR-005 Vereinsdaten anzeigen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Vereinsprofil |
| API | GET `/api/vereine/{id}` |
| BL | sichtbare Vereinsdaten bereitstellen |
| DB | Verein, Ansprechpartner, Sportarten |
| VAL | Sichtbarkeit |
| BER | öffentlich/angemeldet je Datenfeld |
| LOG | optional |
| TEST | öffentliche/private Felder |
| DOK | Sichtbarkeitsregeln |

**Abhängigkeiten:** Vereine  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-POR-006 Veranstaltungen anzeigen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Veranstaltungskalender |
| API | GET `/api/veranstaltungen` |
| BL | öffentliche Veranstaltungen bereitstellen |
| DB | Events, Eventtyp Veranstaltung |
| VAL | Zeitraum, Freigabe |
| BER | öffentlich |
| LOG | optional |
| TEST | Kalenderanzeige |
| DOK | Veranstaltungsportal |

**Abhängigkeiten:** Veranstaltungen  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-POR-007 Online-Antrag

| Baustein | Zerlegung |
|----------|-----------|
| FE | Wizard Runtime |
| API | `/api/antraege`, `/api/wizard` |
| BL | Antragstyp laden, Antrag speichern, einreichen |
| DB | Antrag, Wizard-Konfiguration |
| VAL | dynamische Validierung |
| BER | angemeldeter Portalnutzer |
| LOG | Antragsschritte protokollieren |
| TEST | vollständiger Online-Antrag |
| DOK | Antragstellung Portal |

**Abhängigkeiten:** Antrag/Wizard, Benutzer  
**Risiko:** Hoch  
**Komplexität:** XXL

---

## FUN-POR-008 Benutzerkonto

| Baustein | Zerlegung |
|----------|-----------|
| FE | Kontoübersicht |
| API | GET/PUT `/api/mein-konto` |
| BL | Profil, Organisation, Dokumente, Rechnungen |
| DB | Portalbenutzer, Organisation |
| VAL | Profilfelder |
| BER | angemeldeter Benutzer |
| LOG | Änderungen protokollieren |
| TEST | Profildaten, Rechte |
| DOK | Kontoübersicht |

**Abhängigkeiten:** Benutzer, Vereine, Dokumente, Rechnungen  
**Risiko:** Mittel  
**Komplexität:** L
