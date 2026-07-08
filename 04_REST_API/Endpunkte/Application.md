# Application

## Ziel

Diese Datei beschreibt die REST-Endpunkte für Anträge aus dem SportPortal.

## Grundsatz

Anträge werden über ein konfigurierbares Wizard-Framework erfasst.

Ein Antrag ist nicht automatisch eine Buchung.

```text
Antrag
  ↓
Workflow
  ↓
fachliche Prüfung
  ↓
Reservierung / Buchung in SportFM
```

## V1-Umfang

| Funktion | V1 |
|---|:---:|
| Antragstypen laden | ja |
| Antrag als Entwurf speichern | ja |
| Antrag laden | ja |
| Antrag einreichen | ja |
| Antragsstatus anzeigen | ja |
| Uploads zu Antrag | ja |
| Workflow-Historie anzeigen | ja |

## Vorgesehene Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| GET | /applications/types | verfügbare Antragstypen laden |
| POST | /applications | Antrag anlegen |
| GET | /applications | eigene Anträge laden |
| GET | /applications/{id} | Antrag laden |
| PUT | /applications/{id} | Entwurf speichern |
| POST | /applications/{id}/submit | Antrag einreichen |
| GET | /applications/{id}/history | Antragshistorie laden |
| POST | /applications/{id}/files | Datei zum Antrag hochladen |

## Abgrenzung

Die Antragsdomäne speichert Antrag, Status, Eingabedaten, Uploads und Historie.

Die bestehende Buchungslogik wird dadurch nicht ersetzt.