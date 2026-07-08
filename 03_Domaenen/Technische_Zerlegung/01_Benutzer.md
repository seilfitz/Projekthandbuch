# Technische Zerlegung – Benutzer

| Dokument | 03_Domaenen/Technische_Zerlegung/01_Benutzer.md |
|----------|--------------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne Benutzer umfasst Portalbenutzer, Anmeldung, Rollen, Berechtigungen und Benutzerverwaltung. Sie bildet die Sicherheitsgrundlage für Portal, REST-API und spätere Fremdzugriffe.

---

# 2 Features

## FUN-BEN-001 Benutzer anlegen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Administrationsmaske Benutzer anlegen |
| API | POST `/api/benutzer` |
| BL | Anlage Portalbenutzer, Zuordnung zu Organisation/Nutzer |
| DB | Tabellen für Portalbenutzer, Login, Status |
| VAL | Pflichtfelder, E-Mail-Eindeutigkeit, Passwortregeln |
| BER | Nur berechtigte Administratoren |
| LOG | Benutzeranlage protokollieren |
| TEST | Anlage gültig/ungültig, doppelte E-Mail |
| DOK | Benutzeranlage beschreiben |

**Abhängigkeiten:** Rollenmodell, Organisation/Nutzer  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-BEN-002 Benutzer bearbeiten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Benutzerbearbeitung |
| API | PUT `/api/benutzer/{id}` |
| BL | Änderung Stammdaten, Status, Zuordnung |
| DB | Update Benutzerfelder |
| VAL | E-Mail, Pflichtfelder, Statuswechsel |
| BER | Admin oder berechtigter Organisationsverwalter |
| LOG | Änderungen historisieren |
| TEST | Änderung Daten, unzulässige Änderung |
| DOK | Bearbeitungsregeln |

**Abhängigkeiten:** FUN-BEN-001  
**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-BEN-003 Benutzer deaktivieren

| Baustein | Zerlegung |
|----------|-----------|
| FE | Benutzer deaktivieren / reaktivieren |
| API | PATCH `/api/benutzer/{id}/status` |
| BL | Sperrung, Reaktivierung, Sitzungen beenden |
| DB | Statusfeld Benutzer |
| VAL | Selbstdeaktivierung verhindern |
| BER | Administrator |
| LOG | Statuswechsel protokollieren |
| TEST | Deaktivierung, Login blockiert |
| DOK | Statusmodell |

**Abhängigkeiten:** FUN-BEN-001  
**Risiko:** Mittel  
**Komplexität:** S

---

## FUN-BEN-004 Rollen verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Rollenverwaltung |
| API | GET/POST/PUT `/api/rollen` |
| BL | Rollen anlegen, ändern, zuordnen |
| DB | Rollen, Benutzer-Rollen-Zuordnung |
| VAL | Rollennamen eindeutig |
| BER | Systemadministrator |
| LOG | Rollenänderungen protokollieren |
| TEST | Rollenzuweisung, Zugriffstest |
| DOK | Rollenmodell |

**Abhängigkeiten:** Berechtigungskonzept  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-BEN-005 Berechtigungen verwalten

| Baustein | Zerlegung |
|----------|-----------|
| FE | Berechtigungsübersicht |
| API | GET/PUT `/api/berechtigungen` |
| BL | Rechteprüfung je Domäne/Funktion |
| DB | Rollenrechte, Funktionsrechte |
| VAL | Pflichtrechte, Systemrechte nicht löschbar |
| BER | Systemadministrator |
| LOG | Rechteänderungen revisionsfähig |
| TEST | positive/negative Rechteprüfung |
| DOK | Berechtigungsmatrix |

**Abhängigkeiten:** FUN-BEN-004  
**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-BEN-006 Passwort zurücksetzen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Passwort-vergessen-Dialog |
| API | POST `/api/auth/passwort-zuruecksetzen` |
| BL | Token erzeugen, Mail senden, neues Passwort setzen |
| DB | Reset-Token, Ablaufzeit |
| VAL | Token gültig, Passwortregeln |
| BER | öffentlich, tokenbasiert |
| LOG | Sicherheitsereignis protokollieren |
| TEST | gültiger/abgelaufener Token |
| DOK | Passwortprozess |

**Abhängigkeiten:** Mailversand  
**Risiko:** Mittel  
**Komplexität:** M
