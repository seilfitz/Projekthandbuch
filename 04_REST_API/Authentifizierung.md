# Authentifizierung

## Ziel dieses Dokuments

Dieses Dokument beschreibt die Authentifizierung der SportFM-REST-API.

Die Authentifizierung ist eine Plattformfunktion und gilt für das SportPortal, die schrittweise WPF-Migration und spätere REST-Clients.

## Grundsatz

Die REST-API darf geschützte Daten nur nach erfolgreicher Authentifizierung und Berechtigungsprüfung bereitstellen.

Das Portal führt keine verbindliche Sicherheitsentscheidung aus.

Verbindlich entscheidet die REST-API.

## Abgrenzung

Dieses Kapitel beschreibt:

- Registrierung
- Anmeldung
- Token
- Aktualisierungstoken
- Abmeldung
- Passwort zurücksetzen
- Zwei-Faktor-Authentifizierung
- OAuth für Fremdsysteme
- Benutzerkontext

Nicht Bestandteil dieses Kapitels:

- fachliche Rollen im Detail
- vollständiges Berechtigungsmodell
- Mandantentrennung im Detail
- Benutzeroberfläche des Portals

Diese Themen werden in `Berechtigungen.md` und den Portal-Kapiteln beschrieben.

## Authentifizierungsarten V1

| Verfahren | Zweck | V1 |
|---|---|:---:|
| E-Mail und Passwort | Portalbenutzer anmelden | ja |
| Zwei-Faktor-Authentifizierung | zusätzliche Absicherung | ja |
| Aktualisierungstoken | Sitzung erneuern | ja |
| OAuth Client Credentials | technische Fremdsysteme | ja |
| BundID | optionale spätere Erweiterung | offen |

## Grundlegender Ablauf

```text
Benutzer
  ↓
Login im Portal
  ↓
REST-API prüft Zugangsdaten
  ↓
REST-API erzeugt Zugriffstoken
  ↓
Portal ruft geschützte Endpunkte mit Zugriffstoken auf
  ↓
REST-API prüft Token, Kontext und Berechtigung
```

## Registrierung

Portalbenutzer können sich registrieren.

Geplanter V1-Ablauf:

```text
Registrierung
  ↓
E-Mail-Bestätigung
  ↓
Benutzer zunächst nicht vollständig freigegeben
  ↓
Zuordnung zu Organisation / SportFM-Nutzer
  ↓
Freischaltung
  ↓
Nutzung geschützter Funktionen
```

## Registrierungsdaten

```text
E-Mail
Passwort
Vorname
Nachname
Telefon optional
Organisation / Verein optional
Begründung / Zuordnungswunsch optional
```

Die finale Feldliste wird im Portal- und Datenmodellkapitel festgelegt.

## Anmeldeablauf

```text
POST /auth/login
```

Eingabe:

```text
E-Mail
Passwort
```

Ergebnis:

```text
Zugriffstoken
Aktualisierungstoken
Ablaufzeitpunkt
Benutzerinformationen
verfügbare Kontexte
```

## Zwei-Faktor-Authentifizierung

Für V1 ist Zwei-Faktor-Authentifizierung vorgesehen.

Bevorzugtes Verfahren:

```text
TOTP
```

Beispiele:

- Authenticator-App
- kompatible TOTP-App

### Ablauf Einrichtung

```text
Benutzer ist angemeldet
  ↓
2FA-Einrichtung starten
  ↓
Secret erzeugen
  ↓
QR-Code anzeigen
  ↓
Code prüfen
  ↓
2FA aktivieren
```

### Ablauf Anmeldung mit 2FA

```text
E-Mail und Passwort korrekt
  ↓
REST-API fordert 2FA-Code
  ↓
Benutzer gibt Code ein
  ↓
REST-API prüft Code
  ↓
Token wird ausgegeben
```

## Token-Konzept

### Zugriffstoken

Das Zugriffstoken wird bei geschützten REST-Aufrufen mitgegeben.

```text
Authorization: Bearer <token>
```

Eigenschaften:

| Eigenschaft | Beschreibung |
|---|---|
| kurze Laufzeit | reduziert Risiko bei Verlust |
| enthält Benutzerkennung | technische Identifikation |
| enthält Rollen / Claims | Grundlage für Berechtigungsprüfung |
| enthält aktiven Kontext | Organisation / SportFM-Nutzer |
| nicht dauerhaft speichern | nur für laufende Sitzung |

### Aktualisierungstoken

Das Aktualisierungstoken dient zur Erneuerung eines Zugriffstokens.

Eigenschaften:

| Eigenschaft | Beschreibung |
|---|---|
| längere Laufzeit | erlaubt Sitzungserneuerung |
| widerrufbar | Abmeldung oder Sperrung beendet Nutzung |
| serverseitig gespeichert | Prüfung gegen Datenbank |
| gerätebezogen möglich | spätere Erweiterung |

## Kontext

Ein Benutzer kann in einem oder mehreren fachlichen Kontexten handeln.

Beispiel:

```text
Portalbenutzer
  ↓
Organisation / Verein / Schule / Firma
  ↓
SportFM-Nutzer
  ↓
Rolle
```

Der aktive Kontext bestimmt, welche Daten der Benutzer sehen und welche Funktionen er nutzen darf.

## Claims

Das Zugriffstoken soll nur notwendige Informationen enthalten.

Mögliche Claims:

```text
BenutzerId
E-Mail
Rollen
KontextId
OrganisationId
SportFmNutzerId
TokenVersion
```

Keine sensiblen Daten im Token:

- Passwort
- 2FA-Secret
- vollständige personenbezogene Stammdaten
- interne technische Details

## Abmeldung

```text
POST /auth/logout
```

Bei Abmeldung wird das aktuelle Aktualisierungstoken ungültig gemacht.

Das Zugriffstoken läuft spätestens mit Ablaufzeit aus.

## Passwort zurücksetzen

Ablauf:

```text
Benutzer fordert Passwort-Zurücksetzen an
  ↓
REST-API erzeugt zeitlich begrenzten Token
  ↓
E-Mail wird versendet
  ↓
Benutzer setzt neues Passwort
  ↓
alte Sitzungen können ungültig gemacht werden
```

Endpunkte:

| Methode | Pfad | Zweck |
|---|---|---|
| POST | `/auth/password-reset/request` | Passwort-Zurücksetzen anfordern |
| POST | `/auth/password-reset/confirm` | neues Passwort setzen |

## Benutzerfreischaltung

Nach Registrierung kann eine fachliche Freischaltung erforderlich sein.

Gründe:

- Zuordnung zu bestehendem SportFM-Nutzer
- Prüfung der Vertretungsberechtigung
- Zuordnung zu Rollen
- Schutz vor unberechtigtem Zugriff auf Vereins-, Rechnungs- und Dokumentdaten

Die Freischaltung ist eine Administrationsfunktion.

## OAuth für Fremdsysteme

Für technische Clients ist OAuth vorgesehen.

Zweck:

- Fremdsysteme anbinden
- technische Zugriffe absichern
- keine Benutzerpasswörter für System-zu-System-Zugriffe verwenden

V1-Verfahren:

```text
OAuth Client Credentials
```

Beispiele für spätere technische Clients:

- Hallendisplay
- externe Schnittstellen
- Verwaltungsdienste

## BundID

BundID ist als mögliche spätere Erweiterung vorgesehen.

Für V1 wird BundID nur berücksichtigt, soweit dadurch keine Architektur blockiert wird.

Offen bleibt:

- Zeitpunkt der Umsetzung
- technische Anbindung
- fachliche Zuordnung zu Portalbenutzern
- Umgang mit bestehenden Portalbenutzern

## REST-Endpunkte

| Methode | Pfad | Zweck | V1 |
|---|---|---|:---:|
| POST | `/auth/register` | Benutzer registrieren | ja |
| POST | `/auth/login` | Benutzer anmelden | ja |
| POST | `/auth/refresh` | Zugriffstoken erneuern | ja |
| POST | `/auth/logout` | Sitzung beenden | ja |
| POST | `/auth/password-reset/request` | Passwort-Zurücksetzen anfordern | ja |
| POST | `/auth/password-reset/confirm` | neues Passwort setzen | ja |
| POST | `/auth/2fa/setup` | 2FA einrichten | ja |
| POST | `/auth/2fa/verify` | 2FA-Code prüfen | ja |
| GET | `/auth/me` | aktuelle Benutzerdaten laden | ja |
| GET | `/auth/contexts` | verfügbare Kontexte laden | ja |
| POST | `/auth/context` | aktiven Kontext wechseln | ja |

## Datenhaltung V1

Für die Authentifizierung sind neue Portaltabellen vorgesehen.

Bereits als V1-Struktur benannt:

```text
LHD_SPA_PORT_USER
LHD_SPA_PORT_REFRESH_TOKEN
LHD_SPA_PORT_PASSWORD_RESET
```

Weitere Tabellen für Organisation, Mitgliedschaften und Kontext werden im Datenmodell dokumentiert.

## Sicherheitsgrundsätze

| Nr. | Grundsatz |
|---:|---|
| AUTH-001 | Passwörter werden niemals im Klartext gespeichert. |
| AUTH-002 | Passwort-Hashes verwenden ein geeignetes modernes Hashverfahren. |
| AUTH-003 | Tokens sind zeitlich begrenzt. |
| AUTH-004 | Aktualisierungstoken sind serverseitig widerrufbar. |
| AUTH-005 | 2FA-Secrets werden geschützt gespeichert. |
| AUTH-006 | Fehlermeldungen dürfen keine Benutzerexistenz verraten. |
| AUTH-007 | Zugriff auf Fachobjekte wird zusätzlich über Berechtigungen geprüft. |
| AUTH-008 | Sperrung eines Benutzers verhindert neue Token. |

## Fehlerbehandlung

Authentifizierungsfehler verwenden das einheitliche Fehlerformat aus `Fehlerformat.md`.

Beispiele:

| Fehlercode | Bedeutung |
|---|---|
| AUTH_ANMELDUNG_FEHLGESCHLAGEN | Anmeldung nicht erfolgreich |
| AUTH_TOKEN_UNGUELTIG | Zugriffstoken ist ungültig |
| AUTH_TOKEN_ABGELAUFEN | Zugriffstoken ist abgelaufen |
| AUTH_2FA_ERFORDERLICH | Zwei-Faktor-Code erforderlich |
| AUTH_2FA_UNGUELTIG | Zwei-Faktor-Code ungültig |
| AUTH_BENUTZER_GESPERRT | Benutzer ist gesperrt |
| AUTH_KONTEXT_UNGUELTIG | Kontext ist nicht zulässig |

## Nicht-Ziele

Nicht Bestandteil dieses Kapitels:

- vollständige Benutzerverwaltung im Portal
- fachliche Rollenmatrix
- OpenID-Connect-Detailkonzept für BundID
- Ablösung der bestehenden SportFM-Benutzerverwaltung
- direkte Nutzung von SportFM-WPF-Benutzern für Portalzugriffe

## Offene Punkte

| ID | Frage |
|---|---|
| OF-AUTH-001 | finale Passwortregeln festlegen |
| OF-AUTH-002 | Laufzeiten für Zugriffstoken und Aktualisierungstoken festlegen |
| OF-AUTH-003 | 2FA-Pflicht für alle Benutzer oder nur bestimmte Rollen festlegen |
| OF-AUTH-004 | genaue Freischaltprozesse definieren |
| OF-AUTH-005 | BundID-Umfang und Zeitpunkt festlegen |
| OF-AUTH-006 | technische OAuth-Clients für V1 konkret benennen |

## Nächster Schritt

Nach diesem Kapitel werden `Berechtigungen.md` und das Datenmodell für Portalbenutzer, Organisationen, Rollen und Kontexte weiter konkretisiert.