# Arbeitspakete – System und Plattform

| Dokument | 06_Arbeitspakete/SYS_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **System und Plattform**.

---

# 2 Arbeitspakete

## REST-Grundgerüst

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-001 | Projektstruktur REST-API erstellen | N | siehe Feature-Abhängigkeiten | Projektstruktur REST-API |
| AP-SYS-002 | Controller-Konventionen definieren | N | siehe Feature-Abhängigkeiten | Controller-Konventionen definieren |
| AP-SYS-003 | Service-Struktur aufbauen | N | siehe Feature-Abhängigkeiten | Service-Struktur aufbauen |
| AP-SYS-004 | Oracle-Datenzugriffskonzept implementieren | N | siehe Feature-Abhängigkeiten | Oracle-Datenzugriffskonzept |
| AP-SYS-005 | zentrale Validierung einrichten | N | siehe Feature-Abhängigkeiten | zentrale Validierung einrichten |
| AP-SYS-006 | Authentifizierung integrieren | N | siehe Feature-Abhängigkeiten | Authentifizierung integrieren |
| AP-SYS-007 | Request-/Error-Logging einrichten | N | siehe Feature-Abhängigkeiten | Request-/Error-Logging einrichten |
| AP-SYS-008 | Smoke-Tests erstellen | T | siehe Feature-Abhängigkeiten | Smoke-Tests |
| AP-SYS-009 | REST-Konventionen dokumentieren | D | siehe Feature-Abhängigkeiten | REST-Konventionen dokumentieren |

## Fehlerbehandlung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-010 | Einheitliche Fehlerantworten definieren | N | siehe Feature-Abhängigkeiten | Einheitliche Fehlerantworten definieren |
| AP-SYS-011 | Middleware/Filter Fehlerbehandlung implementieren | N | siehe Feature-Abhängigkeiten | Middleware/Filter Fehlerbehandlung |
| AP-SYS-012 | Fachfehler und technische Fehler trennen | N | siehe Feature-Abhängigkeiten | Fachfehler und technische Fehler trennen |
| AP-SYS-013 | Fehlercodes definieren | N | siehe Feature-Abhängigkeiten | Fehlercodes definieren |
| AP-SYS-014 | Fehlerprotokollierung anbinden | I | siehe Feature-Abhängigkeiten | Fehlerprotokollierung anbinden |
| AP-SYS-015 | Tests Fehlerfälle erstellen | T | siehe Feature-Abhängigkeiten | Tests Fehlerfälle |
| AP-SYS-016 | Dokumentation Fehlercodes ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Fehlercodes ergänzen |

## Logging und Audit

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-017 | Logging-Konzept finalisieren | N | siehe Feature-Abhängigkeiten | Logging-Konzept finalisieren |
| AP-SYS-018 | Request-Logging implementieren | N | siehe Feature-Abhängigkeiten | Request-Logging |
| AP-SYS-019 | Fachereignisse definieren | N | siehe Feature-Abhängigkeiten | Fachereignisse definieren |
| AP-SYS-020 | Audit-Tabellen/bestehendes Logging anbinden | I | siehe Feature-Abhängigkeiten | Audit-Tabellen/bestehendes Logging anbinden |
| AP-SYS-021 | Admin-Zugriff auf Audit absichern | N | siehe Feature-Abhängigkeiten | Admin-Zugriff auf Audit absichern |
| AP-SYS-022 | Audit-Einträge testen | N | siehe Feature-Abhängigkeiten | Audit-Einträge testen |
| AP-SYS-023 | Auditkonzept dokumentieren | D | siehe Feature-Abhängigkeiten | Auditkonzept dokumentieren |

## Konfiguration

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-024 | Oberfläche Systemkonfiguration erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Systemkonfiguration |
| AP-SYS-025 | REST-Endpunkte Konfiguration implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Konfiguration |
| AP-SYS-026 | Businesslogik technische/fachliche Einstellungen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik technische/fachliche Einstellungen |
| AP-SYS-027 | Konfigurationstabellen erstellen/anbinden | I | siehe Feature-Abhängigkeiten | Konfigurationstabellen/anbinden |
| AP-SYS-028 | Wertebereiche validieren | N | siehe Feature-Abhängigkeiten | Wertebereiche validieren |
| AP-SYS-029 | Administratorberechtigung umsetzen | N | siehe Feature-Abhängigkeiten | Administratorberechtigung |
| AP-SYS-030 | Konfigurationsänderungen protokollieren | N | siehe Feature-Abhängigkeiten | Konfigurationsänderungen protokollieren |
| AP-SYS-031 | Tests Konfiguration erstellen | T | siehe Feature-Abhängigkeiten | Tests Konfiguration |
| AP-SYS-032 | Konfigurationshandbuch ergänzen | N | siehe Feature-Abhängigkeiten | Konfigurationshandbuch ergänzen |

## Benachrichtigungen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-033 | Benachrichtigungsübersicht erstellen | N | siehe Feature-Abhängigkeiten | Benachrichtigungsübersicht |
| AP-SYS-034 | REST-Endpunkte Benachrichtigungen implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Benachrichtigungen |
| AP-SYS-035 | Businesslogik Mail/Portalnachricht/Vorlagen umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Mail/Portalnachricht/Vorlagen |
| AP-SYS-036 | Datenmodell Nachricht/Vorlage/Versandstatus erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Nachricht/Vorlage/Versandstatus |
| AP-SYS-037 | Empfänger/Vorlage validieren | N | siehe Feature-Abhängigkeiten | Empfänger/Vorlage validieren |
| AP-SYS-038 | SMTP anbinden | I | siehe Feature-Abhängigkeiten | SMTP anbinden |
| AP-SYS-039 | Versand protokollieren | N | siehe Feature-Abhängigkeiten | Versand protokollieren |
| AP-SYS-040 | Tests Mailversand/Fehler erstellen | T | siehe Feature-Abhängigkeiten | Tests Mailversand/Fehler |
| AP-SYS-041 | Benachrichtigungskonzept dokumentieren | D | siehe Feature-Abhängigkeiten | Benachrichtigungskonzept dokumentieren |

## Workflow-Grundsystem

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-042 | Oberfläche Workflow-Konfiguration/Statusanzeige erstellen | N | siehe Feature-Abhängigkeiten | Oberfläche Workflow-Konfiguration/Statusanzeige |
| AP-SYS-043 | REST-Endpunkte Workflows implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Workflows |
| AP-SYS-044 | Businesslogik Status/Übergänge/Rollen/Historie umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Status/Übergänge/Rollen/Historie |
| AP-SYS-045 | Datenmodell Workflowdefinition/Instanz/Historie erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Workflowdefinition/Instanz/Historie |
| AP-SYS-046 | erlaubte Übergänge validieren | N | siehe Feature-Abhängigkeiten | erlaubte Übergänge validieren |
| AP-SYS-047 | rollenabhängige Aktionen umsetzen | N | siehe Feature-Abhängigkeiten | rollenabhängige Aktionen |
| AP-SYS-048 | Statuswechsel protokollieren | N | siehe Feature-Abhängigkeiten | Statuswechsel protokollieren |
| AP-SYS-049 | Tests Workflowübergänge erstellen | T | siehe Feature-Abhängigkeiten | Tests Workflowübergänge |
| AP-SYS-050 | Workflowmodell dokumentieren | D | siehe Feature-Abhängigkeiten | Workflowmodell dokumentieren |

## Sicherheit

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-051 | JWT/OAuth/2FA-Konzept umsetzen | N | siehe Feature-Abhängigkeiten | JWT/OAuth/2FA-Konzept |
| AP-SYS-052 | zentrale Rechteprüfung je Domäne implementieren | N | siehe Feature-Abhängigkeiten | zentrale Rechteprüfung je Domäne |
| AP-SYS-053 | Datenmodell Rollen/Rechte/Tokens anbinden | I | siehe Feature-Abhängigkeiten | Datenmodell Rollen/Rechte/Tokens anbinden |
| AP-SYS-054 | Token/Login/2FA validieren | N | siehe Feature-Abhängigkeiten | Token/Login/2FA validieren |
| AP-SYS-055 | zentrale Autorisierung implementieren | N | siehe Feature-Abhängigkeiten | zentrale Autorisierung |
| AP-SYS-056 | Sicherheitsereignisse protokollieren | N | siehe Feature-Abhängigkeiten | Sicherheitsereignisse protokollieren |
| AP-SYS-057 | Tests Authentifizierung/Autorisierung erstellen | T | siehe Feature-Abhängigkeiten | Tests Authentifizierung/Autorisierung |
| AP-SYS-058 | Sicherheitskonzept dokumentieren | D | siehe Feature-Abhängigkeiten | Sicherheitskonzept dokumentieren |

## Deployment und Betrieb

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-SYS-059 | Umgebungskonzept erstellen | N | siehe Feature-Abhängigkeiten | Umgebungskonzept |
| AP-SYS-060 | Konfigurationsverwaltung für Umgebungen umsetzen | N | siehe Feature-Abhängigkeiten | Konfigurationsverwaltung für Umgebungen |
| AP-SYS-061 | Datenbank-Migrationsskripte vorbereiten | N | siehe Feature-Abhängigkeiten | Datenbank-Migrationsskripte vorbereiten |
| AP-SYS-062 | Monitoring/Fehlerprotokolle einrichten | N | siehe Feature-Abhängigkeiten | Monitoring/Fehlerprotokolle einrichten |
| AP-SYS-063 | Deployment-Test durchführen | T | siehe Feature-Abhängigkeiten | Deployment-Test durchführen |
| AP-SYS-064 | Betriebshandbuch erstellen | N | siehe Feature-Abhängigkeiten | Betriebshandbuch |
