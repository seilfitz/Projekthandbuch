# Fehlerformat

## Ziel dieses Dokuments

Dieses Dokument beschreibt das einheitliche Fehlerformat der SportFM-REST-API.

Ziel ist eine konsistente, nachvollziehbare und sichere Fehlerbehandlung für Portal, WPF-Migration und spätere REST-Clients.

## Grundsatz

Alle REST-Endpunkte liefern Fehler in einem einheitlichen Format zurück.

Fehler dürfen keine internen Details offenlegen.

Insbesondere werden nicht ausgegeben:

- SQL-Statements
- Oracle-Fehlerstapel
- interne Package-Implementierungen
- technische Stacktraces
- Dateipfade
- sicherheitsrelevante Details

## FehlerDto

```text
FehlerDto

Fehlercode
Meldung
Details
KorrelationsId
Zeitpunkt
```

## Felder

| Feld | Beschreibung |
|---|---|
| Fehlercode | fachlicher oder technischer Fehlercode |
| Meldung | lesbare Fehlermeldung für Client oder Benutzerführung |
| Details | optionale Detailinformationen, z. B. Validierungsfehler |
| KorrelationsId | eindeutige ID zur Nachverfolgung im Logging |
| Zeitpunkt | Zeitpunkt der Fehlererzeugung |

## Beispielstruktur

```json
{
  "fehlercode": "ANTRAG_VALIDIERUNG_FEHLER",
  "meldung": "Der Antrag enthält ungültige oder fehlende Angaben.",
  "details": [
    {
      "feld": "zeitraumVon",
      "meldung": "Das Feld ist erforderlich."
    }
  ],
  "korrelationsId": "b7f2b1f0-3e71-4d0b-9b44-8e7f7f0f2a10",
  "zeitpunkt": "2026-07-08T10:15:00Z"
}
```

## Fehlerarten

| Fehlerart | Beschreibung | Beispiel |
|---|---|---|
| Validierungsfehler | Eingabedaten sind unvollständig oder ungültig | Pflichtfeld fehlt |
| Berechtigungsfehler | Benutzer darf Funktion oder Objekt nicht nutzen | fremde Rechnung |
| Authentifizierungsfehler | Benutzer ist nicht oder nicht gültig angemeldet | Token abgelaufen |
| Fachlicher Fehler | Geschäftsregel verhindert Aktion | Antrag kann nicht eingereicht werden |
| Nicht gefunden | Objekt existiert nicht oder ist nicht sichtbar | Dokument nicht gefunden |
| Konflikt | Aktion kollidiert mit aktuellem Zustand | Entwurf bereits eingereicht |
| Technischer Fehler | unerwarteter Systemfehler | Datenbank nicht erreichbar |

## HTTP-Statuscodes

| Status | Verwendung |
|---:|---|
| 200 | erfolgreiche Anfrage |
| 201 | Ressource wurde erzeugt |
| 204 | erfolgreiche Anfrage ohne Antwortinhalt |
| 400 | ungültige Anfrage oder Validierungsfehler |
| 401 | nicht authentifiziert |
| 403 | nicht berechtigt |
| 404 | Objekt nicht gefunden oder nicht sichtbar |
| 409 | fachlicher Konflikt |
| 422 | fachlich nicht verarbeitbare Anfrage |
| 500 | interner technischer Fehler |
| 503 | Dienst vorübergehend nicht verfügbar |

## Verwendung von 404

Aus Sicherheitsgründen kann die API auch dann `404` zurückgeben, wenn ein Objekt zwar existiert, der Benutzer aber keinen Zugriff darauf hat.

Dadurch wird verhindert, dass fremde IDs abgefragt werden können.

Beispiel:

```text
GET /documents/12345
```

Wenn das Dokument nicht zum berechtigten SportFM-Nutzer gehört, darf keine Aussage getroffen werden, ob das Dokument technisch existiert.

## Fehlercode-Konvention

Fehlercodes werden sprechend und domänenbezogen vergeben.

Muster:

```text
DOMAENE_FEHLERART_DETAIL
```

Beispiele:

| Fehlercode | Bedeutung |
|---|---|
| AUTH_TOKEN_UNGUELTIG | Zugriffstoken ist ungültig |
| AUTH_TOKEN_ABGELAUFEN | Zugriffstoken ist abgelaufen |
| BERECHTIGUNG_VERWEIGERT | Zugriff nicht erlaubt |
| ANTRAG_VALIDIERUNG_FEHLER | Antrag enthält ungültige Daten |
| ANTRAG_STATUS_KONFLIKT | Aktion passt nicht zum aktuellen Status |
| DOKUMENT_NICHT_GEFUNDEN | Dokument nicht vorhanden oder nicht sichtbar |
| RECHNUNG_NICHT_GEFUNDEN | Rechnung nicht vorhanden oder nicht sichtbar |
| BUCHUNG_NICHT_GEFUNDEN | Buchung nicht vorhanden oder nicht sichtbar |
| SYSTEM_UNERWARTETER_FEHLER | unerwarteter technischer Fehler |

## Validierungsfehler

Validierungsfehler enthalten Feldinformationen.

```json
{
  "fehlercode": "ANTRAG_VALIDIERUNG_FEHLER",
  "meldung": "Der Antrag enthält ungültige Angaben.",
  "details": [
    {
      "feld": "sportanlageId",
      "meldung": "Die Sportanlage ist erforderlich."
    },
    {
      "feld": "zeitraumBis",
      "meldung": "Das Enddatum darf nicht vor dem Anfangsdatum liegen."
    }
  ],
  "korrelationsId": "...",
  "zeitpunkt": "..."
}
```

## Fachliche Fehler

Fachliche Fehler entstehen, wenn eine Anfrage technisch korrekt ist, aber fachlich nicht ausgeführt werden darf.

Beispiele:

- Antrag kann nicht erneut eingereicht werden
- Entwurf gehört nicht zum aktuellen Kontext
- Dokument darf nicht heruntergeladen werden
- Rechnung ist für den Benutzer nicht sichtbar

Fachliche Fehler werden nicht im Portal entschieden.

Die verbindliche Entscheidung trifft die REST-API bzw. die darunterliegende SportFM-Fachlogik.

## Technische Fehler

Technische Fehler werden intern vollständig protokolliert.

Der Client erhält nur eine neutrale Meldung.

Beispiel:

```json
{
  "fehlercode": "SYSTEM_UNERWARTETER_FEHLER",
  "meldung": "Die Anfrage konnte nicht verarbeitet werden.",
  "details": [],
  "korrelationsId": "...",
  "zeitpunkt": "..."
}
```

## KorrelationsId

Jede Anfrage erhält eine KorrelationsId.

Diese ID wird verwendet für:

- REST-Logging
- Fehleranalyse
- Nachvollziehbarkeit über Schichten hinweg
- Zuordnung von Portalfehlern zu Serverlogs

```text
Portal
  ↓ KorrelationsId
REST-API
  ↓ KorrelationsId
Business Services
  ↓ KorrelationsId
PL/SQL / Oracle Logging
```

## Bezug zu PL/SQL und Oracle

Oracle- und PL/SQL-Fehler werden nicht direkt an den Client durchgereicht.

Sie werden serverseitig in das einheitliche Fehlerformat übersetzt.

Beispiel:

| Interner Fehler | REST-Fehler |
|---|---|
| Datensatz nicht gefunden | 404 / DOKUMENT_NICHT_GEFUNDEN |
| fachliche Prüfung fehlgeschlagen | 422 / fachlicher Fehlercode |
| technische Datenbankstörung | 500 / SYSTEM_UNERWARTETER_FEHLER |

## Protokollierung

Jeder Fehler wird serverseitig protokolliert.

Mindestens zu speichern:

| Feld | Beschreibung |
|---|---|
| KorrelationsId | eindeutige Fehlerzuordnung |
| Zeitpunkt | Zeitpunkt des Fehlers |
| BenutzerId | soweit vorhanden |
| KontextId | soweit vorhanden |
| Endpunkt | aufgerufener REST-Endpunkt |
| HTTP-Methode | GET, POST, PUT, DELETE |
| Fehlercode | fachlicher Fehlercode |
| technischer Fehler | nur intern |

## Sicherheitsgrundsätze

| Nr. | Grundsatz |
|---:|---|
| FF-001 | Keine internen technischen Details an Clients ausgeben. |
| FF-002 | Keine Oracle-Fehlertexte ungefiltert zurückgeben. |
| FF-003 | Keine Hinweise auf fremde vorhandene Objekte geben. |
| FF-004 | Jeder Fehler erhält eine KorrelationsId. |
| FF-005 | Berechtigungs- und Sichtbarkeitsfehler werden restriktiv behandelt. |

## Abgrenzung

Das Fehlerformat beschreibt nur die Rückgabe an REST-Clients.

Die interne Fehlerbehandlung in PL/SQL-Packages und Oracle-Logging wird im Datenmodell bzw. Architekturkapitel dokumentiert.

## Offene Punkte

| ID | Frage |
|---|---|
| OF-FF-001 | finale Liste fachlicher Fehlercodes je Domäne festlegen |
| OF-FF-002 | verbindliches JSON-Namensschema festlegen |
| OF-FF-003 | Umfang der Protokollierung in Oracle klären |
| OF-FF-004 | Mapping bestehender PL/SQL-Fehler auf REST-Fehlercodes definieren |
| OF-FF-005 | Umgang mit Mehrsprachigkeit der Fehlermeldungen festlegen |

## Nächster Schritt

Die konkrete Fehlercode-Liste wird je Domäne ergänzt, sobald die jeweiligen Endpunkte und DTOs finalisiert sind.