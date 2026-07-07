# Deutsche Kürzel und Codierung

| Feld | Wert |
|---|---|
| Kapitel | 00 – Projektsteuerung |
| Dokument | Deutsche Kürzel und Codierung |
| Status | Verbindliche Arbeitsregel |
| Zweck | Einheitliche deutschsprachige Codierung für Projekthandbuch, Anforderungen, Arbeitspakete, Tests und Architekturentscheidungen |

---

## 1 Grundsatz

Für die interne Codierung im Projekthandbuch werden keine englischen Abkürzungen verwendet.

Verwendet werden deutsche Kürzel oder fest etablierte technische Begriffe.

REST-API, Oracle, PL/SQL, OAuth, JSON und PDF bleiben als technische Eigennamen erhalten.

---

## 2 Verbindliche Kürzel

| Bereich | Kürzel | Bedeutung |
|---|---:|---|
| Anforderung | `ANF` | fachliche, technische oder organisatorische Anforderung |
| Fachdomäne | `FD` | fachlicher Verantwortungsbereich |
| Datenobjekt | `DO` | fachliches Datenobjekt |
| Arbeitspaket | `AP` | konkret umsetzbare Arbeitseinheit |
| Testfall | `TF` | prüfbarer Testfall |
| Architekturentscheidung | `AE` | dokumentierte Architekturentscheidung |
| Meilenstein | `MS` | fachlicher oder technischer Meilenstein |
| Lieferergebnis | `LE` | abgrenzbares Projektergebnis |
| Änderungsantrag | `ÄA` | fachliche oder technische Änderungsanforderung |
| Risiko | `RI` | Projektrisiko oder technisches Risiko |
| Klärungspunkt | `KP` | offener fachlicher oder technischer Punkt |
| Entscheidungspunkt | `EP` | fachliche oder technische Entscheidung |
| Prüfkriterium | `PK` | Kriterium für Abnahme oder Prüfung |
| Datenbereich | `DBR` | zusammenhängender fachlicher Datenbereich |
| Schnittstelle | `SST` | technische oder fachliche Schnittstelle |
| Berechtigung | `BER` | Rolle, Recht oder Zugriffsvorgabe |
| Dokumentation | `DOK` | Dokumentationsbestandteil |
| Betriebspunkt | `BP` | Betriebs-, Wartungs- oder Infrastrukturthema |

---

## 3 Nummernschema

Das Nummernschema lautet:

```text
<Kürzel>-<Bereich>-<laufende Nummer>
```

Beispiele:

```text
ANF-BUCH-001
AP-BUCH-001
TF-BUCH-001
AE-ARCH-001
KP-DAT-001
LE-PORT-001
ÄA-001
```

---

## 4 Bereichskürzel

| Bereich | Kürzel |
|---|---:|
| Architektur | `ARCH` |
| Datenmodell | `DAT` |
| Antrag | `ANT` |
| Wizard / Antragsassistent | `ASS` |
| Arbeitsablauf / Workflow | `ABL` |
| Buchung | `BUCH` |
| Belegung / Verfügbarkeit | `BEL` |
| Sportstätte / Anlage | `ANL` |
| Nutzer / Organisation | `NUT` |
| Authentifizierung | `AUTH` |
| Berechtigung | `BER` |
| Dokumente | `DOK` |
| Gebühren | `GEB` |
| Rechnungen | `RECH` |
| Zahlung | `ZAHL` |
| Meldungen / Benachrichtigungen | `MELD` |
| Auswertung | `AUSW` |
| Administration | `ADM` |
| Migration | `MIG` |
| Betrieb | `BETR` |
| Test | `TEST` |

---

## 5 Ersetzung bisheriger englischer Kürzel

| Bisher | Neu |
|---|---:|
| `PF-*` | `ANF-*` |
| `WP-*` | `AP-*` |
| `TC-*` | `TF-*` |
| `ADR-*` | `AE-*` |
| `BD-*` | `FD-*` |
| `DL-*` | `LE-*` |
| `CR-*` | `ÄA-*` |
| `WBS` | Projektstrukturplan / `PSP` |
| `Business Object` | Datenobjekt / Fachobjekt |
| `Business Domain` | Fachdomäne |
| `Deliverable` | Lieferergebnis |

---

## 6 Anwendung

Diese Codierung ist ab sofort für neue und überarbeitete Dokumente verbindlich.

Bereits vorhandene Dokumente werden bei der weiteren Bearbeitung schrittweise auf diese Kürzel umgestellt.
