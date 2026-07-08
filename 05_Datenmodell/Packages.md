# Packages

> Status: Arbeitsstand für das Projekthandbuch SportFM  
> Kapitel: `05_Datenmodell/Packages.md`  
> Grundlage: Snapshot vom 05.07.2026, Lastenheft, Domänenkapitel, Kapitel `05_Datenmodell/Tabellen.md`, Oracle-Bestand aus `GIT_IMSP.zip` und fachliche Korrekturen zur bestehenden PL/SQL-Logik.

## 1. Ziel des Kapitels

Dieses Kapitel beschreibt die geplante PL/SQL-Package-Struktur für die SportFM-Plattform und das neue Onlineportal.

Es beantwortet insbesondere:

- welche bestehenden Packages fachlich führend bleiben,
- welche neuen Portal-Packages je Domäne benötigt werden,
- welche Tabellen durch welches Package fachlich verwaltet werden,
- welche Packages nur Fassaden auf bestehende SportFM-Logik sind,
- welche Package-Schnittstellen für REST, Portal und spätere WPF-Migration relevant sind,
- welche fachlichen Verantwortlichkeiten nicht in der REST- oder Portal-Schicht dupliziert werden dürfen.

Das Dokument ist **keine vollständige PL/SQL-Spezifikation**. Es definiert die fachliche Package-Architektur als Grundlage für spätere DDL-/Package-Spezifikationen, REST-Endpunkte, Arbeitspakete und Aufwandsschätzung.

---

## 2. Grundsatzentscheidung

Für alle Portal-Domänen werden eigene Oracle-Packages vorgesehen.

Die Packages sind bewusst **domänenorientiert** und nicht tabellenorientiert aufgebaut.

Nicht gewünscht:

```text
PA_LHD_SPA_PORT_CRUD_EVENTS
PA_LHD_SPA_PORT_CRUD_USER
PA_LHD_SPA_PORT_CRUD_FILES
```

Gewünscht:

```text
PA_LHD_SPA_PORT_AUTH
PA_LHD_SPA_PORT_APPLICATION
PA_LHD_SPA_PORT_WORKFLOW
PA_LHD_SPA_PORT_BOOKING
PA_LHD_SPA_PORT_DOCUMENT
```

Damit bildet die Datenbank dieselbe fachliche Struktur ab wie:

```text
03_Domaenen
04_REST_API
05_Datenmodell
06_Arbeitspakete
07_Kalkulation
```

---

## 3. Architekturprinzipien

### 3.1 Keine zweite Fachlogik im Portal

Das Portal berechnet keine Buchungen, Gebühren, Wiederholungen, Stornierungen oder freien Zeiten selbst.

Bestehende Fachlogik bleibt führend in:

| Package | Status | Fachliche Verantwortung |
|---|---|---|
| `PA_LHD_SPA` | Bestand | Buchungslogik, Wiederholungen, Stornierungen, Gebühren und weitere SportFM-Kernlogik |
| `PA_LHD_SPA_OCC` | Bestand | Occurrence-Ermittlung, Winner-Logik, performante Termin-/Belegungszugriffe |

Neue Portal-Packages kapseln, orchestrieren und berechtigen diese Logik.

### 3.2 REST ruft Packages, nicht Tabellen

Die REST-API greift nicht direkt auf Tabellen zu, sondern nutzt definierte Package-Schnittstellen.

```text
REST Controller
  ↓
Application Service / .NET
  ↓
Oracle Package
  ↓
Tabellen / bestehende Packages
```

### 3.3 Fachliche Operationen statt CRUD

Packages sollen fachliche Operationen anbieten.

Beispiele:

```text
submit_application
assign_task
approve_application
get_available_slots
create_reservation_from_application
get_user_documents
get_user_invoices
```

Nicht Ziel sind reine Tabellenoperationen wie:

```text
insert_row
update_row
delete_row
select_by_id
```

### 3.4 Portal-Packages als kontrollierte Zugriffsschicht

Die Portal-Packages erfüllen vier Aufgaben:

1. Portalrechte prüfen.
2. Portalvorgänge validieren.
3. Bestehende SportFM-Logik aufrufen.
4. Portalstatus, Workflow, Audit und Benachrichtigung fortschreiben.

### 3.5 Transaktionsgrenzen bleiben fachlich

Transaktionen werden an fachlichen Vorgängen ausgerichtet.

Beispiele:

| Vorgang | Transaktionsgrenze |
|---|---|
| Antrag einreichen | Antrag, Historie, Workflow, Audit, Benachrichtigung |
| Aufgabe abschließen | Task, Workflowstatus, Historie, Audit |
| Buchung aus Antrag erzeugen | Application, Booking, Occurrence-Fast-Path, Workflow, Dokument-/Benachrichtigungsauslöser |
| Upload zuordnen | File-Metadaten, File-Link, Audit |

---

## 4. Namenskonvention

Alle neuen Packages verwenden die bestehende SportFM-Konvention.

| Typ | Konvention | Beispiel |
|---|---|---|
| Bestandspackage | `PA_LHD_SPA...` | `PA_LHD_SPA` |
| Portalpackage | `PA_LHD_SPA_PORT_...` | `PA_LHD_SPA_PORT_APPLICATION` |
| Hilfspackage | `PA_LHD_SPA_PORT_...` | `PA_LHD_SPA_PORT_AUDIT` |

Package-Namen werden fachlich kurz gehalten.

---

## 5. Package-Gesamtübersicht

### 5.1 Bestehende SportFM-Packages

| Package | Status | Verantwortlichkeit | Nutzung im Portalprojekt |
|---|---|---|---|
| `PA_LHD_SPA` | Bestand | zentrale SportFM-Fachlogik | wird weiterverwendet und bei Bedarf gezielt erweitert |
| `PA_LHD_SPA_OCC` | Bestand | Occurrence-/Winner-Logik | wird für Kalender, freie Zeiten und Belegungssichten genutzt |

### 5.2 Neue Portal-Packages

| Package | Domäne | Status | Zweck |
|---|---|---|---|
| `PA_LHD_SPA_PORT_AUTH` | Authentication | neu | Registrierung, Login, Token, Passwort, 2FA-Anbindung |
| `PA_LHD_SPA_PORT_USER` | PortalUser | neu | Portalnutzerprofil, Benutzerstatus, Benutzerverwaltung |
| `PA_LHD_SPA_PORT_ORG` | Organisation | neu | Organisationen, Abteilungen, Einladungen, Mitgliedschaften |
| `PA_LHD_SPA_PORT_CONTEXT` | Context | neu | aktiver Nutzungskontext, Zuordnung zum SportFM-Nutzer |
| `PA_LHD_SPA_PORT_WIZARD` | Wizard | neu | Wizard-Konfiguration, Antragstypen, Validierungs- und Mappinginformationen |
| `PA_LHD_SPA_PORT_APPLICATION` | Application | neu | digitale Anträge, Antragshistorie, Einreichung, fachliche Übergabe |
| `PA_LHD_SPA_PORT_WORKFLOW` | Workflow | neu | Status, Aufgaben, Arbeitskorb, Rückfragen, Entscheidungen |
| `PA_LHD_SPA_PORT_UPLOAD` | Upload | neu | Upload-Metadaten, Dateizuordnung, Datei-Lifecycle |
| `PA_LHD_SPA_PORT_NOTIFICATION` | Notification | neu | Portalnachrichten, Mailqueue, Benachrichtigungsauslösung |
| `PA_LHD_SPA_PORT_DASHBOARD` | Dashboard | neu | Dashboarddaten und domänenübergreifende Statusübersichten |
| `PA_LHD_SPA_PORT_ADMIN` | Administration | neu | administrative Portalverwaltung, Konfiguration, Betriebsfunktionen |
| `PA_LHD_SPA_PORT_FACILITY` | Facility | neu | Portalzugriff auf Sportanlagen, Teileinheiten, Ausstattung, Suchdaten |
| `PA_LHD_SPA_PORT_AVAILABILITY` | Availability | neu | freie Zeiten, Kalenderdaten, Belegungsansichten über Occurrence/Winner |
| `PA_LHD_SPA_PORT_BOOKING` | Booking | neu | Portalnahe Buchungsoperationen, Reservierung, Übergabe an Bestand |
| `PA_LHD_SPA_PORT_CHARGE` | Charge | neu | Portalnahe Gebührenauskunft über bestehende Gebührenlogik |
| `PA_LHD_SPA_PORT_INVOICE` | Invoice | neu | Rechnungsanzeige, SAP-/ePayment-nahe Portalsicht |
| `PA_LHD_SPA_PORT_DOCUMENT` | Document | neu | Dokumentanzeige, Download, Dokumentbezug im Portal |
| `PA_LHD_SPA_PORT_REF` | ReferenceData | neu | Auswahldaten, Stammdatenlisten, Lookup-Werte |
| `PA_LHD_SPA_PORT_AUDIT` | Audit / Revision | neu | fachliches Audit, Zugriffsnachweise, Revisionsprotokolle |
| `PA_LHD_SPA_PORT_LOG` | Logging | neu / optional | technisches Portal-Logging, Fehlerprotokollierung |

---

## 6. Package-Muster je Domäne

Jedes Portal-Package folgt demselben fachlichen Aufbau.

```text
PA_LHD_SPA_PORT_<DOMAIN>
  ├─ öffentliche fachliche Prozeduren/Funktionen
  ├─ interne Validierungen
  ├─ Status- und Rechteprüfung
  ├─ Audit- und Logging-Aufrufe
  ├─ Fehlerbehandlung
  └─ Rückgabe an REST-Service
```

### 6.1 Standardparameter

Soweit sinnvoll, erhalten fachliche Operationen folgende Standardparameter:

| Parameter | Bedeutung |
|---|---|
| `p_actor_user_id` | Portalnutzer, der die Operation auslöst |
| `p_context_id` | aktiver Portal-/Organisationskontext |
| `p_correlation_id` | technische Korrelation REST → DB → Log |
| `p_client` | aufrufender Client, z. B. Portal, WPF, API-Client |
| `p_payload` | strukturierte Eingabe als JSON/CLOB, falls fachlich sinnvoll |
| `p_result` | Rückgabe als Cursor, JSON/CLOB oder definierter OUT-Parameter |

### 6.2 Standardfelder in neuen Tabellen

Neue Portal-Tabellen sollten einheitlich Audit- und Statusfelder enthalten.

| Feld | Zweck |
|---|---|
| `ID_...` | Primärschlüssel |
| `STATE` | fachlicher Zustand / aktiv / gelöscht / gesperrt |
| `CREATED_AT` | Anlagezeitpunkt |
| `CREATED_BY` | anlegender Benutzer |
| `UPDATED_AT` | Änderungszeitpunkt |
| `UPDATED_BY` | ändernder Benutzer |
| `VERSION` | optimistische Sperre / fachliche Version |
| `TENANT_CONTEXT_ID` oder `ID_CONTEXT` | Kontextbezug, falls fachlich erforderlich |

---

## 7. Bestehende Packages

## 7.1 `PA_LHD_SPA`

### Status

Bestand, fachlich führend.

### Verantwortung

`PA_LHD_SPA` enthält die bestehende zentrale SportFM-Fachlogik.

Dazu gehören nach aktuellem Kenntnisstand insbesondere:

- Wiederholungen,
- Stornierungen,
- Gebühren,
- Buchungslogik,
- zentrale Hilfsfunktionen im Bestand.

### Portalrelevanz

Dieses Package wird nicht ersetzt.

Portal-Packages rufen bestehende Logik aus `PA_LHD_SPA` auf, wenn fachliche Operationen auf Buchungen, Gebühren oder Stornierungen wirken.

### Typische Aufrufe aus neuen Packages

```text
PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA

PA_LHD_SPA_PORT_CHARGE
  → PA_LHD_SPA.p_get_charges

PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA
```

### Nicht-Ziele

- keine direkte Nutzung durch Portal-Frontend,
- keine Freigabe als REST-nahe Tabellen-API,
- keine Duplizierung in .NET.

---

## 7.2 `PA_LHD_SPA_OCC`

### Status

Bestand, fachlich führend.

### Verantwortung

`PA_LHD_SPA_OCC` enthält die performante Termin- und Belegungslogik.

Dazu gehören:

- Ermittlung von Occurrences,
- Befüllung / Aktualisierung von `LHD_SPA_OCC`,
- Winner-Logik,
- performante Kalender- und Belegungszugriffe,
- stündliche Aktualisierung,
- Fast Path für kurzfristige Aktualisierung.

### Portalrelevanz

Dieses Package ist die fachliche Grundlage für:

- freie Zeiten,
- Kalenderansichten,
- Belegungsvorschau,
- Verfügbarkeitsprüfung,
- Kollisions-/Belegungsinformationen.

### Typische Aufrufe aus neuen Packages

```text
PA_LHD_SPA_PORT_AVAILABILITY
  → PA_LHD_SPA_OCC

PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA_OCC Fast Path nach buchungsrelevanter Änderung
```

---

## 8. Neue Portal-Packages im Detail

## 8.1 `PA_LHD_SPA_PORT_AUTH`

### Domäne

Authentication

### Verantwortung

Authentifizierung und technische Sitzung des Portalnutzers.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_USER` | Loginstatus, E-Mail, Passwortstatus, Benutzerstatus |
| `LHD_SPA_PORT_REFRESH_TOKEN` | Refresh Tokens / Sitzungen |
| `LHD_SPA_PORT_PASSWORD_RESET` | Passwort-zurücksetzen-Vorgänge |
| `LHD_SPA_PORT_AUDIT` | sicherheitsrelevante Audit-Einträge |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `register_user` | neuen Portalnutzer registrieren |
| `confirm_email` | E-Mail-Adresse bestätigen |
| `login_user` | Login prüfen und Sitzung vorbereiten |
| `create_refresh_token` | Refresh Token erzeugen |
| `validate_refresh_token` | Refresh Token prüfen |
| `revoke_refresh_token` | Refresh Token widerrufen |
| `request_password_reset` | Passwort-Reset anstoßen |
| `reset_password` | Passwort neu setzen |
| `lock_user` | Benutzer sperren |
| `unlock_user` | Benutzer entsperren |
| `record_failed_login` | Fehlversuch protokollieren |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_AUTH
  → PA_LHD_SPA_PORT_USER
  → PA_LHD_SPA_PORT_NOTIFICATION
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

- 2FA und BundID können fachlich angebunden werden, ohne dass die Bestandslogik geändert wird.
- Passwort-Hashing und Token-Ausstellung können teilweise in der .NET-Schicht liegen; das Package verwaltet die persistente Sicherheits- und Sitzungsstruktur.

---

## 8.2 `PA_LHD_SPA_PORT_USER`

### Domäne

PortalUser

### Verantwortung

Fachliches Portalnutzerprofil und Benutzerverwaltung.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_USER` | Stammdaten Portalnutzer |
| `LHD_SPA_PORT_MEMBERSHIP` | Kontext der Organisationszugehörigkeit |
| `LHD_SPA_PORT_MEMBERSHIP_ROLE` | Rollen im Organisationskontext |
| `LHD_SPA_PORT_AUDIT` | Änderungen am Benutzerprofil |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_user_profile` | eigenes Benutzerprofil laden |
| `update_user_profile` | eigenes Profil ändern |
| `get_user_status` | Benutzerstatus prüfen |
| `set_user_state` | Status ändern, z. B. aktiv, gesperrt, gelöscht |
| `get_user_memberships` | Organisationen / Abteilungen des Benutzers laden |
| `get_user_permissions` | wirksame Rechte des Benutzers ermitteln |
| `delete_or_deactivate_user` | Benutzer datenschutzkonform deaktivieren |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_USER
  → PA_LHD_SPA_PORT_ORG
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_AUDIT
```

---

## 8.3 `PA_LHD_SPA_PORT_ORG`

### Domäne

Organisation

### Verantwortung

Organisationen, Abteilungen, Mitgliedschaften und Einladungen im Portal.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_ORGANIZATION` | Organisation / Verein / Firma / Schule / Kita / Personenkontext |
| `LHD_SPA_PORT_DEPARTMENT` | Abteilungen innerhalb einer Organisation |
| `LHD_SPA_PORT_MEMBERSHIP` | Zuordnung Portalnutzer zu Organisation / Abteilung |
| `LHD_SPA_PORT_MEMBERSHIP_ROLE` | Rollen je Mitgliedschaft |
| `LHD_SPA_PORT_INVITATION` | Einladungen weiterer Benutzer |
| `LHD_SPA_PORT_CONTEXT` | fachliche Verknüpfung zum SportFM-Nutzerkontext |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `create_organization` | Organisation anlegen |
| `update_organization` | Organisation ändern |
| `create_department` | Abteilung anlegen |
| `update_department` | Abteilung ändern |
| `invite_member` | Portalnutzer einladen |
| `accept_invitation` | Einladung annehmen |
| `assign_role` | Rolle zuweisen |
| `remove_role` | Rolle entfernen |
| `remove_member` | Mitgliedschaft beenden |
| `get_organization_members` | Mitglieder einer Organisation laden |
| `get_organization_contexts` | zugeordnete SportFM-Kontexte laden |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_ORG
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_NOTIFICATION
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Große Vereine können mehrere Abteilungen und mehrere berechtigte Personen besitzen. Diese Portalstruktur ist nicht identisch mit dem bestehenden SportFM-Nutzer, sondern wird über den Context an den Bestand angebunden.

---

## 8.4 `PA_LHD_SPA_PORT_CONTEXT`

### Domäne

Context

### Verantwortung

Sichtbarkeits- und Nutzungskontext zwischen Portalnutzer, Organisation, Abteilung und bestehendem SportFM-Nutzer.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_CONTEXT` | Mapping Portalorganisation/-abteilung zu SportFM-Nutzerkontext |
| `LHD_SPA_PORT_MEMBERSHIP` | berechtigte Benutzer im Kontext |
| `LHD_SPA_PORT_AUDIT` | Kontextwechsel und Rechteprüfung |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_available_contexts` | verfügbare Kontexte eines Benutzers laden |
| `set_active_context` | aktiven Kontext setzen / prüfen |
| `validate_context_access` | Zugriff auf Kontext prüfen |
| `resolve_sportfm_user` | SportFM-Nutzer zum Portal-Kontext ermitteln |
| `can_access_document` | Dokumentzugriff fachlich prüfen |
| `can_access_invoice` | Rechnungszugriff fachlich prüfen |
| `can_submit_application` | Antragseinreichung im Kontext prüfen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_USER
  → PA_LHD_SPA_PORT_ORG
  → PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_INVOICE
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Dieses Package ist sicherheitskritisch. Es verhindert, dass Portalnutzer Dokumente, Rechnungen, Anträge oder Buchungen fremder Organisationen sehen.

---

## 8.5 `PA_LHD_SPA_PORT_WIZARD`

### Domäne

Wizard

### Verantwortung

Konfigurierbares Antragsframework für unterschiedliche Antragstypen.

### Verwaltete Tabellen

Für V1 kann die Wizard-Konfiguration zunächst als JSON-/Konfigurationsstruktur geführt werden. Für eine langfristige 10-Jahres-Plattform sollte das Package jedoch bereits als fachlicher Einstiegspunkt vorgesehen werden.

Mögliche spätere Tabellen:

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_WIZARD_DEF` | Wizard-Definition je Antragstyp |
| `LHD_SPA_PORT_WIZARD_STEP` | Schritte |
| `LHD_SPA_PORT_WIZARD_FIELD` | Felder |
| `LHD_SPA_PORT_WIZARD_RULE` | Sichtbarkeit / Validierung / Bedingungen |
| `LHD_SPA_PORT_WIZARD_MAP` | Mapping Antrag → SportFM / Application Payload |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_wizard_definition` | Wizard-Konfiguration für Antragstyp laden |
| `validate_step` | Schrittvalidierung durchführen |
| `validate_application_payload` | vollständigen Antrag fachlich validieren |
| `get_next_step` | nächste Seite / Schrittlogik ermitteln |
| `build_summary` | Zusammenfassung erzeugen |
| `map_payload_to_application` | Wizarddaten in Application-Struktur überführen |
| `get_required_uploads` | notwendige Uploads je Antragstyp ermitteln |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_WIZARD
  → PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_REF
  → PA_LHD_SPA_PORT_UPLOAD
```

### Hinweise

Der Wizard wird nicht als einzelnes hart programmiertes Formular verstanden, sondern als langfristig konfigurierbares Antragsframework. Die Package-Struktur sollte deshalb von Anfang an vorhanden sein, auch wenn V1 zunächst mit einer reduzierten Konfigurationsspeicherung startet.

---

## 8.6 `PA_LHD_SPA_PORT_APPLICATION`

### Domäne

Application

### Verantwortung

Digitale Anträge aus dem Portal.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_APPLICATION` | Antrag, Antragstyp, Status, Payload, Kontext |
| `LHD_SPA_PORT_APPLICATION_HISTORY` | fachliche Historie des Antrags |
| `LHD_SPA_PORT_FILE_LINK` | Upload-Zuordnung |
| `LHD_SPA_PORT_WORKFLOW` | Workflowstatus |
| `LHD_SPA_PORT_TASK` | Arbeitskorbaufgaben |
| `LHD_SPA_PORT_AUDIT` | Revisionsprotokoll |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `create_draft` | Antragsentwurf anlegen |
| `update_draft` | Antragsentwurf speichern |
| `get_application` | Antrag laden |
| `get_applications_for_context` | Anträge eines Kontextes laden |
| `submit_application` | Antrag einreichen |
| `withdraw_application` | Antrag zurückziehen |
| `return_for_correction` | Antrag zur Korrektur zurückgeben |
| `change_application_state` | Statuswechsel durchführen |
| `append_history` | Historieneintrag schreiben |
| `link_file` | Upload zu Antrag zuordnen |
| `prepare_booking_transfer` | fachliche Übergabe an Reservierung/Buchung vorbereiten |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_WIZARD
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_WORKFLOW
  → PA_LHD_SPA_PORT_UPLOAD
  → PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA_PORT_NOTIFICATION
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Der Antrag ist noch keine Buchung und noch keine Reservierung. Er ist ein digitaler Vorgang, der nach Prüfung in interne Planung, Reservierung oder Buchung überführt werden kann.

---

## 8.7 `PA_LHD_SPA_PORT_WORKFLOW`

### Domäne

Workflow

### Verantwortung

Status, Aufgaben, Arbeitskorb, Rückfragen, Entscheidungen und fachliche Vorgangshistorie.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_WORKFLOW` | Workflowinstanz / Status zum Vorgang |
| `LHD_SPA_PORT_TASK` | Aufgaben und Arbeitskorb |
| `LHD_SPA_PORT_APPLICATION_HISTORY` | fachliche Antragshistorie |
| `LHD_SPA_PORT_NOTIFICATION` | Benachrichtigungen bei Statuswechsel |
| `LHD_SPA_PORT_AUDIT` | revisionsrelevante Workflowaktionen |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `start_workflow` | Workflow zu Antrag starten |
| `get_workflow_state` | aktuellen Status laden |
| `assign_task` | Aufgabe zuweisen |
| `claim_task` | Aufgabe übernehmen |
| `complete_task` | Aufgabe abschließen |
| `request_information` | Rückfrage an Portalnutzer erstellen |
| `answer_information_request` | Rückfrage beantworten |
| `approve` | fachlich freigeben |
| `reject` | fachlich ablehnen |
| `cancel_workflow` | Workflow abbrechen |
| `get_workbasket` | Arbeitskorb laden |
| `get_workflow_history` | Verlauf laden |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_WORKFLOW
  → PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_NOTIFICATION
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Die Workflow-Logik sollte nicht nur für den ersten Portal-Antrag funktionieren, sondern als Plattformdienst für spätere Antragsarten wiederverwendbar sein.

---

## 8.8 `PA_LHD_SPA_PORT_UPLOAD`

### Domäne

Upload

### Verantwortung

Dateiannahme, Metadaten und Zuordnung von Uploads zu Anträgen oder Workflowvorgängen.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_FILE` | Datei-Metadaten |
| `LHD_SPA_PORT_FILE_LINK` | Zuordnung Datei zu Antrag / Vorgang / Workflow |
| `LHD_SPA_PORT_AUDIT` | Upload- und Downloadereignisse |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `create_file_metadata` | Datei-Metadaten anlegen |
| `link_file_to_application` | Datei einem Antrag zuordnen |
| `link_file_to_task` | Datei einer Aufgabe / Rückfrage zuordnen |
| `get_files_for_application` | Uploads eines Antrags laden |
| `mark_file_deleted` | Datei logisch löschen |
| `validate_file_access` | Zugriff auf Datei prüfen |
| `promote_to_document` | Upload bei Bedarf in dauerhaftes Dokument überführen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_UPLOAD
  → PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_WORKFLOW
  → PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Upload ist nicht identisch mit Document. Uploads sind eingereichte Dateien. Dokumente sind dauerhaft verwaltete SportFM-Dokumente, Bescheide, Genehmigungen oder Rechnungsdokumente.

---

## 8.9 `PA_LHD_SPA_PORT_NOTIFICATION`

### Domäne

Notification

### Verantwortung

Portalnachrichten, Mailqueue und Benachrichtigungsauslösung.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_NOTIFICATION` | Portalnachrichten |
| `LHD_SPA_PORT_MAIL_QUEUE` | ausgehende E-Mails |
| `LHD_SPA_PORT_MAIL_TEMPLATE` | Mailvorlagen |
| `LHD_SPA_PORT_AUDIT` | Benachrichtigungsaudit |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `create_notification` | Portalnachricht erzeugen |
| `mark_notification_read` | Nachricht als gelesen markieren |
| `get_notifications_for_user` | Nachrichten eines Benutzers laden |
| `enqueue_mail` | Mail in Queue stellen |
| `get_pending_mails` | offene Mails für Maildienst laden |
| `mark_mail_sent` | Mail als versendet markieren |
| `mark_mail_failed` | Mailfehler protokollieren |
| `render_mail_template` | Mailvorlage mit Daten befüllen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_NOTIFICATION
  ← PA_LHD_SPA_PORT_AUTH
  ← PA_LHD_SPA_PORT_APPLICATION
  ← PA_LHD_SPA_PORT_WORKFLOW
  ← PA_LHD_SPA_PORT_DOCUMENT
  ← PA_LHD_SPA_PORT_INVOICE
```

### Hinweise

Der Versand selbst kann über einen .NET-Hintergrunddienst erfolgen. Das Package verwaltet Queue, Status und fachlichen Versandkontext.

---

## 8.10 `PA_LHD_SPA_PORT_DASHBOARD`

### Domäne

Dashboard

### Verantwortung

Verdichtete Startseiten- und Statusinformationen für Portalnutzer und Sachbearbeitung.

### Verwaltete Tabellen

In V1 sind keine zwingenden eigenen Dashboard-Tabellen erforderlich. Das Package aggregiert Daten aus anderen Domänen.

Mögliche spätere Tabellen:

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_DASHBOARD_PREF` | benutzerspezifische Dashboard-Einstellungen |
| `LHD_SPA_PORT_DASHBOARD_CACHE` | optionale Performance-Zwischenspeicherung |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_portal_dashboard` | Dashboard für Portalnutzer laden |
| `get_admin_dashboard` | Dashboard für Sachbearbeitung / Admin laden |
| `get_open_tasks_summary` | offene Aufgaben zusammenfassen |
| `get_application_summary` | Antragsstatus zusammenfassen |
| `get_document_summary` | neue Dokumente zusammenfassen |
| `get_invoice_summary` | Rechnungsübersicht zusammenfassen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_DASHBOARD
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_WORKFLOW
  → PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_INVOICE
  → PA_LHD_SPA_PORT_NOTIFICATION
```

---

## 8.11 `PA_LHD_SPA_PORT_ADMIN`

### Domäne

Administration

### Verantwortung

Administrative Portal- und Betriebsfunktionen.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_USER` | Benutzerverwaltung |
| `LHD_SPA_PORT_ORGANIZATION` | Organisationsverwaltung |
| `LHD_SPA_PORT_CONTEXT` | Kontextzuordnung |
| `LHD_SPA_PORT_MAIL_TEMPLATE` | Mailvorlagen |
| `LHD_SPA_PORT_AUDIT` | Audit-/Revisionssuche |
| `LHD_SPA_PORT_NOTIFICATION` | Systemnachrichten |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `search_users` | Portalnutzer suchen |
| `set_user_state` | Benutzer sperren / entsperren / deaktivieren |
| `search_organizations` | Organisationen suchen |
| `approve_organization` | Organisation freischalten |
| `maintain_context_mapping` | Portalorganisation mit SportFM-Nutzer verbinden |
| `get_audit_entries` | Audit anzeigen |
| `get_system_health` | fachliche System-/Queueübersicht laden |
| `requeue_failed_mail` | fehlgeschlagene Mail erneut einreihen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_ADMIN
  → PA_LHD_SPA_PORT_USER
  → PA_LHD_SPA_PORT_ORG
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_NOTIFICATION
  → PA_LHD_SPA_PORT_AUDIT
```

---

## 8.12 `PA_LHD_SPA_PORT_FACILITY`

### Domäne

Facility

### Verantwortung

Portalnahe Sicht auf Sportanlagen, Teileinheiten, Sportkomplexe, Ausstattung und Suchinformationen.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| RIB FM Objekt-/Stammdatentabellen | führende Sportanlagen- und Teileinheitsdaten |
| `LHD_SPA_SPORTSCOMPLEXES` | Sportkomplexe |
| `LHD_SPA_FACILITY2COMPLEX` | Zuordnung Sportanlage zu Sportkomplex |
| `LHD_SPA_FACILITYGROUPS` | Sportanlagengruppen |
| `LHD_SPA_PORT_AUDIT` | optionales Zugriffsaudit |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `search_facilities` | Sportanlagen für Portal suchen |
| `get_facility_detail` | Detaildaten einer Sportanlage laden |
| `get_facility_units` | buchbare Teileinheiten laden |
| `get_facility_complexes` | Sportkomplexe laden |
| `get_facility_filters` | Filterdaten für Suche bereitstellen |
| `get_facility_for_map` | Daten für Themenstadtplan / Kartendarstellung bereitstellen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_FACILITY
  → PA_LHD_SPA_PORT_REF
  → PA_LHD_SPA_PORT_AVAILABILITY
```

### Hinweise

Facility pflegt keine zweite Sportanlagenwelt. Führend bleiben RIB FM / SportFM-Stammdaten.

---

## 8.13 `PA_LHD_SPA_PORT_AVAILABILITY`

### Domäne

Availability

### Verantwortung

Freie Zeiten, Kalenderdaten, Belegungssichten und Verfügbarkeitsprüfung.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_OCC` | konkrete Occurrences |
| `LHD_SPA_OCC_WINNER` | resultierende Belegung |
| `LHD_SPA_OCC_DAY_COVERAGE` | Berechnungsabdeckung |
| `LHD_SPA_EVENTS` | Eventdetails bei Bedarf |
| `LHD_SPA_EVENT2UNIT` | Teileinheitenzuordnung |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_calendar` | Kalenderdaten laden |
| `get_occupancy` | Belegung für Zeitraum / Anlage / Einheit laden |
| `search_available_slots` | freie Zeiten suchen |
| `check_slot_availability` | konkretes Zeitfenster prüfen |
| `get_conflicts` | Konflikte zu gewünschtem Zeitraum ermitteln |
| `get_day_coverage` | Berechnungsstand prüfen |
| `request_fast_path` | Fast-Path-Neuberechnung anstoßen, falls erforderlich |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_AVAILABILITY
  → PA_LHD_SPA_OCC
  → PA_LHD_SPA_PORT_FACILITY
  → PA_LHD_SPA_PORT_BOOKING
```

### Hinweise

Die freie-Zeiten-Suche nutzt zwingend die bestehende Occurrence-/Winner-Logik. Keine zweite Berechnung im Portal.

---

## 8.14 `PA_LHD_SPA_PORT_BOOKING`

### Domäne

Booking

### Verantwortung

Portalnahe Buchungsoperationen, Reservierung, Überführung geprüfter Anträge in SportFM-Buchungslogik.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_EVENTS` | bestehender Buchungskern |
| `LHD_SPA_EVENT2UNIT` | Zuordnung zu Teileinheiten |
| `LHD_SPA_RECURRING_PATTERN` | Wiederholungsmuster |
| `LHD_SPA_EVENTTYPES` | Eventtypen |
| `LHD_SPA_OCC` | berechnete Vorkommen |
| `LHD_SPA_OCC_WINNER` | resultierende Belegung |
| `LHD_SPA_PORT_APPLICATION` | Antrag als Ausgangspunkt |
| `LHD_SPA_PORT_WORKFLOW` | Status und Arbeitskorb |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_booking` | Buchungsdetail laden |
| `get_bookings_for_context` | Buchungen des aktiven Kontextes laden |
| `create_reservation_from_application` | Reservierung/Planung aus Antrag erzeugen |
| `create_booking_from_application` | verbindliche Buchung aus Antrag erzeugen |
| `create_cancellation_event` | Stornierung als eigenen Event erzeugen |
| `update_booking_period` | Zeitraum fachlich ändern |
| `end_booking` | Buchung zu Datum enden lassen |
| `get_booking_documents` | Dokumentbezug zu Buchung laden |
| `recalculate_occurrences` | Fast Path / Neuberechnung anstoßen |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA
  → PA_LHD_SPA_OCC
  → PA_LHD_SPA_PORT_APPLICATION
  → PA_LHD_SPA_PORT_AVAILABILITY
  → PA_LHD_SPA_PORT_CHARGE
  → PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Stornierungen sind fachlich Buchungen vom Typ Stornierung. Sie dürfen nicht als einfache Statusänderung modelliert werden.

---

## 8.15 `PA_LHD_SPA_PORT_CHARGE`

### Domäne

Charge

### Verantwortung

Portalnahe Gebührenauskunft und Kapselung bestehender Gebührenlogik.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_CHARGES` | Gebührenregeln |
| `LHD_SPA_CHARGETYPES` | Gebührentypen |
| `LHD_SPA_CHARGEGROUPS` | Gebührengruppen |
| `LHD_SPA_CHARGE2FACILITYGROUP` | Anlagen-/Gebührenzuordnung |
| `LHD_SPA_EVENTCHARGES` | Eventgebühren |
| `LHD_SPA_INVOICE_CHARGEINFOS` | abgerechnete Gebühreninformationen |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_charge_preview` | Gebührenvorschau, soweit fachlich zulässig |
| `get_charges_for_booking` | Gebühren einer Buchung laden |
| `get_charge_rules` | Gebührenregeln für interne / administrative Zwecke laden |
| `calculate_booking_charges` | bestehende Gebührenlogik kapseln |
| `get_invoice_charge_infos` | abgerechnete Gebührenpositionen laden |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_CHARGE
  → PA_LHD_SPA.p_get_charges
  → PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA_PORT_INVOICE
```

### Hinweise

Das Portal berechnet Gebühren nicht selbst. Gebühren werden durch SportFM bereitgestellt oder berechnet.

---

## 8.16 `PA_LHD_SPA_PORT_INVOICE`

### Domäne

Invoice

### Verantwortung

Rechnungsanzeige im Portal und vorbereitende Schnittstelle für ePayment-nahe Vorgänge.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_INVOICES` | Rechnungskopf |
| `LHD_SPA_INVOICE_CHARGEINFOS` | Rechnungspositionen / Gebühreninformationen |
| `LHD_SPA_DOCUMENTS` | Rechnungsdokumente |
| `LHD_SPA_PORT_CONTEXT` | Zugriffskontext |
| `LHD_SPA_PORT_AUDIT` | Zugriffsaudit |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_invoices_for_context` | Rechnungen des aktiven Kontextes laden |
| `get_invoice_detail` | Rechnung mit Positionen laden |
| `get_invoice_document` | zugehöriges Rechnungsdokument ermitteln |
| `validate_invoice_access` | Zugriff prüfen |
| `get_payment_status` | Zahlungsstatus bereitstellen, falls vorhanden |
| `prepare_epayment` | ePayment-Prozess fachlich vorbereiten, sofern in Scope |
| `record_payment_callback` | Zahlungsrückmeldung verarbeiten, sofern in Scope |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_INVOICE
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_CHARGE
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Die SAP-Integration existiert bereits über REST/OAuth an SAP. Das Portal erzeugt keine eigene Rechnungslogik.

---

## 8.17 `PA_LHD_SPA_PORT_DOCUMENT`

### Domäne

Document

### Verantwortung

Dokumentanzeige, Dokumentdownload und Dokumentbezug im Portal.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_DOCUMENTS` | Dokumente |
| `LHD_SPA_DOCUMENTS_EVENTS` | Dokumente zu Events |
| `LHD_SPA_DOCUMENT_TYPES` | Dokumenttypen |
| `LHD_SPA_DOCUMENT_TEMPLATES` | Dokumentvorlagen |
| `LHD_SPA_DOCUMENT_TEXTMODULES` | Textbausteine |
| `LHD_SPA_DOCUMENT_NUMBERS` | Dokumentnummern |
| `LHD_SPA_PORT_CONTEXT` | Zugriffskontext |
| `LHD_SPA_PORT_AUDIT` | Download-/Zugriffsaudit |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_documents_for_context` | Dokumente des aktiven Kontextes laden |
| `get_document_detail` | Dokumentmetadaten laden |
| `get_document_content` | Dokumentinhalt für Download bereitstellen |
| `get_documents_for_booking` | Dokumente zu Buchung laden |
| `get_documents_for_invoice` | Dokumente zu Rechnung laden |
| `validate_document_access` | Zugriff prüfen |
| `record_document_download` | Download protokollieren |
| `create_document_from_template` | Dokument aus Vorlage erzeugen, sofern fachlich in Scope |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_DOCUMENT
  → PA_LHD_SPA_PORT_CONTEXT
  → PA_LHD_SPA_PORT_BOOKING
  → PA_LHD_SPA_PORT_INVOICE
  → PA_LHD_SPA_PORT_AUDIT
```

### Hinweise

Grundsatz: Alle eigenen Dokumente sind grundsätzlich portalrelevant. Die Begrenzung erfolgt über Kontext- und Rechteprüfung, nicht über eine zweite Dokumentenablage.

---

## 8.18 `PA_LHD_SPA_PORT_REF`

### Domäne

ReferenceData

### Verantwortung

Auswahldaten, Stammdatenlisten und Lookup-Werte für Portal, Wizard und REST.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_EVENTTYPES` | Eventtypen |
| `LHD_SPA_EVENTCLASSES` | Eventklassen |
| `LHD_SPA_SPORTCATEGORIES` | Sportkategorien |
| `LHD_SPA_SPORTGROUPS` | Sportgruppen |
| `LHD_SPA_SPORTSUBGROUPS` | Sportuntergruppen |
| `LHD_SPA_SPORTTYPES` | Sportarten |
| `LHD_SPA_FACILITYGROUPS` | Sportanlagengruppen |
| `LHD_SPA_DOCUMENT_TYPES` | Dokumenttypen |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `get_sport_types` | Sportarten laden |
| `get_sport_groups` | Sportgruppen laden |
| `get_event_types` | Eventtypen laden |
| `get_facility_groups` | Sportanlagengruppen laden |
| `get_document_types` | Dokumenttypen laden |
| `get_application_types` | Antragstypen laden |
| `get_lookup_bundle` | gebündelte Auswahldaten für Portal laden |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_REF
  ← PA_LHD_SPA_PORT_WIZARD
  ← PA_LHD_SPA_PORT_FACILITY
  ← PA_LHD_SPA_PORT_BOOKING
  ← PA_LHD_SPA_PORT_APPLICATION
```

---

## 8.19 `PA_LHD_SPA_PORT_AUDIT`

### Domäne

Audit / Revision

### Verantwortung

Fachliches Audit und revisionsrelevante Protokollierung für Portalvorgänge.

### Verwaltete Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_PORT_AUDIT` | fachliches Audit |
| `LHD_SPA_LOGGING` | bestehendes Logging, falls technisch sinnvoll angebunden |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `write_audit` | Audit-Eintrag schreiben |
| `get_audit_for_object` | Audit zu Objekt laden |
| `get_audit_for_user` | Audit zu Benutzer laden |
| `get_security_audit` | sicherheitsrelevante Ereignisse laden |
| `purge_audit_if_allowed` | Lösch-/Aufbewahrungslogik, falls zulässig |

### Schnittstellen zu anderen Packages

```text
PA_LHD_SPA_PORT_AUDIT
  ← alle Portal-Packages
```

### Hinweise

Audit ist nicht optional. Gerade Dokumentzugriff, Rechnungszugriff, Antragseinreichung, Statuswechsel, Kontextwechsel und administrative Eingriffe müssen nachvollziehbar bleiben.

---

## 8.20 `PA_LHD_SPA_PORT_LOG`

### Domäne

Logging / Betrieb

### Verantwortung

Technisches Portal-Logging, Fehlerprotokollierung und Betriebsdiagnose.

### Verwaltete / genutzte Tabellen

| Tabelle | Nutzung |
|---|---|
| `LHD_SPA_LOGGING` | bestehendes Logging |
| optionale neue Portal-Logtabelle | falls Trennung fachlich/technisch erforderlich wird |

### Zentrale Operationen

| Operation | Zweck |
|---|---|
| `write_log` | technischen Logeintrag schreiben |
| `write_error` | Fehler protokollieren |
| `get_logs_by_correlation` | Logs zu Korrelations-ID laden |
| `get_recent_errors` | aktuelle Fehler laden |

### Hinweise

Dieses Package kann auch mit `PA_LHD_SPA_PORT_AUDIT` zusammengeführt werden, wenn kein separates technisches Portal-Logging benötigt wird. Fachlich sollten Audit und technisches Logging jedoch unterschieden bleiben.

---

## 9. Domäne → Tabellen → Package

| Domäne | Package | Zentrale Tabellen |
|---|---|---|
| Authentication | `PA_LHD_SPA_PORT_AUTH` | `LHD_SPA_PORT_USER`, `LHD_SPA_PORT_REFRESH_TOKEN`, `LHD_SPA_PORT_PASSWORD_RESET` |
| PortalUser | `PA_LHD_SPA_PORT_USER` | `LHD_SPA_PORT_USER`, `LHD_SPA_PORT_MEMBERSHIP`, `LHD_SPA_PORT_MEMBERSHIP_ROLE` |
| Organisation | `PA_LHD_SPA_PORT_ORG` | `LHD_SPA_PORT_ORGANIZATION`, `LHD_SPA_PORT_DEPARTMENT`, `LHD_SPA_PORT_MEMBERSHIP`, `LHD_SPA_PORT_INVITATION` |
| Context | `PA_LHD_SPA_PORT_CONTEXT` | `LHD_SPA_PORT_CONTEXT`, `LHD_SPA_PORT_MEMBERSHIP` |
| Wizard | `PA_LHD_SPA_PORT_WIZARD` | V1 Konfiguration/Payload; später Wizard-Tabellen |
| Application | `PA_LHD_SPA_PORT_APPLICATION` | `LHD_SPA_PORT_APPLICATION`, `LHD_SPA_PORT_APPLICATION_HISTORY` |
| Workflow | `PA_LHD_SPA_PORT_WORKFLOW` | `LHD_SPA_PORT_WORKFLOW`, `LHD_SPA_PORT_TASK` |
| Upload | `PA_LHD_SPA_PORT_UPLOAD` | `LHD_SPA_PORT_FILE`, `LHD_SPA_PORT_FILE_LINK` |
| Notification | `PA_LHD_SPA_PORT_NOTIFICATION` | `LHD_SPA_PORT_NOTIFICATION`, `LHD_SPA_PORT_MAIL_QUEUE`, `LHD_SPA_PORT_MAIL_TEMPLATE` |
| Dashboard | `PA_LHD_SPA_PORT_DASHBOARD` | Aggregation; optional Dashboard-Cache/Prefs |
| Administration | `PA_LHD_SPA_PORT_ADMIN` | administrative Nutzung mehrerer Portal-Tabellen |
| Facility | `PA_LHD_SPA_PORT_FACILITY` | RIB FM Stammdaten, `LHD_SPA_SPORTSCOMPLEXES`, `LHD_SPA_FACILITY2COMPLEX`, `LHD_SPA_FACILITYGROUPS` |
| Availability | `PA_LHD_SPA_PORT_AVAILABILITY` | `LHD_SPA_OCC`, `LHD_SPA_OCC_WINNER`, `LHD_SPA_OCC_DAY_COVERAGE` |
| Booking | `PA_LHD_SPA_PORT_BOOKING` | `LHD_SPA_EVENTS`, `LHD_SPA_EVENT2UNIT`, `LHD_SPA_RECURRING_PATTERN`, `LHD_SPA_EVENTTYPES` |
| Charge | `PA_LHD_SPA_PORT_CHARGE` | `LHD_SPA_CHARGES`, `LHD_SPA_EVENTCHARGES`, `LHD_SPA_CHARGETYPES`, `LHD_SPA_CHARGEGROUPS` |
| Invoice | `PA_LHD_SPA_PORT_INVOICE` | `LHD_SPA_INVOICES`, `LHD_SPA_INVOICE_CHARGEINFOS`, `LHD_SPA_DOCUMENTS` |
| Document | `PA_LHD_SPA_PORT_DOCUMENT` | `LHD_SPA_DOCUMENTS`, `LHD_SPA_DOCUMENTS_EVENTS`, `LHD_SPA_DOCUMENT_TYPES`, Vorlagen/Textmodule |
| ReferenceData | `PA_LHD_SPA_PORT_REF` | Sportarten, Eventtypen, Dokumenttypen, FacilityGroups |
| Audit | `PA_LHD_SPA_PORT_AUDIT` | `LHD_SPA_PORT_AUDIT`, optional `LHD_SPA_LOGGING` |
| Logging | `PA_LHD_SPA_PORT_LOG` | `LHD_SPA_LOGGING`, optional Portal-Logtabelle |

---

## 10. REST → Package-Zuordnung

| REST-Bereich | Primäres Package | Wichtige Nebenpackages |
|---|---|---|
| `/auth` | `PA_LHD_SPA_PORT_AUTH` | `USER`, `NOTIFICATION`, `AUDIT` |
| `/users` | `PA_LHD_SPA_PORT_USER` | `ORG`, `CONTEXT`, `AUDIT` |
| `/organizations` | `PA_LHD_SPA_PORT_ORG` | `USER`, `CONTEXT`, `NOTIFICATION`, `AUDIT` |
| `/contexts` | `PA_LHD_SPA_PORT_CONTEXT` | `ORG`, `USER`, `AUDIT` |
| `/wizard` | `PA_LHD_SPA_PORT_WIZARD` | `REF`, `APPLICATION`, `UPLOAD` |
| `/applications` | `PA_LHD_SPA_PORT_APPLICATION` | `WIZARD`, `WORKFLOW`, `UPLOAD`, `BOOKING`, `NOTIFICATION`, `AUDIT` |
| `/workflow` | `PA_LHD_SPA_PORT_WORKFLOW` | `APPLICATION`, `NOTIFICATION`, `AUDIT` |
| `/uploads` | `PA_LHD_SPA_PORT_UPLOAD` | `APPLICATION`, `WORKFLOW`, `DOCUMENT`, `AUDIT` |
| `/notifications` | `PA_LHD_SPA_PORT_NOTIFICATION` | `USER`, `CONTEXT`, `AUDIT` |
| `/dashboard` | `PA_LHD_SPA_PORT_DASHBOARD` | `APPLICATION`, `WORKFLOW`, `DOCUMENT`, `INVOICE`, `NOTIFICATION` |
| `/admin` | `PA_LHD_SPA_PORT_ADMIN` | mehrere Domänenpackages |
| `/facilities` | `PA_LHD_SPA_PORT_FACILITY` | `REF`, `AVAILABILITY` |
| `/availability` | `PA_LHD_SPA_PORT_AVAILABILITY` | `PA_LHD_SPA_OCC`, `FACILITY`, `BOOKING` |
| `/bookings` | `PA_LHD_SPA_PORT_BOOKING` | `PA_LHD_SPA`, `PA_LHD_SPA_OCC`, `CHARGE`, `DOCUMENT`, `AUDIT` |
| `/charges` | `PA_LHD_SPA_PORT_CHARGE` | `PA_LHD_SPA`, `BOOKING`, `INVOICE` |
| `/invoices` | `PA_LHD_SPA_PORT_INVOICE` | `CONTEXT`, `DOCUMENT`, `CHARGE`, `AUDIT` |
| `/documents` | `PA_LHD_SPA_PORT_DOCUMENT` | `CONTEXT`, `BOOKING`, `INVOICE`, `AUDIT` |
| `/reference-data` | `PA_LHD_SPA_PORT_REF` | mehrere Bestandstabellen |

---

## 11. Abhängigkeiten zwischen Packages

### 11.1 Kernabhängigkeiten

```text
AUTH
  → USER
  → ORG
  → CONTEXT
```

```text
WIZARD
  → APPLICATION
  → WORKFLOW
  → NOTIFICATION
  → AUDIT
```

```text
APPLICATION
  → BOOKING
  → CHARGE
  → DOCUMENT
```

```text
AVAILABILITY
  → PA_LHD_SPA_OCC
```

```text
BOOKING
  → PA_LHD_SPA
  → PA_LHD_SPA_OCC
```

```text
CHARGE
  → PA_LHD_SPA
```

```text
DOCUMENT / INVOICE
  → CONTEXT
  → AUDIT
```

### 11.2 Verbotene Abhängigkeiten

| Nicht erlaubt | Grund |
|---|---|
| `AUTH` → `BOOKING` | Authentifizierung darf keine Fachlogik kennen |
| `WIZARD` → `LHD_SPA_EVENTS` direkt | Wizard darf keine Buchungslogik schreiben |
| `APPLICATION` → `LHD_SPA_EVENTS` direkt ohne Booking-Package | Buchungsübergabe muss fachlich gekapselt bleiben |
| `PORTAL` → `LHD_SPA_OCC` direkt | Availability-Package kapselt Occurrence-Zugriff |
| `PORTAL` → `LHD_SPA_DOCUMENTS` direkt | Dokumentzugriff benötigt Kontext-/Rechteprüfung |
| `PORTAL` → `LHD_SPA_INVOICES` direkt | Rechnungszugriff benötigt Kontext-/Rechteprüfung |
| `NOTIFICATION` → `BOOKING` | Benachrichtigung darf keine Buchungslogik auslösen |

---

## 12. Status- und Fehlerkonzept

### 12.1 Einheitliche Rückgabe

Packages sollen ein einheitliches Ergebnisformat liefern.

Mögliche fachliche Struktur:

```text
result_code
result_message
business_error_code
correlation_id
payload / cursor
```

### 12.2 Fehlerarten

| Fehlerart | Beispiel | Behandlung |
|---|---|---|
| Validierungsfehler | Pflichtfeld fehlt | fachlicher Fehler an REST zurückgeben |
| Berechtigungsfehler | fremder Kontext | Zugriff verweigern, Audit schreiben |
| Statusfehler | Antrag bereits eingereicht | fachlicher Fehler |
| Konflikt | gewünschte Zeit belegt | Konfliktinformation liefern |
| technischer Fehler | DB-Fehler | Log schreiben, generischen Fehler liefern |

### 12.3 Fehlercodes

Es sollte ein eigener fachlicher Fehlercodebereich definiert werden.

Beispiele:

```text
AUTH_001_INVALID_LOGIN
AUTH_002_USER_LOCKED
CTX_001_CONTEXT_FORBIDDEN
APP_001_INVALID_STATE
APP_002_VALIDATION_FAILED
WF_001_TASK_NOT_ASSIGNED
BOOK_001_SLOT_NOT_AVAILABLE
DOC_001_ACCESS_DENIED
INV_001_ACCESS_DENIED
```

---

## 13. Audit- und Loggingregeln

### 13.1 Zwingend zu auditieren

| Vorgang | Audit erforderlich |
|---|---:|
| Registrierung | ja |
| Login-Fehlversuche | ja |
| Kontextwechsel | ja |
| Antrag einreichen | ja |
| Antrag zurückziehen | ja |
| Workflowentscheidung | ja |
| Rückfrage / Antwort | ja |
| Upload | ja |
| Dokumentdownload | ja |
| Rechnungsanzeige / Rechnungsdownload | ja |
| administrative Änderung | ja |
| Rollenänderung | ja |

### 13.2 Technisches Logging

Technisches Logging muss mindestens erfassen:

- Package,
- Funktion,
- Zeitpunkt,
- Benutzer / technischer Benutzer,
- Kontext,
- Correlation-ID,
- Fehlertext,
- fachlicher Objektbezug.

---

## 14. Transaktions- und Commit-Regeln

### 14.1 Grundregel

Ein Package darf nur dann selbst committen, wenn es fachlich der eindeutige Abschluss einer Operation ist.

Bei Operationen, die aus der REST-Schicht mehrere Packages orchestrieren, muss die Transaktionsgrenze bewusst festgelegt werden.

### 14.2 Kritische Vorgänge

| Vorgang | Transaktionsempfehlung |
|---|---|
| `submit_application` | Application + History + Workflow + Notification + Audit in einer fachlichen Transaktion |
| `complete_task` | Task + Workflow + History + Audit in einer fachlichen Transaktion |
| `create_booking_from_application` | Booking + Applicationstatus + Workflow + Occurrence-Fast-Path + Audit abgestimmt behandeln |
| `enqueue_mail` | fachlich gemeinsam mit auslösendem Vorgang oder nachgelagert über Outbox-Muster |
| `record_document_download` | eigenes Audit ohne Beeinflussung des Downloads möglich |

---

## 15. Sicherheitsregeln für Packages

Jede öffentlich durch REST nutzbare Package-Operation muss prüfen:

1. Ist der Benutzer authentifiziert?
2. Ist der Kontext zulässig?
3. Hat der Benutzer die notwendige Rolle?
4. Gehört das Objekt zum aktiven Kontext?
5. Ist der aktuelle Status des Objekts für diese Operation zulässig?
6. Muss ein Audit-Eintrag geschrieben werden?

Diese Prüfungen dürfen nicht ausschließlich im Portal-Frontend liegen.

---

## 16. Umsetzungsreihenfolge

Die Package-Umsetzung sollte in dieser Reihenfolge erfolgen.

| Reihenfolge | Package | Begründung |
|---:|---|---|
| 1 | `PA_LHD_SPA_PORT_AUDIT` | Basis für Nachvollziehbarkeit |
| 2 | `PA_LHD_SPA_PORT_LOG` | technische Fehleranalyse |
| 3 | `PA_LHD_SPA_PORT_AUTH` | Voraussetzung für Portalzugang |
| 4 | `PA_LHD_SPA_PORT_USER` | Benutzerprofil |
| 5 | `PA_LHD_SPA_PORT_ORG` | Organisationen / Mitgliedschaften |
| 6 | `PA_LHD_SPA_PORT_CONTEXT` | Sicherheits- und Sichtbarkeitsgrundlage |
| 7 | `PA_LHD_SPA_PORT_REF` | Auswahldaten für Portal/Wizard |
| 8 | `PA_LHD_SPA_PORT_WIZARD` | Antragsframework |
| 9 | `PA_LHD_SPA_PORT_UPLOAD` | Anlagen zu Anträgen |
| 10 | `PA_LHD_SPA_PORT_APPLICATION` | Antragserstellung und Einreichung |
| 11 | `PA_LHD_SPA_PORT_WORKFLOW` | Arbeitskorb und Bearbeitung |
| 12 | `PA_LHD_SPA_PORT_NOTIFICATION` | Rückmeldungen und Mailqueue |
| 13 | `PA_LHD_SPA_PORT_FACILITY` | Sportanlagen- und Suchdaten |
| 14 | `PA_LHD_SPA_PORT_AVAILABILITY` | freie Zeiten / Kalender |
| 15 | `PA_LHD_SPA_PORT_BOOKING` | Übergabe Antrag → Reservierung/Buchung |
| 16 | `PA_LHD_SPA_PORT_CHARGE` | Gebührenauskunft |
| 17 | `PA_LHD_SPA_PORT_DOCUMENT` | Dokumentzugriff |
| 18 | `PA_LHD_SPA_PORT_INVOICE` | Rechnungszugriff |
| 19 | `PA_LHD_SPA_PORT_DASHBOARD` | verdichtete Übersicht |
| 20 | `PA_LHD_SPA_PORT_ADMIN` | Administration / Betrieb |

---

## 17. Relevanz für Aufwandsschätzung

Die Packages sind eigene Arbeitspaketblöcke in der Kalkulation.

Je Package müssen mindestens geschätzt werden:

- fachliche Detailkonzeption,
- Tabellen / DDL,
- Package-Spezifikation,
- Package-Implementierung,
- Unit-/Integrationstests,
- REST-Anbindung,
- Fehler- und Auditlogik,
- Dokumentation.

Für komplexe Domänen sind zusätzliche Blöcke notwendig:

| Domäne | Zusatzaufwand |
|---|---|
| Wizard | Konfigurationsmodell, Validierungslogik, Mapping |
| Application | Payload-Versionierung, Historie, Einreichlogik |
| Workflow | Statusmodell, Arbeitskorb, Aufgabenlogik |
| Availability | Performance, Occurrence-/Winner-Zugriff, Suchlogik |
| Booking | Übergabe in bestehende Buchungslogik, Stornierung, Fast Path |
| Document | Zugriffsschutz, BLOB-/Downloadlogik, Audit |
| Invoice | Zugriffsschutz, SAP-/ePayment-Kontext |

---

## 18. Offene Punkte

| Punkt | Klärung |
|---|---|
| endgültiges Statusmodell je neuer Portal-Tabelle | mit Domänen Application, Workflow, Upload, Notification abstimmen |
| physische Speicherung Wizard-Konfiguration | V1 JSON/Konfiguration vs. spätere relationale Wizard-Tabellen festlegen |
| genaue Payload-Struktur `LHD_SPA_PORT_APPLICATION` | Antragstypen und Mapping definieren |
| Berechtigungsmatrix je Package-Operation | mit Rollen-/Rechtekapitel synchronisieren |
| Rückgabeformat Package → .NET | Cursor, JSON/CLOB oder Record je Operation festlegen |
| Commit-Strategie | pro fachlicher Operation verbindlich entscheiden |
| technisches Logging vs. fachliches Audit | Tabellen und Trennung finalisieren |
| ePayment-Funktionen | Umfang in V1 klären |
| Document-Erzeugung über Portal | nur Anzeige/Download oder auch Erzeugung in V1 klären |
| Dashboard-Caching | erst bei Performancebedarf entscheiden |

---

## 19. Zusammenfassung

Für die SportFM-Plattform werden neue Portal-Packages je Domäne benötigt.

Die wichtigsten Grundsätze sind:

- bestehende Packages `PA_LHD_SPA` und `PA_LHD_SPA_OCC` bleiben fachlich führend,
- alle Portal-Domänen erhalten eigene Packages,
- neue Packages kapseln Berechtigungen, Status, Workflow, Audit und Portalzugriffe,
- das Portal greift nicht direkt auf Tabellen zu,
- REST bildet fachliche Operationen ab,
- Buchungs-, Gebühren-, Dokumenten- und Rechnungslogik wird nicht im Portal dupliziert,
- Wizard, Application und Workflow werden als langfristige Plattformbausteine geplant.

Damit bildet dieses Package-Modell die Grundlage für:

- detaillierte Tabellenplanung,
- REST-Endpunkte,
- Arbeitspakete,
- Aufwandsschätzung,
- Tests,
- spätere Migration der WPF-Anwendung auf die REST-/Package-Schicht.
