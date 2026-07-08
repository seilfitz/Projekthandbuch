# Technische Zerlegung – System und Plattform

| Dokument | 03_Domaenen/Technische_Zerlegung/12_System.md |
|----------|----------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Domäne System/Plattform umfasst querschnittliche technische Features, die für alle anderen Domänen benötigt werden.

---

# 2 Querschnittsfeatures

## FUN-SYS-001 REST-Grundgerüst

| Baustein | Zerlegung |
|----------|-----------|
| API | Projektstruktur, Controller-Konventionen |
| BL | Service-Struktur |
| DB | Datenzugriffskonzept Oracle |
| VAL | zentrale Validierung |
| BER | Authentifizierungsintegration |
| LOG | Request-/Error-Logging |
| TEST | Smoke-Tests |
| DOK | REST-Konventionen |

**Abhängigkeiten:** Architekturentscheidung REST  
**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-SYS-002 Fehlerbehandlung

| Baustein | Zerlegung |
|----------|-----------|
| API | einheitliche Fehlerantworten |
| BL | Fachfehler, technische Fehler |
| LOG | Fehlerprotokollierung |
| TEST | Fehlerfälle |
| DOK | Fehlercodes |

**Risiko:** Mittel  
**Komplexität:** M

---

## FUN-SYS-003 Logging und Audit

| Baustein | Zerlegung |
|----------|-----------|
| API | Request-Logging |
| BL | Fachereignisse |
| DB | Audit-Tabellen / bestehendes Logging |
| BER | Zugriff nur Admin |
| LOG | zentrale Komponente |
| TEST | Audit-Einträge |
| DOK | Auditkonzept |

**Risiko:** Hoch  
**Komplexität:** L

---

## FUN-SYS-004 Konfiguration

| Baustein | Zerlegung |
|----------|-----------|
| FE | Administrationsoberfläche |
| API | GET/PUT `/api/konfiguration` |
| BL | technische und fachliche Einstellungen |
| DB | Konfigurationstabellen |
| VAL | Wertebereiche |
| BER | Administrator |
| LOG | Änderungen protokollieren |
| TEST | Konfiguration ändern |
| DOK | Konfigurationshandbuch |

**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-SYS-005 Benachrichtigungen

| Baustein | Zerlegung |
|----------|-----------|
| FE | Benachrichtigungsübersicht |
| API | POST `/api/benachrichtigungen` |
| BL | Mail, Portalnachricht, Vorlagen |
| DB | Nachricht, Vorlage, Versandstatus |
| VAL | Empfänger, Vorlage |
| BER | System/Sachbearbeitung |
| LOG | Versand protokollieren |
| SCH | SMTP / später weitere Kanäle |
| TEST | Mailversand, Fehler |
| DOK | Benachrichtigungskonzept |

**Risiko:** Mittel  
**Komplexität:** L

---

## FUN-SYS-006 Workflow-Grundsystem

| Baustein | Zerlegung |
|----------|-----------|
| FE | Workflow-Konfiguration / Statusanzeige |
| API | `/api/workflows` |
| BL | Status, Übergänge, Rollen, Historie |
| DB | Workflowdefinition, Instanz, Historie |
| VAL | erlaubte Übergänge |
| BER | rollenabhängige Aktionen |
| LOG | Statuswechsel protokollieren |
| TEST | Workflowübergänge |
| DOK | Workflowmodell |

**Risiko:** Hoch  
**Komplexität:** XXL

---

## FUN-SYS-007 Sicherheit

| Baustein | Zerlegung |
|----------|-----------|
| API | JWT/OAuth/2FA-Integration |
| BL | Rechteprüfung je Domäne |
| DB | Rollen, Rechte, Tokens |
| VAL | Token, Login, 2FA |
| BER | zentrale Autorisierung |
| LOG | Sicherheitsereignisse |
| TEST | Authentifizierung/Autorisierung |
| DOK | Sicherheitskonzept |

**Risiko:** Hoch  
**Komplexität:** XL

---

## FUN-SYS-008 Deployment und Betrieb

| Baustein | Zerlegung |
|----------|-----------|
| BL | Umgebungen, Konfiguration |
| DB | Migrationsskripte |
| LOG | Monitoring, Fehlerprotokolle |
| SCH | Infrastruktur |
| TEST | Deployment-Test |
| DOK | Betriebshandbuch |

**Risiko:** Hoch  
**Komplexität:** L
