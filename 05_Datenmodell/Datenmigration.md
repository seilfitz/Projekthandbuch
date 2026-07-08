# Datenmigration

> Kapitel: `05_Datenmodell/Datenmigration.md`  
> Projekt: SportFM / SportPortal  
> Status: Arbeitsfassung  
> Zweck: Grundlage für Umsetzung, Deployment, Test und Aufwandsschätzung

---

## 1. Ziel des Dokuments

Dieses Dokument beschreibt die Datenmigration und Initialbefüllung für die Weiterentwicklung von SportFM zur Plattform mit REST-API und Onlineportal.

Dabei ist wichtig:

- SportFM bleibt führendes Fachsystem.
- Die bestehenden Oracle-Tabellen für Buchungen, Belegungen, Gebühren, Dokumente und Rechnungen werden nicht ersetzt.
- Neue Portal-Tabellen ergänzen den Bestand.
- Bestehende Fachlogik in `PA_LHD_SPA` und `PA_LHD_SPA_OCC` bleibt maßgeblich.
- Es findet keine vollständige Migration der bestehenden Fachlogik in neue Portalstrukturen statt.

Die Datenmigration dient vor allem dazu, das neue Portal startfähig zu machen und bestehende SportFM-Daten kontrolliert für Portalnutzer verfügbar zu machen.

---

## 2. Grundprinzipien

### 2.1 Bestandsschutz

Bestehende produktive SportFM-Daten bleiben in den vorhandenen Tabellen erhalten.

Insbesondere bleiben führend:

```text
LHD_SPA_EVENTS
LHD_SPA_OCC
LHD_SPA_OCC_WINNER
LHD_SPA_RECURRING_PATTERN
LHD_SPA_DOCUMENTS
LHD_SPA_INVOICES
LHD_SPA_CHARGES
```

Die vorhandenen Tabellen werden nicht in neue Portal-Tabellen kopiert.

### 2.2 Referenzierung statt Kopie

Das Portal referenziert bestehende SportFM-Daten.

Beispiele:

```text
PortalUser → Organisation → SportFM-Nutzer
Application → spätere Reservierung / Buchung
DocumentView → LHD_SPA_DOCUMENTS
InvoiceView → LHD_SPA_INVOICES
Availability → LHD_SPA_OCC_WINNER
```

### 2.3 Keine doppelte Fachlogik

Die Migration erzeugt keine neue Buchungs-, Gebühren-, Rechnungs- oder Belegungslogik.

Die neuen Portal-Tabellen speichern Portalzustände, Workflows, Konfigurationen und Zuordnungen.

Die fachliche Entscheidung bleibt im SportFM-Kern.

### 2.4 Revisionsfähigkeit

Alle migrationsrelevanten Änderungen müssen nachvollziehbar sein.

Erforderlich sind:

- Migrationsskripte mit Versionsnummer
- Protokollierung erfolgreicher Schritte
- Protokollierung fehlerhafter Schritte
- Wiederholbarkeit, soweit fachlich möglich
- Rollback-Konzept für technische Fehler

---

## 3. Migrationsarten

Für das Projekt werden vier Arten von Datenbewegungen unterschieden.

| Art | Beschreibung | Beispiel |
|---|---|---|
| Keine Migration | Bestand bleibt unverändert und wird nur gelesen | Buchungen, Occurrences, Winner |
| Initialbefüllung | Neue Tabellen werden einmalig befüllt | Rollen, Rechte, Wizard-Konfiguration |
| Zuordnung | Neue Portalobjekte werden mit Bestandsdaten verknüpft | Organisation zu SportFM-Nutzer |
| Laufende Synchronisation | Daten werden regelmäßig abgeglichen oder referenziert | Sportstätten, Nutzerstammdaten, Dokumente |

---

## 4. Bestandsdaten ohne Migration

Diese Datenbereiche werden nicht migriert, sondern weiterhin im Bestand genutzt.

### 4.1 Buchungen / Events

Führende Tabellen:

```text
LHD_SPA_EVENTS
LHD_SPA_EVENT2UNIT
LHD_SPA_RECURRING_PATTERN
```

Begründung:

- Buchungen sind produktive Fachobjekte.
- Bestehende Logik ist erprobt.
- Stornierungen sind fachlich eigene Events.
- Wiederholungen werden über vorhandene Logik verarbeitet.

Migrationsentscheidung:

```text
Keine Kopie in Portal-Tabellen.
Keine Neuberechnung im Portal.
Nur Zugriff über Portal-Packages und REST-API.
```

### 4.2 Occurrences / Winner

Führende Tabellen:

```text
LHD_SPA_OCC
LHD_SPA_OCC_WINNER
```

Begründung:

- Die performante Terminlogik ist bereits gelöst.
- Die Tabellen werden durch vorhandene Jobs und Fast Path gepflegt.
- Kalender und freie Zeiten müssen diese vorberechneten Strukturen nutzen.

Migrationsentscheidung:

```text
Keine Migration.
Nutzung als performante Lesestruktur.
```

### 4.3 Gebühren

Führende Tabellen:

```text
LHD_SPA_CHARGES
LHD_SPA_CHARGETYPES
LHD_SPA_CHARGEGROUPS
LHD_SPA_EVENTCHARGES
```

Migrationsentscheidung:

```text
Keine Migration in Portalstrukturen.
Gebühren werden ausschließlich durch SportFM berechnet.
```

### 4.4 Rechnungen

Führende Tabellen:

```text
LHD_SPA_INVOICES
LHD_SPA_INVOICE_CHARGEINFOS
```

Migrationsentscheidung:

```text
Keine Kopie von Rechnungen.
Portal erhält nur sichtberechtigten Zugriff.
```

### 4.5 Dokumente

Führende Tabellen:

```text
LHD_SPA_DOCUMENTS
LHD_SPA_DOCUMENT_TYPES
LHD_SPA_DOCUMENT_TEMPLATES
LHD_SPA_DOCUMENT_TEXTMODULES
```

Migrationsentscheidung:

```text
Keine Kopie bestehender Dokumente.
Portalzugriff über Berechtigung und Referenz.
```

---

## 5. Neu anzulegende Portal-Datenbereiche

Die folgenden Datenbereiche entstehen neu und müssen initial befüllt oder zur Laufzeit aufgebaut werden.

### 5.1 Authentication

Neue Daten:

```text
Portal-Konten
Passwort-/Login-Informationen
2FA-Informationen
OAuth-Clients
Tokens / Sessions
Login-Protokoll
```

Migrationsart:

```text
Initial leer.
Entsteht durch Registrierung, Freischaltung und Administration.
```

Besonderheit:

Bestehende SportFM-Benutzerverwaltung wird nicht 1:1 übernommen, da Portalnutzer fachlich anders aufgebaut sind als interne SportFM-Benutzer.

### 5.2 PortalUser

Neue Daten:

```text
Portalnutzer
Kontaktinformationen
Freischaltstatus
Verknüpfung zu Organisationen
Benutzerpräferenzen
```

Migrationsart:

```text
Initial leer oder optionale Vorbefüllung bekannter Ansprechpartner.
```

Offen zu klären:

- Sollen bestehende Ansprechpartner aus RIB FM / SportFM initial als Portalnutzer vorgeschlagen werden?
- Oder erfolgt die initiale Anlage vollständig über Selbstregistrierung?

### 5.3 Organisation

Neue Daten:

```text
Portal-Organisation
Organisationsrolle
Zuordnung zu SportFM-Nutzer
Vertretungsberechtigungen
Abteilungen / Bereiche eines Vereins
```

Migrationsart:

```text
Zuordnung zu bestehenden SportFM-Nutzern erforderlich.
```

Beispiel:

```text
SportFM-Nutzer: Dynamo Dresden
Portal-Organisation: Dynamo Dresden
PortalUser 1: Abteilung Fußball
PortalUser 2: Abteilung Nachwuchs
PortalUser 3: Vorstand / Verwaltung
```

Wichtig:

Im SportFM reicht der Nutzer als Buchungspartner. Im Portal müssen mehrere handelnde Personen pro Organisation abgebildet werden.

### 5.4 Context

Neue Daten:

```text
aktiver Bearbeitungskontext
PortalUser
Organisation
Rolle
Berechtigung
```

Migrationsart:

```text
Keine klassische Migration.
Kontexte entstehen aus PortalUser-Organisation-Zuordnungen.
```

### 5.5 Wizard

Neue Daten:

```text
Wizard-Definitionen
Antragstypen
Schritte
Felder
Validierungen
Sichtbarkeitsregeln
Upload-Kategorien
Mapping-Regeln
```

Migrationsart:

```text
Initialbefüllung mit den ersten Antragsarten aus dem Lastenheft und den Anlagen.
```

Initial mindestens vorzusehen:

```text
Trainingszeiten ohne Schulsportanlagen
Wettkämpfe / Veranstaltungen ohne Schulsportanlagen
Trainingszeiten ESBZ
Wettkämpfe / Veranstaltungen ESBZ
Trainingszeiten Schulsportanlagen
Wettkämpfe / Veranstaltungen Schulsportanlagen
Einzelveranstaltungen
```

Hinweis:

Der Antrag „Anmietung Dritter“ ist nach bisherigem Stand nicht Bestandteil des Projektumfangs, kann aber als späterer Erweiterungskandidat im Wizard-Framework berücksichtigt werden.

### 5.6 Application

Neue Daten:

```text
Antrag
Antragsdaten
Status
Einreichdatum
Antragsteller
Organisation
Wizard-Version
Übergabe an SportFM
```

Migrationsart:

```text
Initial leer.
Entsteht durch Portalnutzung.
```

Bestandsanträge werden nicht nachträglich rekonstruiert, sofern nicht ausdrücklich gefordert.

### 5.7 Workflow

Neue Daten:

```text
Workflow-Instanzen
Statushistorie
Aufgaben
Rückfragen
Bearbeiterzuordnung
Entscheidungen
```

Migrationsart:

```text
Initialbefüllung der Workflow-Definitionen.
Workflow-Instanzen entstehen zur Laufzeit.
```

### 5.8 Upload

Neue Daten:

```text
hochgeladene Dateien
Metadaten
Zuordnung zu Antrag / Workflow / Organisation
Prüfstatus
Virusprüfung / technische Prüfungen
```

Migrationsart:

```text
Initial leer.
Entsteht durch Portalnutzung.
```

Wichtig:

Uploads sind nicht automatisch SportFM-Dokumente. Erst nach fachlicher Übernahme können daraus Dokumente im SportFM-Kontext entstehen oder referenziert werden.

### 5.9 Notification

Neue Daten:

```text
E-Mail-Benachrichtigungen
Portalnachrichten
Versandstatus
Vorlagen
Fehlerprotokolle
```

Migrationsart:

```text
Initialbefüllung von Nachrichtenvorlagen.
Nachrichten entstehen zur Laufzeit.
```

### 5.10 Dashboard

Neue Daten:

```text
Dashboard-Kacheln
rollenabhängige Sichten
Benutzerpräferenzen
Kennzahlen-Konfiguration
```

Migrationsart:

```text
Initialbefüllung der Standard-Dashboards.
```

---

## 6. Mapping zwischen Portal und SportFM

### 6.1 PortalUser zu SportFM-Nutzer

Grundmodell:

```text
PortalUser
  → PortalUserOrganisation
    → Organisation
      → SportFM-Nutzer
```

Ein SportFM-Nutzer kann mehrere Portalnutzer haben.

Ein Portalnutzer kann mehrere Organisationen vertreten, sofern dies fachlich zugelassen wird.

### 6.2 Organisation zu SportFM-Nutzer

Die Organisation bildet die Portal-Sicht auf einen bestehenden SportFM-Nutzer.

Beispiele:

```text
Verein
Schule
Kita
Firma
Privatperson
```

Die Zuordnung muss eindeutig und prüfbar sein.

### 6.3 Application zu Booking

Ein Antrag ist zunächst kein Event.

Ablauf:

```text
Application eingereicht
→ Workflow-Prüfung
→ Reservierung / Planung
→ Buchung in LHD_SPA_EVENTS
→ Occurrence-Berechnung
→ Winner-Berechnung
```

Migrationsentscheidung:

```text
Applications werden nicht in bestehende Events migriert.
Erst die fachliche Übernahme erzeugt oder verändert SportFM-Events.
```

### 6.4 Upload zu Document

Uploads bleiben zunächst Portalobjekte.

Mögliche Übergänge:

```text
Upload bleibt Antragsanlage
Upload wird Bestandteil der Akte
Upload wird in SportFM-Dokument übernommen
Upload wird mit SportFM-Dokument referenziert
```

Die konkrete Übergabe muss im Pflichtenheft je Dokumenttyp festgelegt werden.

### 6.5 Invoice und Document Sichtbarkeit

Grundsatz:

```text
Portalnutzer sehen Dokumente und Rechnungen ihrer berechtigten Organisationen.
```

Die Zugriffskontrolle erfolgt über:

```text
PortalUser
Organisation
SportFM-Nutzer
Dokument / Rechnung
```

---

## 7. Initialbefüllung

### 7.1 Rollen und Rechte

Initial anzulegen sind mindestens:

```text
Portalnutzer
Organisationsadministrator
Antragsteller
Lesender Benutzer
Sachbearbeiter
Administrator
System / API-Client
```

Die endgültigen Rollen werden im Rollen- und Berechtigungskonzept festgelegt.

### 7.2 Statuswerte

Initial erforderlich sind Statuswerte für:

```text
PortalUser
Organisation
Application
Workflow
Upload
Notification
OAuth-Client
```

Bestehende Statuswerte werden nicht überschrieben.

Bekannte Bestandsstatus:

```text
EVENTS:
  1 aktiv
 -1 gelöscht

DOCUMENTS:
 -1 gelöscht
  1 aktuell
  2 bearbeitet
  3 gedruckt

INVOICES:
 -1 gelöscht
  1 aktuell
```

### 7.3 Wizard-Konfiguration

Initial zu importieren sind die ersten Antragstypen aus den Lastenheft-Anlagen.

Für jeden Antragstyp erforderlich:

```text
Wizard-ID
Version
Schritte
Felder
Pflichtfelder
Validierungen
Upload-Anforderungen
Zusammenfassung
Einreichlogik
Mapping auf Application
```

### 7.4 Workflow-Konfiguration

Initiale Standard-Workflows:

```text
Antrag eingereicht
in Prüfung
Rückfrage
nachgereicht
fachlich geprüft
übernommen in Planung/Reservierung
abgelehnt
abgeschlossen
```

Die genaue Ausprägung wird je Antragstyp festgelegt.

### 7.5 Nachrichtenvorlagen

Initial erforderlich:

```text
Registrierung bestätigen
E-Mail-Adresse verifizieren
Freischaltung erfolgt
Antrag eingereicht
Rückfrage vorhanden
Antrag übernommen
Antrag abgelehnt
Dokument verfügbar
Rechnung verfügbar
Passwort zurücksetzen
2FA eingerichtet
```

---

## 8. Laufende Synchronisation und Referenzierung

### 8.1 Sportstätten

Sportstätten bleiben im Bestandssystem führend.

Das Portal benötigt lesenden Zugriff auf:

```text
Sportanlage
Teileinheit
Adresse
Standort
Ausstattung
Sportartenzuordnung
Verfügbarkeiten
```

Eine Kopie in Portal-Tabellen ist nur zulässig, wenn sie als Cache klar gekennzeichnet und invalidierbar ist.

### 8.2 Nutzerstammdaten

SportFM-/RIB-FM-Nutzer bleiben fachlich führend.

Portal-Organisationen referenzieren diese Nutzer.

Änderungen an Stammdaten müssen entweder:

```text
direkt aus SportFM gelesen
oder kontrolliert in Portal-Sichten aktualisiert
```

werden.

### 8.3 Dokumente und Rechnungen

Dokumente und Rechnungen bleiben im Bestand.

Das Portal liest berechtigte Dokumente und Rechnungen über REST/API und darf keine Schattenkopie erzeugen.

---

## 9. Migrationsskripte

### 9.1 Skriptstruktur

Empfohlene Struktur:

```text
/db/migration
  /001_base
  /002_authentication
  /003_portaluser
  /004_organisation
  /005_context
  /006_wizard
  /007_application
  /008_workflow
  /009_upload
  /010_notification
  /011_dashboard
  /012_permissions
  /013_initial_data
```

### 9.2 Namenskonvention

```text
VYYYYMMDD_NNN__beschreibung.sql
```

Beispiel:

```text
V20260708_001__create_portal_user_tables.sql
V20260708_002__create_wizard_tables.sql
V20260708_003__insert_initial_roles.sql
```

### 9.3 Wiederholbarkeit

Skripte müssen so geschrieben werden, dass sie kontrolliert ausgeführt werden können.

Für produktive Migrationen gilt:

```text
Keine ungeprüften DROP-Anweisungen.
Keine destruktiven Änderungen ohne Rollback-Konzept.
Keine Änderung an Bestandsdaten ohne Protokollierung.
```

---

## 10. Teststrategie für Migration

### 10.1 Technische Migrationstests

Zu prüfen:

```text
Tabellen werden korrekt angelegt
Constraints sind vorhanden
Indizes sind vorhanden
Initialdaten sind vorhanden
Packages kompilieren
Berechtigungen sind gesetzt
```

### 10.2 Fachliche Migrationstests

Zu prüfen:

```text
Portalnutzer kann Organisation zugeordnet werden
Organisation referenziert SportFM-Nutzer
Dokumente werden nur berechtigten Nutzern angezeigt
Rechnungen werden nur berechtigten Nutzern angezeigt
Wizard erzeugt gültige Application
Application kann Workflow starten
Application kann später in SportFM übernommen werden
```

### 10.3 Regressionstests Bestand

Zu prüfen:

```text
bestehende Buchungen bleiben unverändert
bestehende Occurrence-/Winner-Logik funktioniert weiter
bestehende Dokumente bleiben abrufbar
bestehende Rechnungen bleiben abrufbar
bestehende SAP-Integration bleibt unverändert
```

---

## 11. Go-Live-Vorgehen

### 11.1 Vorbereitungsphase

```text
Datenbanksicherung erstellen
Migrationspaket bereitstellen
Skripte in Testumgebung ausführen
Abnahme der Testmigration
Go-Live-Freigabe einholen
```

### 11.2 Produktivmigration

```text
Wartungsfenster starten
Backup prüfen
DDL-Skripte ausführen
Initialdaten einspielen
Packages kompilieren
Berechtigungen setzen
Smoke-Test durchführen
Portal/API aktivieren
```

### 11.3 Nachlauf

```text
Monitoring aktivieren
Fehlerprotokolle prüfen
Login/Registrierung testen
Dokumentzugriff testen
Rechnungszugriff testen
Wizard-Testantrag stellen
Workflow-Test durchführen
```

---

## 12. Risiken

| Risiko | Bewertung | Maßnahme |
|---|---|---|
| Falsche Zuordnung PortalUser zu SportFM-Nutzer | hoch | Freischaltprozess, Vier-Augen-Prüfung, Audit |
| Unberechtigter Zugriff auf Dokumente/Rechnungen | sehr hoch | strenge Objektberechtigungen, Tests, Logging |
| Doppelte Fachlogik durch Portal-Tabellen | hoch | klare Referenzierungsregel, keine Schattenbuchungen |
| Fehlerhafte Wizard-Konfiguration | mittel | Versionierung, Testmodus, Freigabeprozess |
| Migration beschädigt Bestandsdaten | sehr hoch | keine destruktive Migration, Backup, Rollback |
| Performanceprobleme bei Verfügbarkeiten | mittel | Nutzung von OCC/WINNER, keine Live-Neuberechnung im Portal |
| Uneinheitliche Statusmodelle | mittel | zentrale Statusdefinition, Mapping-Dokumentation |

---

## 13. Offene Klärungspunkte

| Nr. | Punkt | Bedeutung |
|---:|---|---|
| 1 | Werden bestehende Ansprechpartner initial als Portalnutzer importiert? | Aufwand / Datenschutz / Freischaltung |
| 2 | Wie erfolgt die eindeutige Prüfung einer Organisationszuordnung? | Sicherheit / Missbrauchsschutz |
| 3 | Welche Dokumenttypen sollen sofort im Portal sichtbar sein? | Berechtigung / UI / Test |
| 4 | Welche Rechnungsinformationen werden im Portal angezeigt? | Datenschutz / ePayment |
| 5 | Wird ein Portal-Cache für Sportstätten benötigt? | Performance / Aktualität |
| 6 | Welche Wizard-Anträge gehören zum MVP? | Aufwand / Meilensteinplanung |
| 7 | Welche Uploads werden dauerhaft im Portal gespeichert? | Speicher / Aufbewahrung / Datenschutz |
| 8 | Wie lange werden Portalprotokolle aufbewahrt? | Datenschutz / Revision |
| 9 | Soll es eine technische Migration bestehender manueller Anträge geben? | Aufwand / Nutzen |

---

## 14. Aufwandrelevante Arbeitspakete

Aus der Datenmigration ergeben sich mindestens folgende Arbeitspakete:

```text
DM-01 Migrationskonzept finalisieren
DM-02 Tabellenmigration Portalbasis erstellen
DM-03 Initialdaten Rollen/Rechte erstellen
DM-04 Initialdaten Statuswerte erstellen
DM-05 Initialdaten Wizard-Konfiguration erstellen
DM-06 Initialdaten Workflow-Konfiguration erstellen
DM-07 Initialdaten Nachrichtenvorlagen erstellen
DM-08 Organisation-SportFM-Nutzer-Mapping implementieren
DM-09 Dokument-/Rechnungszugriff testen
DM-10 Migrationstestumgebung aufbauen
DM-11 Testmigration durchführen
DM-12 Go-Live-Migrationsskripte erstellen
DM-13 Rollback-Konzept dokumentieren
DM-14 Migrationsprotokollierung implementieren
DM-15 Abnahmetests Migration durchführen
```

---

## 15. Zusammenfassung

Die Datenmigration ist bewusst schlank zu halten.

Die vorhandene SportFM-Fachlogik wird nicht migriert, sondern weiterhin genutzt.

Neu aufgebaut werden ausschließlich die Portalstrukturen für:

```text
Authentifizierung
Portalnutzer
Organisationen
Kontexte
Wizard
Anträge
Workflows
Uploads
Benachrichtigungen
Dashboards
Berechtigungen
Audit
```

Die wichtigste Migrationsentscheidung lautet:

> Bestehende SportFM-Daten werden nicht dupliziert. Das Portal referenziert und nutzt sie über definierte Packages und REST-Services.

Damit bleibt SportFM führendes Fachsystem und wird gleichzeitig zur erweiterbaren Plattform für Portal, WPF, Fremdsysteme und zukünftige Clients.
