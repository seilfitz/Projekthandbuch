# Offene Punkte

## Zweck

Die folgende Übersicht bündelt die für die Umsetzung von SportFM V1 noch zu klärenden Punkte. Sie dient als Arbeitsgrundlage für Projektleitung, Fachbereich und technische Umsetzung. Architekturgrundsätze, die bereits im Projekthandbuch festgelegt sind, werden nicht erneut als offene Punkte geführt.

## Klärungsliste

| ID | Offener Punkt | Erforderliches Ergebnis | Bezug | Priorität |
|---|---|---|---|:---:|
| OP-001 | V1-Endpunktumfang | Verbindliche Freigabe der Endpunkte und Operationen je Domäne einschließlich Abgrenzung zur Folgeversion | AP-003, AP-007, AP-010 bis AP-015, AP-024 | hoch |
| OP-002 | API-Verträge | Finale DTOs, Validierungsregeln, Fehlercodes, Filter, Sortierung und Seitennavigation für die freigegebenen Endpunkte | AP-003 und fachliche REST-Arbeitspakete | hoch |
| OP-003 | Authentifizierung und Kontofreigabe | Festlegung von Token-Laufzeiten, 2FA-Pflicht, Passwortregeln sowie Registrierung, Prüfung und Freischaltung von Portalnutzerkonten | AP-004, AP-005, AP-017 | hoch |
| OP-004 | Organisations- und Berechtigungskontext | Verbindliche Regeln für die Zuordnung von Portalnutzer, Organisation, SportFM-Nutzer und Rolle sowie für Vertretungen und Kontextwechsel | AP-005, AP-006, AP-030 | hoch |
| OP-005 | Antrag und SportFM-Übergabe | Festlegung des fachlichen Mappings eines genehmigten Antrags auf vorhandene SportFM-Objekte einschließlich Fehler- und Wiederholungsbehandlung | AP-007, AP-009, AP-028, AP-030, AP-038 | hoch |
| OP-006 | Initialbefüllung und Stammdatenabgleich | Entscheidung, welche Rollen, Rechte, Wizard-Konfigurationen und Referenzdaten initial angelegt werden und ob bekannte Ansprechpartner vorbefüllt werden | AP-006, AP-008, AP-017, AP-024 | mittel |
| OP-007 | Nichtfunktionale Abnahmekriterien | Messbare Zielwerte für Antwortzeiten, Verfügbarkeit, Protokollierung, Datenschutz, Aufbewahrung und maximale Uploadgrößen | AP-003, AP-018, AP-020, AP-021, AP-025 | hoch |
| OP-008 | Umfang der WPF-Migration | Auswahl der lesenden Funktionen, die in V1 bereits über die REST-API statt über direkten Oracle-Zugriff ausgeführt werden | AP-019 | mittel |
| OP-009 | Betriebs- und Integrationsumgebung | Festlegung von Zielumgebungen, Deploymentweg, Zertifikaten, Secret-Verwaltung, Monitoring und Zuständigkeiten im Betrieb | AP-002, AP-021 | hoch |

## Bearbeitung

Offene Punkte werden vor Beginn des jeweils abhängigen Arbeitspakets geklärt. Entscheidungen mit Architekturwirkung werden zusätzlich im Kapitel `11_Entscheidungen` dokumentiert. Änderungen am freigegebenen V1-Umfang sind über das Projektcontrolling nachzuführen.
