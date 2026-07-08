
# Feature-Traceability

| Dokument | 03_Domaenen/Feature_Traceability.md |
|-----------|-------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Die Feature-Traceability stellt die vollständige Nachverfolgbarkeit aller Projektartefakte sicher. Jede fachliche Anforderung wird eindeutig einer Domäne, einem Feature, den REST-Endpunkten, dem Datenmodell, den Arbeitspaketen und den Testfällen zugeordnet.

## Ziele

- Vollständige Rückverfolgbarkeit
- Konsistente Implementierung
- Grundlage für Aufwandsschätzung
- Unterstützung der Testplanung
- Änderungsmanagement
- Nachweis der Lastenheftabdeckung

---

# 2 Traceability-Modell

```text
Lastenheft
    │
    ▼
Domäne
    │
    ▼
Feature
    │
 ┌──┴──────────────┐
 ▼                 ▼
REST API      Datenmodell
 └──────┬──────────┘
        ▼
Arbeitspaket
        ▼
Testfall
```

---

# 3 Artefaktkennungen

| Präfix | Bedeutung |
|---------|-----------|
| LF | Lastenheft-Anforderung |
| DOM | Domäne |
| FUN | Feature |
| API | REST-Endpunkt |
| TAB | Datenbanktabelle |
| AP | Arbeitspaket |
| TF | Testfall |

---

# 4 Traceability-Matrix

| LF | Domäne | Feature | API | Tabelle | AP | TF |
|----|---------|---------|-----|----------|----|----|
| LF-5.1.1 | DOM-POR | FUN-POR-001 Registrierung | POST /auth/register | user, portal_user | AP-POR-001 | TF-POR-001 |
| LF-5.1.2 | DOM-POR | FUN-POR-002 Anmeldung | POST /auth/login | user | AP-POR-002 | TF-POR-002 |
| LF-5.1.3 | DOM-POR | FUN-POR-003 Freie Zeiten | GET /availability | reservation, facility | AP-POR-003 | TF-POR-003 |
| LF-5.1.4 | DOM-VER | FUN-VER-001 Vereinsdaten | GET /clubs | club | AP-VER-001 | TF-VER-001 |
| LF-5.1.5 | DOM-ANL | FUN-ANL-007 Sportanlagen | GET /facilities | facility | AP-ANL-007 | TF-ANL-007 |
| LF-5.2 | DOM-ANT | FUN-ANT-001 Antrag erstellen | POST /applications | application | AP-ANT-001 | TF-ANT-001 |
| LF-6.2.2 | DOM-ANT | FUN-ANT-005 Statuswechsel | PATCH /applications/{id}/status | application | AP-ANT-005 | TF-ANT-005 |
| LF-6.2.3 | DOM-RES | FUN-RES-001 Reservierung | POST /reservations | reservation | AP-RES-001 | TF-RES-001 |
| LF-6.2.3 | DOM-RES | FUN-RES-005 Konfliktprüfung | POST /reservations/check | reservation | AP-RES-005 | TF-RES-005 |
| LF-6.2.4 | DOM-BUC | FUN-BUC-001 Buchung | POST /bookings | booking | AP-BUC-001 | TF-BUC-001 |
| LF-6.2.4 | DOM-BUC | FUN-BUC-004 Gebühren | POST /fees/calculate | fee | AP-BUC-004 | TF-BUC-004 |
| LF-6.2.5 | DOM-DOK | FUN-DOK-001 Bescheid | POST /documents | document | AP-DOK-001 | TF-DOK-001 |
| LF-6.2.6 | DOM-AUS | FUN-AUS-001 Belegungsplan | GET /reports/occupancy | booking | AP-AUS-001 | TF-AUS-001 |
| LF-12.1 | DOM-SST | FUN-SST-002 Themenstadtplan | GET /map | facility | AP-SST-002 | TF-SST-002 |
| LF-12.2 | DOM-SST | FUN-SST-004 ePayBL | POST /payment | payment | AP-SST-004 | TF-SST-004 |

---

# 5 Vollständigkeitsregeln

Jedes Feature muss:

- genau einer Domäne zugeordnet sein.
- mindestens einer Lastenheftanforderung entsprechen.
- mindestens einen REST-Endpunkt besitzen (soweit technisch erforderlich).
- mindestens eine Datenbanktabelle verwenden.
- mindestens ein Arbeitspaket besitzen.
- mindestens einen Testfall besitzen.

---

# 6 Änderungsmanagement

Bei Änderungen gilt folgende Reihenfolge:

1. Lastenheft aktualisieren
2. Domäne prüfen
3. Feature anpassen
4. REST-API anpassen
5. Datenmodell anpassen
6. Arbeitspaket aktualisieren
7. Testfälle aktualisieren

---

# 7 Verwendung

Die Traceability-Matrix ist verbindliche Grundlage für:

- Vollständigkeitsprüfung
- Aufwandsschätzung
- Sprintplanung
- Releaseplanung
- Testmanagement
- Projektcontrolling
- Change-Management

Vor Abschluss jedes Meilensteins ist sicherzustellen, dass jede Lastenheftanforderung mindestens einem implementierten Feature, einem abgeschlossenen Arbeitspaket und einem erfolgreichen Testfall zugeordnet werden kann.
