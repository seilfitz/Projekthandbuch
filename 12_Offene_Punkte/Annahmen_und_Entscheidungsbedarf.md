# Annahmen und Entscheidungsbedarf

## Zweck

Dieses Dokument trennt belastbare Planungsannahmen von noch ausstehenden Entscheidungen. Dadurch bleiben Aufwandsschätzung und Projektplanung nachvollziehbar, ohne ungeklärte Sachverhalte als bereits beschlossen darzustellen.

## Planungsannahmen

| ID | Annahme | Auswirkung auf Planung und Umsetzung |
|---|---|---|
| AN-001 | SportFM bleibt das führende Fachsystem; Oracle 19c und die vorhandene PL/SQL-Logik werden weiterverwendet. | Keine Neuimplementierung von Buchungs-, Belegungs-, Gebühren- oder Rechnungslogik in Portal oder REST-API |
| AN-002 | Die REST-API wird als fachliche Zugriffsschicht und modularer Monolith umgesetzt. | Einheitliche Plattformbasis; keine Microservice-Infrastruktur in V1 |
| AN-003 | Das SportPortal ist ein neuer Blazor-Client und greift ausschließlich über die REST-API auf geschützte Fachdaten zu. | Keine direkten Datenbankzugriffe und keine verbindlichen Berechtigungsentscheidungen im Client |
| AN-004 | Bestehende Buchungen, Occurrences, Winner, Dokumente, Gebühren und Rechnungen werden nicht in neue Portal-Tabellen migriert. | Referenzierung des Bestands; Migration beschränkt sich auf neue Portalobjekte, Konfigurationen und Zuordnungen |
| AN-005 | V1 umfasst Antrag, Wizard, Workflow, Arbeitskorb, Entscheidungen und die kontrollierte Übergabe an SportFM. | Umsetzung entlang des in den Arbeitspaketen beschriebenen kritischen Pfads |
| AN-006 | Die WPF-Anwendung wird schrittweise und nicht als Big Bang abgelöst. | Nur ausgewählte, vorzugsweise lesende Funktionen werden in V1 auf REST umgestellt |
| AN-007 | Sicherheits-, Audit- und Berechtigungsprüfungen erfolgen serverseitig. | Authentication, Context/Authorization und Audit/Logging sind Voraussetzungen der fachlichen Funktionen |

Werden diese Annahmen geändert, sind Architektur, Datenmodell, Arbeitspakete, Aufwand und Terminplan erneut zu bewerten.

## Entscheidungsbedarf

| ID | Entscheidung | Empfohlene Festlegung für V1 | Entscheidung spätestens vor |
|---|---|---|---|
| EB-001 | Verbindlicher fachlicher V1-Umfang | Beschränkung auf die in der Arbeitspaketübersicht ausgewiesenen Kernfunktionen; weitere Komfort- und Integrationsfunktionen als Folgeausbau | Abschluss AP-001 |
| EB-002 | Authentifizierungsverfahren | E-Mail/Passwort mit TOTP-2FA für Portalnutzer; OAuth Client Credentials für technische Clients; BundID außerhalb V1 | Beginn AP-004 |
| EB-003 | Konten- und Organisationsfreigabe | Selbstregistrierung mit E-Mail-Bestätigung und anschließender administrativer Zuordnung/Freigabe | Beginn AP-006 |
| EB-004 | API-Versionierung und Vertragsfreigabe | Versionspräfix `/api/v1`; OpenAPI als verbindlicher Vertrag; inkompatible Änderungen nur über neue Hauptversion | Abschluss AP-003 |
| EB-005 | Speicherung von Uploads | Ablageort, Virenprüfung, zulässige Dateitypen, Größenlimits sowie Lösch- und Aufbewahrungsfristen verbindlich festlegen | Beginn AP-025 |
| EB-006 | Übergabe genehmigter Anträge | Idempotente, protokollierte Übergabe mit eindeutigem Mapping und fachlich definiertem Fehlerstatus | Beginn AP-030 |
| EB-007 | WPF-Migrationsumfang | In V1 nur die bereits über stabile REST-Endpunkte verfügbaren lesenden Funktionen migrieren | Beginn AP-019 |
| EB-008 | Messbare Qualitätsziele | Antwortzeiten, Verfügbarkeit, Datenschutz, Logging, Wiederanlauf und Abnahmekriterien je Endpunktgruppe festlegen | Testplanung AP-020 |
| EB-009 | Betriebsmodell | Verantwortlichkeiten für Deployment, Zertifikate, Secrets, Monitoring, Datensicherung und Störungsbearbeitung festlegen | Beginn AP-021 |

## Dokumentation der Entscheidungen

Beschlossene Festlegungen werden mit Datum, Verantwortlichem und Begründung in `11_Entscheidungen` übernommen. Der zugehörige offene Punkt wird anschließend in `Offene_Punkte.md` geschlossen oder aktualisiert.
