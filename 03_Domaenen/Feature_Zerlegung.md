# Feature-Zerlegung

| Feld | Wert |
|---|---|
| Kapitel | 03 – Domänen |
| Dokument | Feature_Zerlegung |
| Status | Arbeitsstand für Kalkulationsvorbereitung |
| Zweck | Funktionale Zerlegung der Domänen als Vorstufe zur Arbeitspaket- und Aufwandsschätzung |
| Abgrenzung | Dieses Dokument beschreibt Features, noch keine Arbeitspakete und noch keine Stunden |
| Folgekapitel | `06_Arbeitspakete`, `07_Kalkulation`, `09_Testkonzept` |

---

## 1 Zweck

Dieses Dokument zerlegt die Domänen des Projekts SportFM in kalkulationsfähige Features.

Die Feature-Zerlegung ist die verbindliche Zwischenebene zwischen:

```text
Domäne
  ↓
Feature
  ↓
Arbeitspaket
  ↓
Aufwand
  ↓
Testfall
```

Damit wird verhindert, dass Arbeitspakete zu früh, zu grob oder rein technisch gebildet werden.

---

## 2 Grundsätze

| Nr. | Grundsatz |
|---:|---|
| 1 | Features werden fachlich beschrieben, nicht nach Masken oder Tabellen. |
| 2 | Ein Feature beschreibt eine erkennbare Funktion oder Fähigkeit der Plattform. |
| 3 | Features sind noch keine Arbeitspakete. |
| 4 | Jedes Feature wird genau einer führenden Domäne zugeordnet. |
| 5 | Querschnittsfunktionen werden nur einmal beschrieben und von anderen Domänen referenziert. |
| 6 | Vorhandene SportFM-Fachlogik wird freigelegt oder erweitert, nicht neu erfunden. |
| 7 | Portal, REST-Schnittstelle und WPF-Migration werden erst in den Arbeitspaketen getrennt. |

---

## 3 Deutsche Codierung

Für alle neuen Projektobjekte werden deutsche Kürzel verwendet.

| Kürzel | Bedeutung |
|---|---|
| `DOM` | Domäne |
| `FOB` | Fachobjekt |
| `FUN` | Funktion / Feature |
| `ANF` | Anforderung |
| `AP` | Arbeitspaket |
| `TF` | Testfall |
| `AE` | Architekturentscheidung |
| `KL` | Klärpunkt |
| `RIS` | Risiko |

### 3.1 Domänenkürzel

| Kürzel | Domäne | bestehende Datei |
|---|---|---|
| `ZUG` | Zugriff und Authentifizierung | `Authentication.md` |
| `PBN` | Portalbenutzer | `PortalUser.md` |
| `ORG` | Organisation | `Organisation.md` |
| `KON` | Kontext | `Context.md` |
| `VER` | Verwaltung | `Administration.md` |
| `ANT` | Antrag | `Application.md` |
| `ASS` | Antragsassistent | `Wizard.md` |
| `ABL` | Ablaufsteuerung | `Workflow.md` |
| `DAU` | Dateiannahme und Upload | `Upload.md` |
| `SPO` | Sportstätten | `Facility.md` |
| `VFG` | Verfügbarkeit | `Availability.md` |
| `BUC` | Buchung | `Booking.md` |
| `GEB` | Gebühren | `Charge.md` |
| `REC` | Rechnung | `Invoice.md` |
| `DOK` | Dokumente | `Document.md` |
| `NCH` | Nachrichten | `Notification.md` |
| `UEB` | Übersicht / Dashboard | `Dashboard.md` |
| `AUS` | Auswertungen | noch zu ergänzen |
| `SYS` | System / Querschnitt | mehrere Dokumente |

### 3.2 Beispielcodierung

```text
DOM-ANT        Domäne Antrag
FUN-ANT-010    Feature Antrag einreichen
ANF-ANT-010    Anforderung Antrag einreichen
AP-ANT-010-01  Arbeitspaket zu Antrag einreichen
TF-ANT-010-01  Testfall zu Antrag einreichen
```

---

## 4 Feature-Klassen

| Klasse | Bedeutung |
|---|---|
| `N` | Neuentwicklung |
| `E` | Erweiterung bestehender Funktion |
| `F` | Freilegung vorhandener Funktion über Schnittstelle |
| `M` | Migration / Ablösung vorhandener Nutzung |
| `I` | Integration externer oder bestehender Systeme |
| `K` | Konfiguration |
| `Q` | Querschnittsfunktion |

---

## 5 Prioritäten

| Priorität | Bedeutung |
|---|---|
| `MUSS` | zwingend für Projektziel oder V1 erforderlich |
| `SOLL` | fachlich wichtig, aber nicht zwingend für erste nutzbare Ausbaustufe |
| `KANN` | sinnvoll, aber nachrangig |
| `ZUKUNFT` | bewusst vorgesehene spätere Erweiterung |

---

## 6 Feature-Landkarte

```text
ZUG ─ PBN ─ ORG ─ KON
  ↓
UEB
  ↓
ANT ─ ASS ─ ABL ─ DAU
  ↓
SPO ─ VFG ─ BUC
  ↓
GEB ─ REC ─ DOK
  ↓
NCH ─ AUS ─ VER ─ SYS
```

---

# 7 Feature-Zerlegung nach Domänen

## 7.1 DOM-ZUG – Zugriff und Authentifizierung

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-ZUG-001 | Registrierung | N | MUSS | Portalnutzer registrieren sich mit E-Mail und Passwort. |
| FUN-ZUG-002 | E-Mail-Bestätigung | N | MUSS | Registrierung wird über bestätigte E-Mail-Adresse abgesichert. |
| FUN-ZUG-003 | Freischaltung | N | MUSS | Neu registrierte Benutzer bleiben bis zur Prüfung gesperrt. |
| FUN-ZUG-004 | Anmeldung | N | MUSS | Benutzer melden sich am Portal an. |
| FUN-ZUG-005 | Abmeldung | N | MUSS | Benutzer können ihre Sitzung beenden. |
| FUN-ZUG-006 | Passwort vergessen | N | MUSS | Passwort kann über gesicherten Prozess zurückgesetzt werden. |
| FUN-ZUG-007 | Passwort ändern | N | MUSS | Angemeldete Benutzer können ihr Passwort ändern. |
| FUN-ZUG-008 | Zwei-Faktor-Anmeldung | N | MUSS | Zusätzliche Absicherung über zweiten Faktor. |
| FUN-ZUG-009 | Sitzungsverwaltung | N | MUSS | Sitzungen werden zeitlich und technisch kontrolliert. |
| FUN-ZUG-010 | Tokenverwaltung | N | MUSS | Zugriff auf REST-Schnittstelle wird technisch abgesichert. |
| FUN-ZUG-011 | OAuth für Fremdsysteme | N/I | MUSS | Externe Systeme erhalten kontrollierten Zugriff über OAuth. |
| FUN-ZUG-012 | BundID-Anbindung | I | SOLL | Anmeldung über BundID wird vorbereitet bzw. angebunden. |
| FUN-ZUG-013 | Benutzer sperren | N | MUSS | Administratoren können Portalbenutzer sperren. |
| FUN-ZUG-014 | Benutzer entsperren | N | MUSS | Gesperrte Benutzer können wieder aktiviert werden. |
| FUN-ZUG-015 | Anmeldeprotokoll | Q | MUSS | Anmeldeereignisse werden nachvollziehbar protokolliert. |

---

## 7.2 DOM-PBN – Portalbenutzer

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-PBN-001 | Benutzerprofil anlegen | N | MUSS | Fachliches Portalprofil wird erzeugt. |
| FUN-PBN-002 | Benutzerprofil bearbeiten | N | MUSS | Stammdaten des Portalbenutzers können gepflegt werden. |
| FUN-PBN-003 | E-Mail-Adresse ändern | N | MUSS | Änderung erfolgt mit erneuter Bestätigung. |
| FUN-PBN-004 | Telefonnummern pflegen | N | SOLL | Kontaktinformationen werden verwaltet. |
| FUN-PBN-005 | Zustimmung verwalten | N | MUSS | Datenschutz- und Nutzungszustimmungen werden gespeichert. |
| FUN-PBN-006 | Benutzereinstellungen | N | SOLL | Sprache, Darstellung und Benachrichtigungen werden gepflegt. |
| FUN-PBN-007 | Organisationszuordnung anzeigen | N | MUSS | Benutzer sieht zugeordnete Organisationen. |
| FUN-PBN-008 | Rollen anzeigen | N | MUSS | Benutzer sieht seine Berechtigungsrollen. |
| FUN-PBN-009 | Vertreterfunktion | N | SOLL | Benutzer können für mehrere Organisationen handeln. |
| FUN-PBN-010 | Benutzer deaktivieren | N | MUSS | Benutzerprofil kann fachlich deaktiviert werden. |
| FUN-PBN-011 | Änderungsverlauf | Q | SOLL | Änderungen am Profil werden nachvollziehbar protokolliert. |

---

## 7.3 DOM-ORG – Organisation

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-ORG-001 | Organisation anzeigen | F | MUSS | SportFM-Nutzer / Organisation wird im Portal angezeigt. |
| FUN-ORG-002 | Organisation suchen | F | MUSS | Organisationen können administrativ gesucht werden. |
| FUN-ORG-003 | Organisation verknüpfen | N/E | MUSS | Portalorganisation wird mit SportFM-Nutzer verknüpft. |
| FUN-ORG-004 | Abteilungen verwalten | N | SOLL | Vereine können fachlich in Abteilungen gegliedert werden. |
| FUN-ORG-005 | Ansprechpartner verwalten | N/E | MUSS | Ansprechpartner werden je Organisation gepflegt. |
| FUN-ORG-006 | Antragsberechtigte verwalten | N | MUSS | Berechtigte Personen für Antragstellung werden definiert. |
| FUN-ORG-007 | Rollen je Organisation | N | MUSS | Rechte werden organisationsbezogen vergeben. |
| FUN-ORG-008 | Mitgliedschaft prüfen | N/I | SOLL | Vereinsdaten können später mit externen Quellen abgeglichen werden. |
| FUN-ORG-009 | Dokumente der Organisation | F | MUSS | Organisation sieht zugehörige Dokumente. |
| FUN-ORG-010 | Rechnungen der Organisation | F | MUSS | Organisation sieht zugehörige Rechnungen. |
| FUN-ORG-011 | Organisationshistorie | Q | SOLL | Änderungen werden nachvollziehbar gemacht. |

---

## 7.4 DOM-KON – Kontext

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-KON-001 | Kontext ermitteln | N | MUSS | Verfügbare Arbeitskontexte werden aus Zuordnungen abgeleitet. |
| FUN-KON-002 | Kontext auswählen | N | MUSS | Benutzer wählt aktive Organisation / Rolle. |
| FUN-KON-003 | Kontext wechseln | N | MUSS | Wechsel zwischen mehreren Berechtigungsräumen. |
| FUN-KON-004 | Kontext speichern | N | SOLL | Zuletzt genutzter Kontext wird vorgemerkt. |
| FUN-KON-005 | Sichtbarkeit begrenzen | N | MUSS | Daten werden nach aktivem Kontext gefiltert. |
| FUN-KON-006 | Kontextprüfung Schnittstelle | Q | MUSS | Jede relevante REST-Anfrage prüft den Kontext. |
| FUN-KON-007 | Kontextfehler behandeln | Q | MUSS | Unzulässige Zugriffe liefern einheitliche Fehler. |

---

## 7.5 DOM-ANT – Antrag

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-ANT-001 | Antragstyp auswählen | N | MUSS | Portalnutzer wählt den passenden Antragstyp. |
| FUN-ANT-002 | Antrag als Entwurf anlegen | N | MUSS | Antrag kann begonnen und gespeichert werden. |
| FUN-ANT-003 | Entwurf bearbeiten | N | MUSS | Entwurf kann später fortgesetzt werden. |
| FUN-ANT-004 | Entwurf löschen | N | MUSS | Noch nicht eingereichte Entwürfe können entfernt werden. |
| FUN-ANT-005 | Antrag validieren | N | MUSS | Antrag wird fachlich und technisch geprüft. |
| FUN-ANT-006 | Antrag einreichen | N | MUSS | Antrag wird verbindlich an SportFM übergeben. |
| FUN-ANT-007 | Antrag sperren | N | MUSS | Eingereichte Anträge sind nicht mehr frei änderbar. |
| FUN-ANT-008 | Antrag zurückziehen | N | SOLL | Antragsteller kann Antrag vor Bearbeitung zurückziehen. |
| FUN-ANT-009 | Antrag anzeigen | N | MUSS | Antragsteller sieht eingereichte und offene Anträge. |
| FUN-ANT-010 | Antragstatus anzeigen | N | MUSS | Bearbeitungsstand wird im Portal angezeigt. |
| FUN-ANT-011 | Antragsnummer erzeugen | N/E | MUSS | Jeder Antrag erhält eine eindeutige Nummer. |
| FUN-ANT-012 | Anlagen zuordnen | N | MUSS | Uploads werden dem Antrag zugeordnet. |
| FUN-ANT-013 | Antragshistorie | Q | MUSS | Status- und Bearbeitungsschritte werden gespeichert. |
| FUN-ANT-014 | Rückfrage beantworten | N | MUSS | Antragsteller kann Nachfragen beantworten. |
| FUN-ANT-015 | Nachweise nachreichen | N | MUSS | Fehlende Anlagen können nachgereicht werden. |
| FUN-ANT-016 | Antrag in Arbeitskorb übergeben | N/E | MUSS | Eingereichte Anträge erscheinen in SportFM-Bearbeitung. |
| FUN-ANT-017 | Antrag mit Reservierung verknüpfen | N/E | MUSS | Aus Antrag kann eine Reservierung entstehen. |
| FUN-ANT-018 | Antrag mit Buchung verknüpfen | N/E | MUSS | Genehmigter Antrag wird mit Buchung verbunden. |
| FUN-ANT-019 | Antrag ablehnen | N/E | MUSS | Ablehnung wird fachlich dokumentiert. |
| FUN-ANT-020 | Antrag archivieren | N | MUSS | Abgeschlossene Anträge bleiben nachvollziehbar. |
| FUN-ANT-021 | Antrag drucken / PDF erzeugen | N/E | SOLL | Antrag kann als PDF ausgegeben werden. |
| FUN-ANT-022 | Antrag kopieren | N | SOLL | Bestehende Antragsdaten können wiederverwendet werden. |
| FUN-ANT-023 | Mehrfachantrag | N | SOLL | Mehrere Zeiten / Anlagen können in einem Antrag gebündelt werden. |
| FUN-ANT-024 | Antragstellerdaten übernehmen | N/E | MUSS | Stammdaten werden aus Profil / Organisation übernommen. |
| FUN-ANT-025 | Plausibilitätsprüfung | N/E | MUSS | Antrag wird gegen Mindestregeln geprüft. |

---

## 7.6 DOM-ASS – Antragsassistent

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-ASS-001 | Assistent aus Konfiguration laden | N | MUSS | Antragserfassung wird dynamisch aus Konfiguration aufgebaut. |
| FUN-ASS-002 | Schritte definieren | K/N | MUSS | Antragstypen bestehen aus konfigurierbaren Schritten. |
| FUN-ASS-003 | Felder definieren | K/N | MUSS | Felder werden typisiert und konfiguriert. |
| FUN-ASS-004 | Pflichtfelder definieren | K/N | MUSS | Pflichtangaben werden je Antragstyp festgelegt. |
| FUN-ASS-005 | Feldtypen bereitstellen | N | MUSS | Text, Zahl, Datum, Auswahl, Mehrfachauswahl, Datei, Zeitraum. |
| FUN-ASS-006 | Sichtbarkeitsregeln | K/N | MUSS | Felder / Schritte erscheinen abhängig von Eingaben. |
| FUN-ASS-007 | Validierungsregeln | K/N | MUSS | Regeln prüfen Eingaben vor Speicherung und Einreichung. |
| FUN-ASS-008 | Zwischenspeicherung | N | MUSS | Eingaben werden schrittweise gespeichert. |
| FUN-ASS-009 | Fortschrittsanzeige | N | MUSS | Nutzer sieht Bearbeitungsfortschritt. |
| FUN-ASS-010 | Zusammenfassung | N | MUSS | Vor Einreichung wird eine Übersicht angezeigt. |
| FUN-ASS-011 | Antragstyp-Versionierung | N | MUSS | Bestehende Anträge bleiben gegen alte Konfiguration nachvollziehbar. |
| FUN-ASS-012 | Konfigurationsprüfung | N | MUSS | Fehlerhafte Assistenten-Konfiguration wird erkannt. |
| FUN-ASS-013 | Standardbausteine | N/K | SOLL | Wiederverwendbare Schritt- und Feldgruppen. |
| FUN-ASS-014 | Gebührenhinweis anzeigen | F/E | SOLL | Gebührenerwartung wird nur durch SportFM-Fachlogik geliefert. |
| FUN-ASS-015 | Verfügbarkeitsauswahl einbinden | F/E | MUSS | Freie Zeiten können in den Antrag übernommen werden. |
| FUN-ASS-016 | Anlagenanforderungen einbinden | K/N | MUSS | Erforderliche Nachweise werden je Antragstyp gesteuert. |
| FUN-ASS-017 | Barrierefreie Bedienung | Q | MUSS | Assistent erfüllt Barrierefreiheitsanforderungen. |
| FUN-ASS-018 | Mehrsprachigkeit vorbereiten | Q | SOLL | Texte können sprachabhängig gepflegt werden. |
| FUN-ASS-019 | Einzelformular-Vergleich | K | MUSS | Aufwand Framework vs. Einzelformular wird kalkulatorisch darstellbar. |

---

## 7.7 DOM-ABL – Ablaufsteuerung

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-ABL-001 | Ablaufmodell definieren | N/K | MUSS | Status und Übergänge werden konfigurierbar beschrieben. |
| FUN-ABL-002 | Status setzen | N | MUSS | Vorgänge erhalten eindeutige Bearbeitungsstatus. |
| FUN-ABL-003 | Statuswechsel prüfen | N | MUSS | Nur zulässige Übergänge sind möglich. |
| FUN-ABL-004 | Aufgabe erzeugen | N | MUSS | Bearbeitung erzeugt Aufgaben für Sachbearbeiter. |
| FUN-ABL-005 | Aufgabe zuweisen | N | MUSS | Aufgaben können Rollen oder Personen zugewiesen werden. |
| FUN-ABL-006 | Aufgabe übernehmen | N | SOLL | Sachbearbeiter übernimmt Vorgang. |
| FUN-ABL-007 | Rückfrage stellen | N | MUSS | Sachbearbeiter kann Rückfragen an Antragsteller stellen. |
| FUN-ABL-008 | Rückfrage beantworten | N | MUSS | Antwort geht zurück in Bearbeitung. |
| FUN-ABL-009 | Unterlagen nachfordern | N | MUSS | Fehlende Nachweise werden gezielt angefordert. |
| FUN-ABL-010 | Genehmigung vorbereiten | N/E | MUSS | Aus geprüften Daten wird Buchung / Dokument vorbereitet. |
| FUN-ABL-011 | Ablehnung dokumentieren | N | MUSS | Ablehnung mit Begründung. |
| FUN-ABL-012 | Abschluss setzen | N | MUSS | Vorgang wird abgeschlossen. |
| FUN-ABL-013 | Fristen verwalten | N/K | SOLL | Bearbeitungsfristen können überwacht werden. |
| FUN-ABL-014 | Ablaufhistorie | Q | MUSS | Jeder Bearbeitungsschritt wird dokumentiert. |
| FUN-ABL-015 | Benachrichtigung auslösen | Q | MUSS | Statuswechsel erzeugen Nachrichten. |
| FUN-ABL-016 | Arbeitskorb SportFM | N/E | MUSS | Vorgänge werden für Sachbearbeitung gebündelt. |

---

## 7.8 DOM-DAU – Dateiannahme und Upload

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-DAU-001 | Datei hochladen | N | MUSS | Antragsteller kann Anlagen hochladen. |
| FUN-DAU-002 | Dateityp prüfen | N | MUSS | Nur erlaubte Dateitypen werden angenommen. |
| FUN-DAU-003 | Dateigröße prüfen | N | MUSS | Größenlimits werden erzwungen. |
| FUN-DAU-004 | Virenprüfung anbinden | I | MUSS | Dateien werden vor Übernahme geprüft. |
| FUN-DAU-005 | Uploadkategorie zuordnen | N/K | MUSS | Uploads werden fachlich klassifiziert. |
| FUN-DAU-006 | Upload einem Antrag zuordnen | N | MUSS | Datei wird eindeutig verknüpft. |
| FUN-DAU-007 | Upload ersetzen | N | SOLL | Fehlerhafte Datei kann ersetzt werden. |
| FUN-DAU-008 | Upload löschen | N | SOLL | Entwurfsdateien können entfernt werden. |
| FUN-DAU-009 | Upload an Dokumente übergeben | E/I | MUSS | Dauerhafte Ablage erfolgt über Dokumentendomäne. |
| FUN-DAU-010 | Download für Sachbearbeitung | N/E | MUSS | Sachbearbeitung kann Anlagen prüfen. |
| FUN-DAU-011 | Uploadprotokoll | Q | MUSS | Annahme und Änderungen werden protokolliert. |

---

## 7.9 DOM-SPO – Sportstätten

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-SPO-001 | Sportstätte suchen | F | MUSS | Nutzer suchen Sportstätten nach Kriterien. |
| FUN-SPO-002 | Sportstätte anzeigen | F | MUSS | Detaildaten werden aus Bestand angezeigt. |
| FUN-SPO-003 | Teileinheiten anzeigen | F | MUSS | Buchbare Einheiten werden sichtbar. |
| FUN-SPO-004 | Sportkomplex anzeigen | F | MUSS | Zusammengehörige Anlagen werden gruppiert. |
| FUN-SPO-005 | Sportartfilter | F | MUSS | Suche nach passenden Sportarten. |
| FUN-SPO-006 | Stadtbezirkfilter | F | MUSS | Suche nach räumlicher Lage. |
| FUN-SPO-007 | Ausstattung anzeigen | F/E | SOLL | Ausstattung wird soweit vorhanden angezeigt. |
| FUN-SPO-008 | Barrierefreiheit anzeigen | F/E | SOLL | Barrierefreiheitsangaben werden angezeigt. |
| FUN-SPO-009 | Kartenverknüpfung | I | SOLL | Anbindung an Themenstadtplan. |
| FUN-SPO-010 | Bilder / Medien anzeigen | E | KANN | Sportstätten können mit Medien dargestellt werden. |
| FUN-SPO-011 | Stammdatenqualität markieren | E | SOLL | Fehlende / unvollständige Angaben werden sichtbar. |
| FUN-SPO-012 | Sportstätte für Antrag übernehmen | F/E | MUSS | Gewählte Anlage wird in Antrag übernommen. |

---

## 7.10 DOM-VFG – Verfügbarkeit

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-VFG-001 | Freie Zeiten suchen | F/E | MUSS | Suche nutzt vorhandene Occurrence-/Winner-Logik. |
| FUN-VFG-002 | Zeitraumfilter | F | MUSS | Einschränkung nach Datum / Saison. |
| FUN-VFG-003 | Wochentagfilter | F | MUSS | Einschränkung nach Wochentagen. |
| FUN-VFG-004 | Uhrzeitfilter | F | MUSS | Einschränkung nach Zeitfenster. |
| FUN-VFG-005 | Sportartfilter | F | MUSS | Suche nach sportfachlicher Eignung. |
| FUN-VFG-006 | Anlagenfilter | F | MUSS | Einschränkung nach Sportstätte / Typ. |
| FUN-VFG-007 | Ergebnisbewertung | E | MUSS | Ergebnisse zeigen Verfügbarkeit und Einschränkungen. |
| FUN-VFG-008 | Konflikte anzeigen | E | SOLL | Belegte Zeiten können mit Ursache angezeigt werden. |
| FUN-VFG-009 | Ferien / Feiertage berücksichtigen | F | MUSS | Bestandlogik berücksichtigt Ausschlüsse. |
| FUN-VFG-010 | Sperrungen berücksichtigen | F | MUSS | Sperrungen fließen über Winner-Logik ein. |
| FUN-VFG-011 | Vorschlag in Antrag übernehmen | N/E | MUSS | Gewählte Zeit wird in Antrag übernommen. |
| FUN-VFG-012 | Kalenderansicht Portal | N/F | MUSS | Verfügbarkeiten werden kalendarisch dargestellt. |
| FUN-VFG-013 | Performante Abfrage | F/E | MUSS | Zugriff erfolgt auf vorbereitete Occurrences / Winner. |
| FUN-VFG-014 | Ergebnisexport | N | KANN | Ergebnisse können exportiert werden. |

---

## 7.11 DOM-BUC – Buchung

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-BUC-001 | Buchung anzeigen | F | MUSS | Bestehende Buchungen werden fachlich angezeigt. |
| FUN-BUC-002 | Buchungsdetails anzeigen | F | MUSS | Detaildaten aus `LHD_SPA_EVENTS`. |
| FUN-BUC-003 | Buchung aus Antrag vorbereiten | E | MUSS | Genehmigter Antrag liefert Daten für Buchung. |
| FUN-BUC-004 | Reservierung erzeugen | E/N | MUSS | Vormerkung vor verbindlicher Buchung. |
| FUN-BUC-005 | Reservierung ändern | E/N | MUSS | Planungsdaten können angepasst werden. |
| FUN-BUC-006 | Reservierung löschen | E/N | MUSS | Vormerkungen können verworfen werden. |
| FUN-BUC-007 | Buchung erzeugen | F/E | MUSS | Buchung wird über bestehende Fachlogik angelegt. |
| FUN-BUC-008 | Buchung ändern | F/E | MUSS | Bestehende Buchung wird angepasst. |
| FUN-BUC-009 | Buchung beenden | F/E | MUSS | Buchung endet fachlich über Enddatum. |
| FUN-BUC-010 | Stornierung erzeugen | F/E | MUSS | Stornierung wird als eigener Eventtyp erzeugt. |
| FUN-BUC-011 | Wiederholung wöchentlich | F | MUSS | Vorhandene Wiederholungslogik wird genutzt. |
| FUN-BUC-012 | Wiederholung vierzehntägig | F | MUSS | Bestehende 14-tägige Logik wird genutzt. |
| FUN-BUC-013 | Veranstaltung buchen | F/E | MUSS | Belegungsart Veranstaltung wird unterstützt. |
| FUN-BUC-014 | Sperrung buchen | F/E | MUSS | Sperrungen bleiben Events. |
| FUN-BUC-015 | Reinigung buchen | F/E | MUSS | Reinigung bleibt Eventtyp. |
| FUN-BUC-016 | Schulbetrieb buchen | F/E | MUSS | Schulbetrieb bleibt priorisierte Belegungsart. |
| FUN-BUC-017 | GTA buchen | F/E | MUSS | GTA bleibt fachliche Belegungsart. |
| FUN-BUC-018 | Occurrences erzeugen | F | MUSS | `PA_LHD_SPA_OCC` bleibt führend. |
| FUN-BUC-019 | Winner ermitteln | F | MUSS | Resultierende Belegung wird aus Bestand ermittelt. |
| FUN-BUC-020 | Konfliktprüfung | F/E | MUSS | Kollisionen werden gegen Bestandlogik geprüft. |
| FUN-BUC-021 | Prioritäten anwenden | F | MUSS | Prioritäten aus Vergabelogik werden berücksichtigt. |
| FUN-BUC-022 | Gebührenbezug herstellen | F/E | MUSS | Buchung erhält Gebühreninformationen. |
| FUN-BUC-023 | Dokumentenbezug herstellen | F/E | MUSS | Dokumente werden mit Buchung verknüpft. |
| FUN-BUC-024 | Kalenderdaten bereitstellen | F | MUSS | Buchungen werden für Kalenderansichten geliefert. |
| FUN-BUC-025 | Portalansicht eigene Buchungen | F/N | MUSS | Nutzer sieht eigene Buchungen. |
| FUN-BUC-026 | WPF-Migration Buchung lesen | M | SOLL | WPF liest Buchungsdaten schrittweise über REST. |
| FUN-BUC-027 | WPF-Migration Buchung schreiben | M | ZUKUNFT | WPF schreibt Buchungen später über REST. |

---

## 7.12 DOM-GEB – Gebühren

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-GEB-001 | Gebührenstammdaten lesen | F | MUSS | Bestehende Gebühren werden bereitgestellt. |
| FUN-GEB-002 | Gebührenarten anzeigen | F | MUSS | Gebührentypen werden angezeigt. |
| FUN-GEB-003 | Gebührenvorschau | F/E | SOLL | Hinweis auf mögliche Gebühren aus SportFM-Logik. |
| FUN-GEB-004 | Gebühren für Buchung ermitteln | F | MUSS | Berechnung bleibt in SportFM. |
| FUN-GEB-005 | Eventgebühren anzeigen | F | MUSS | Gebührenbezug je Buchung. |
| FUN-GEB-006 | Gültigkeitszeiträume berücksichtigen | F | MUSS | Bestand berücksichtigt gültige Sätze. |
| FUN-GEB-007 | Umsatzsteuer anzeigen | F | MUSS | Steuerangaben werden transparent angezeigt. |
| FUN-GEB-008 | Gebühreninformationen für Rechnung | F | MUSS | Übergang zu Rechnungsdomäne. |
| FUN-GEB-009 | Gebührenhinweis im Antrag | F/E | SOLL | Antragsteller erhält Hinweis, keine verbindliche Berechnung. |
| FUN-GEB-010 | Gebührenprotokoll | Q | SOLL | Berechnungsgrundlage nachvollziehbar machen. |

---

## 7.13 DOM-REC – Rechnung

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-REC-001 | Rechnungsliste anzeigen | F | MUSS | Nutzer sieht eigene Rechnungen. |
| FUN-REC-002 | Rechnungsdetails anzeigen | F | MUSS | Details aus Bestand werden angezeigt. |
| FUN-REC-003 | Rechnungszeitraum anzeigen | F | MUSS | Abrechnungszeitraum wird dargestellt. |
| FUN-REC-004 | SAP-Bezug anzeigen | F | SOLL | SAP-Belegangaben werden intern sichtbar. |
| FUN-REC-005 | Rechnungsdokument herunterladen | F | MUSS | PDF wird über Dokumentendomäne bereitgestellt. |
| FUN-REC-006 | Zahlungsstatus anzeigen | E/I | SOLL | Zahlungsstatus wird angezeigt, wenn verfügbar. |
| FUN-REC-007 | ePayBL-Zahlung starten | I | SOLL | Onlinezahlung wird angebunden. |
| FUN-REC-008 | Zahlungsrückmeldung verarbeiten | I | SOLL | Rückmeldung wird übernommen. |
| FUN-REC-009 | Storno anzeigen | F/E | SOLL | Stornorelevante Informationen werden angezeigt. |
| FUN-REC-010 | Rechnungshistorie | Q | SOLL | Änderungen und Zahlstatus werden nachvollziehbar. |

---

## 7.14 DOM-DOK – Dokumente

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-DOK-001 | Dokumentliste anzeigen | F | MUSS | Nutzer sieht eigene Dokumente. |
| FUN-DOK-002 | Dokumentdetails anzeigen | F | MUSS | Metadaten werden angezeigt. |
| FUN-DOK-003 | Dokument herunterladen | F | MUSS | Datei wird berechtigt bereitgestellt. |
| FUN-DOK-004 | Dokumenttyp anzeigen | F | MUSS | Typen aus Bestand werden genutzt. |
| FUN-DOK-005 | Dokumentstatus anzeigen | F | MUSS | Status aktuell / bearbeitet / gedruckt usw. |
| FUN-DOK-006 | Dokument mit Antrag verknüpfen | E | MUSS | Antragsdokumente werden zugeordnet. |
| FUN-DOK-007 | Dokument mit Buchung verknüpfen | F/E | MUSS | Buchungsdokumente bleiben verknüpft. |
| FUN-DOK-008 | Dokument mit Rechnung verknüpfen | F/E | MUSS | Rechnungsdokumente werden bereitgestellt. |
| FUN-DOK-009 | Vorlage verwenden | F/E | MUSS | Bestehende Vorlagen werden weiter genutzt. |
| FUN-DOK-010 | PDF erzeugen | F/E | SOLL | PDF-Erzeugung wird fachlich angebunden. |
| FUN-DOK-011 | Upload übernehmen | E/I | MUSS | Geprüfte Uploads werden dauerhaft gespeichert. |
| FUN-DOK-012 | Portal-Sichtbarkeit prüfen | E | MUSS | Nur berechtigte Dokumente werden angezeigt. |
| FUN-DOK-013 | Download protokollieren | Q | MUSS | Abrufe werden revisionsnah protokolliert. |
| FUN-DOK-014 | Dokument archivieren | E | MUSS | Dokumente bleiben entsprechend Aufbewahrung verfügbar. |
| FUN-DOK-015 | Dokumentenversion anzeigen | F | SOLL | Versionen werden nachvollziehbar. |

---

## 7.15 DOM-NCH – Nachrichten

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-NCH-001 | Portalnachricht erzeugen | N | MUSS | System erzeugt interne Nachricht. |
| FUN-NCH-002 | Portalnachricht anzeigen | N | MUSS | Nutzer sieht Nachrichten im Portal. |
| FUN-NCH-003 | Nachricht als gelesen markieren | N | MUSS | Lesestatus wird gespeichert. |
| FUN-NCH-004 | E-Mail versenden | N/I | MUSS | Benachrichtigungen können per E-Mail gesendet werden. |
| FUN-NCH-005 | Nachrichtenvorlagen | N/K | SOLL | Texte werden konfigurierbar gepflegt. |
| FUN-NCH-006 | Rückfragenachricht | N | MUSS | Workflow-Rückfragen erzeugen Nachricht. |
| FUN-NCH-007 | Dokumentbenachrichtigung | N/E | SOLL | Neues Dokument kann Nachricht erzeugen. |
| FUN-NCH-008 | Rechnungsbenachrichtigung | N/E | SOLL | Neue Rechnung kann Nachricht erzeugen. |
| FUN-NCH-009 | Fristenerinnerung | N/K | SOLL | Fristen können Erinnerungen auslösen. |
| FUN-NCH-010 | Versandprotokoll | Q | MUSS | Versand wird nachvollziehbar protokolliert. |

---

## 7.16 DOM-UEB – Übersicht / Dashboard

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-UEB-001 | Startseite Portal | N | MUSS | Benutzer erhält kontextbezogene Übersicht. |
| FUN-UEB-002 | Offene Anträge anzeigen | N/F | MUSS | Laufende Anträge werden aggregiert. |
| FUN-UEB-003 | Aufgaben anzeigen | N/F | MUSS | Offene Rückfragen / Aufgaben werden angezeigt. |
| FUN-UEB-004 | Neue Dokumente anzeigen | N/F | MUSS | Dokumenthinweise werden aggregiert. |
| FUN-UEB-005 | Neue Rechnungen anzeigen | N/F | SOLL | Rechnungshinweise werden aggregiert. |
| FUN-UEB-006 | Nächste Buchungen anzeigen | N/F | SOLL | Kommende Buchungen werden angezeigt. |
| FUN-UEB-007 | Schnellzugriff Antrag | N | MUSS | Direkter Einstieg in Antragstellung. |
| FUN-UEB-008 | Schnellzugriff freie Zeiten | N | MUSS | Direkter Einstieg in Suche. |
| FUN-UEB-009 | Rollenabhängige Kacheln | N/K | SOLL | Dashboard passt sich Kontext und Rolle an. |
| FUN-UEB-010 | Keine eigene Fachlogik | Q | MUSS | Dashboard aggregiert nur Daten anderer Domänen. |

---

## 7.17 DOM-VER – Verwaltung

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-VER-001 | Benutzer verwalten | N | MUSS | Administratoren verwalten Portalbenutzer. |
| FUN-VER-002 | Organisationen verwalten | N/E | MUSS | Zuordnungen und Ansprechpartner werden administriert. |
| FUN-VER-003 | Rollen verwalten | N | MUSS | Rollen werden vergeben und entzogen. |
| FUN-VER-004 | Berechtigungen verwalten | N | MUSS | Rechte werden rollenbezogen gesteuert. |
| FUN-VER-005 | Antragstypen verwalten | N/K | MUSS | Konfiguration des Antragsassistenten. |
| FUN-VER-006 | Ablaufmodelle verwalten | N/K | MUSS | Status und Übergänge pflegen. |
| FUN-VER-007 | Nachrichtenvorlagen verwalten | N/K | SOLL | E-Mail- und Portaltexte pflegen. |
| FUN-VER-008 | Systemkonfiguration anzeigen | N | MUSS | Relevante Einstellungen sichtbar machen. |
| FUN-VER-009 | Protokolle einsehen | Q | SOLL | Administrativer Zugriff auf Audit / Fehler. |
| FUN-VER-010 | Stammdatenqualität prüfen | E | SOLL | Fehlende Sportstätten- oder Organisationsdaten sichtbar machen. |

---

## 7.18 DOM-AUS – Auswertungen

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-AUS-001 | Standardauswertung ausführen | F/E | SOLL | Bestehende Auswertungen werden bereitgestellt. |
| FUN-AUS-002 | Filterbare Auswertung | N/E | SOLL | Auswertungen können parameterisiert werden. |
| FUN-AUS-003 | Export nach Excel | F/E | SOLL | Ergebnisse können exportiert werden. |
| FUN-AUS-004 | Export nach PDF | N/E | KANN | Berichtsausgabe als PDF. |
| FUN-AUS-005 | Belegungsstatistik | F/E | SOLL | Auslastung und Nutzung auswerten. |
| FUN-AUS-006 | Antragsstatistik | N | SOLL | Anzahl, Status, Durchlaufzeiten. |
| FUN-AUS-007 | Gebührenstatistik | F/E | KANN | Gebühren nach Zeitraum / Nutzer / Anlage. |
| FUN-AUS-008 | Rechnungsstatistik | F/E | KANN | Rechnungen und Zahlstatus auswerten. |
| FUN-AUS-009 | Portalnutzung auswerten | N | KANN | Nutzung des Portals technisch/fachlich auswerten. |
| FUN-AUS-010 | Auswertungsrechte | Q | MUSS | Zugriff auf Auswertungen wird berechtigt. |

---

## 7.19 DOM-SYS – System / Querschnitt

| Feature | Titel | Klasse | Priorität | Kurzbeschreibung |
|---|---|---:|---|---|
| FUN-SYS-001 | Fehlerformat | Q | MUSS | Einheitliches Fehlerformat für REST-Schnittstelle. |
| FUN-SYS-002 | Protokollierung | Q | MUSS | Fachliche und technische Ereignisse werden protokolliert. |
| FUN-SYS-003 | Auditdaten | Q | MUSS | Relevante Änderungen bleiben nachvollziehbar. |
| FUN-SYS-004 | Berechtigungsprüfung | Q | MUSS | Jede geschützte Funktion prüft Zugriff. |
| FUN-SYS-005 | Schnittstellenversionierung | Q | MUSS | REST-Schnittstelle wird versioniert. |
| FUN-SYS-006 | Konfigurationsverwaltung | Q | MUSS | Systemparameter werden kontrolliert verwaltet. |
| FUN-SYS-007 | Gesundheitsprüfung | Q | SOLL | Systemstatus kann überwacht werden. |
| FUN-SYS-008 | Hintergrundverarbeitung | Q | MUSS | Jobs für Mail, Benachrichtigungen, Synchronisierung. |
| FUN-SYS-009 | Datenschutzfunktionen | Q | MUSS | Datenminimierung, Auskunft, Löschung soweit fachlich möglich. |
| FUN-SYS-010 | Aufbewahrungsfristen | Q | MUSS | Fristen werden bei Archivierung berücksichtigt. |
| FUN-SYS-011 | Barrierefreiheit | Q | MUSS | Portal erfüllt Barrierefreiheitsanforderungen. |
| FUN-SYS-012 | Mehrsprachigkeit | Q | SOLL | Portaltexte werden mehrsprachig vorbereitet. |
| FUN-SYS-013 | Leistungsfähigkeit | Q | MUSS | Suche und Kalender bleiben bei erwarteter Datenmenge performant. |
| FUN-SYS-014 | Datensicherung / Wiederherstellung | Q | MUSS | Betriebliches Sicherungskonzept wird berücksichtigt. |
| FUN-SYS-015 | Deployment | Q | MUSS | Bereitstellung für Test und Produktion. |

---

# 8 Zusammenfassung der Feature-Anzahl

| Domäne | Anzahl Features |
|---|---:|
| Zugriff und Authentifizierung | 15 |
| Portalbenutzer | 11 |
| Organisation | 11 |
| Kontext | 7 |
| Antrag | 25 |
| Antragsassistent | 19 |
| Ablaufsteuerung | 16 |
| Dateiannahme und Upload | 11 |
| Sportstätten | 12 |
| Verfügbarkeit | 14 |
| Buchung | 27 |
| Gebühren | 10 |
| Rechnung | 10 |
| Dokumente | 15 |
| Nachrichten | 10 |
| Übersicht / Dashboard | 10 |
| Verwaltung | 10 |
| Auswertungen | 10 |
| System / Querschnitt | 15 |
| **Gesamt** | **258** |

---

# 9 Auswertung für die nächste Phase

Diese Feature-Zerlegung ist noch keine Aufwandsschätzung.

Sie dient als Grundlage für die spätere Zerlegung in Arbeitspakete.

Pro Feature entstehen typischerweise mehrere Arbeitspakete, z. B.:

```text
Feature
  ↓
Datenmodell
Schnittstelle
Fachservice
Portaloberfläche
Validierung
Berechtigung
Protokollierung
Test
Dokumentation
```

Bei 258 Features ist realistisch mit etwa 350 bis 600 Arbeitspaketen zu rechnen, abhängig vom gewünschten Detaillierungsgrad.

---

# 10 Nächste Schritte

1. Diese Datei in `03_Domaenen/Feature_Zerlegung.md` übernehmen.
2. Für jedes Feature prüfen, ob es bereits in den vorhandenen Domänendokumenten beschrieben ist.
3. Fehlende Features in den jeweiligen Domänendokumenten ergänzen.
4. Danach `03_Domaenen/Feature_Matrix.md` erstellen.
5. Anschließend `06_Arbeitspakete/Arbeitspaketliste.md` aus den Features ableiten.

---

# 11 Klärpunkte

| Klärpunkt | Thema | Beschreibung |
|---|---|---|
| KL-ANT-001 | Antragstypen V1 | Finale Liste der Antragstypen für erste Ausbaustufe bestätigen. |
| KL-ORG-001 | Organisationsmodell | Abteilungen, Ansprechpartner und antragsberechtigte Personen fachlich bestätigen. |
| KL-ZUG-001 | BundID | Verbindlichkeit und Zeitpunkt der BundID-Anbindung klären. |
| KL-REC-001 | ePayBL | Umfang der Onlinezahlung klären. |
| KL-DOK-001 | Portal-Sichtbarkeit | Bestätigen, dass grundsätzlich alle eigenen Dokumente sichtbar sein sollen. |
| KL-BUC-001 | Reservierungsmodell | Technische Abbildung von Reservierungen im Bestand oder als neue Struktur klären. |
| KL-AUS-001 | Auswertungen | Reporting als eigene Domäne ergänzen oder nur in vorhandenen Modulen belassen. |

---

# 12 Änderungsauswirkungen

Änderungen an diesem Dokument wirken sich aus auf:

- `03_Domaenen/Uebersicht.md`,
- alle Domänendokumente in `03_Domaenen/`,
- `04_REST_API/Endpunkte.md`,
- `05_Datenmodell/Tabellen.md`,
- `06_Arbeitspakete/Arbeitspaketliste.md`,
- `07_Kalkulation/Aufwandsschaetzung.md`,
- `09_Testkonzept/Testfaelle.md`.
