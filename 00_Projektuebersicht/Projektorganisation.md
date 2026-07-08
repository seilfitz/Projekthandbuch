# Projektorganisation

| Feld | Wert |
|---|---|
| Kapitel | 00 – Projektübersicht |
| Dokument | Projektorganisation |
| Status | Entwurf |
| Version | 1.0 |
| Grundlage | Lastenheft SportFM, bereitgestellte Projektunterlagen, Projektgrundsätze |

---

## 1 Zweck des Dokuments

Dieses Dokument beschreibt die organisatorische Struktur für das Projekt **Weiterentwicklung SportFM**.

Es legt fest:

- welche Organisationseinheiten beteiligt sind,
- welche Gremien und Rollen benötigt werden,
- wie Entscheidungen getroffen werden,
- wie Eskalationen erfolgen,
- wie die Zusammenarbeit zwischen Fachbereich, IT und weiteren Beteiligten organisiert wird.

Die detaillierte Beschreibung einzelner Rollen erfolgt im Dokument `Rollen_und_Verantwortlichkeiten.md`.

---

## 2 Grundsatz der Projektorganisation

Das Projekt wird als gemeinsames Vorhaben von **Eigenbetrieb IT-Dienstleistungen** und **Eigenbetrieb Sportstätten** geführt.

SportFM bleibt das führende Fachverfahren für die Vergabe, Planung, Buchung, Dokumentation und Abrechnung kommunaler Sportanlagen.

Das Projekt erweitert SportFM zu einer serviceorientierten Plattform mit REST-API und Onlineportal.

Daraus ergeben sich zwei organisatorische Schwerpunkte:

| Schwerpunkt | Verantwortung |
|---|---|
| Fachliche Anforderungen und Abnahme | Eigenbetrieb Sportstätten |
| Technische Umsetzung und Projektsteuerung | Eigenbetrieb IT-Dienstleistungen |

---

## 3 Organisationsmodell

Die Projektorganisation folgt einem mehrstufigen Modell.

```text
Projektauftraggeber
        │
        ▼
Projektlenkung
        │
        ▼
Projektleitung
        │
        ├── Projektsteuerung
        ├── Fachkoordination
        ├── Architektur / Entwicklung
        ├── Test und Abnahme
        └── Betrieb / Einführung
```

Dieses Modell stellt sicher, dass fachliche Entscheidungen, technische Entscheidungen und steuernde Entscheidungen klar getrennt und trotzdem eng abgestimmt werden.

---

## 4 Projektauftraggeber

Der Projektauftraggeber verantwortet das Projekt auf oberster Ebene.

Aufgaben:

- Bestätigung des Projektziels,
- Freigabe wesentlicher Projektphasen,
- Entscheidung bei Zielkonflikten,
- Entscheidung über wesentliche Änderungen am Projektumfang,
- Sicherstellung der organisatorischen Rahmenbedingungen.

---

## 5 Projektlenkung

Die Projektlenkung übernimmt die übergreifende Steuerung des Projektes.

Aufgaben:

- Bewertung des Projektfortschritts,
- Entscheidung über Meilensteine,
- Priorisierung bei Ressourcenkonflikten,
- Entscheidung über Änderungsanträge,
- Eskalationsinstanz für fachliche und organisatorische Konflikte.

Regeltermin:

- monatlich oder anlassbezogen.

---

## 6 Projektleitung

Die Projektleitung koordiniert das Gesamtprojekt operativ.

Aufgaben:

- Projektplanung,
- Terminsteuerung,
- Ressourcensteuerung,
- Risikomanagement,
- Änderungsmanagement,
- Berichtswesen,
- Qualitätssicherung,
- Koordination der Projektbeteiligten,
- Vorbereitung von Entscheidungen für die Projektlenkung.

Die Projektleitung stellt sicher, dass Fachlichkeit, Technik, Termine und Kosten gemeinsam betrachtet werden.

---

## 7 Projektteam

Das Projektteam setzt sich aus fachlichen, technischen und organisatorischen Beteiligten zusammen.

Kernbereiche:

| Bereich | Aufgabe |
|---|---|
| Fachbereich Sportstätten | Anforderungen, Prozesse, fachliche Prüfung |
| EB IT | Architektur, Entwicklung, Integration, Betrieb |
| Fachämter | spezifische Fachanforderungen und Abstimmungen |
| Betrieb / Infrastruktur | Bereitstellung und Betrieb der technischen Umgebung |
| Test / Abnahme | Prüfung der Umsetzung gegen Anforderungen |

---

## 8 Beteiligte Organisationseinheiten

Im Projekt werden abhängig vom jeweiligen Themengebiet weitere Organisationseinheiten beteiligt.

| Organisationseinheit | Einbindung |
|---|---|
| Eigenbetrieb Sportstätten | Fachliche Gesamtverantwortung |
| Eigenbetrieb IT-Dienstleistungen | Projektleitung, Architektur, Umsetzung |
| Amt für Schulen | Schulsportanlagen, Schulbetrieb, GTA |
| Amt für Geodaten und Kataster | Themenstadtplan, Kartenintegration |
| Amt für Presse-, Öffentlichkeitsarbeit und Protokoll | Einbindung in dresden.de, redaktionelle Anforderungen |
| Steuer- und Stadtkassenamt | Zahlung, ePayBL, Zahlungsprozesse |
| Stabsstelle Digitalisierung | Onlinezugang, Antragsassistenten |
| weitere Fachbereiche | themenbezogene Zuarbeit |

Die konkrete Beteiligung erfolgt je Arbeitspaket und wird in Projektterminen festgelegt.

---

## 9 Teilprojekte

Für die Steuerung wird das Gesamtprojekt in fachlich sinnvolle Teilprojekte gegliedert.

| Teilprojekt | Inhalt |
|---|---|
| TP-01 Projektsteuerung | Planung, Controlling, Risiken, Entscheidungen |
| TP-02 Plattform | REST-API, Authentifizierung, zentrale Dienste |
| TP-03 Onlineportal | Portaloberfläche, Nutzerkonto, Dashboard |
| TP-04 Antragssystem | Wizard, Anträge, Arbeitsabläufe, Uploads |
| TP-05 SportFM-Erweiterung | Module Antrag, Planung, Buchung, Dokumente |
| TP-06 Schnittstellen | SAP, Themenstadtplan, ePayBL, weitere Systeme |
| TP-07 Test und Abnahme | Testfälle, Testbetrieb, fachliche Abnahme |
| TP-08 Betrieb | Infrastruktur, Monitoring, Wartung, Einführung |

---

## 10 Entscheidungswege

Entscheidungen werden nach Tragweite unterschieden.

| Entscheidung | Zuständig |
|---|---|
| fachliche Detailentscheidung | Fachkoordination / Fachbereich |
| technische Detailentscheidung | Architektur / Entwicklung |
| Architekturentscheidung | Projektleitung mit Architektur |
| Priorisierung von Anforderungen | Projektleitung mit Fachbereich |
| Änderung des Projektumfangs | Projektlenkung |
| Budgetrelevante Änderung | Projektauftraggeber / Projektlenkung |
| Meilensteinfreigabe | Projektlenkung |

Architekturentscheidungen werden als `AE-*` dokumentiert.

Änderungen am Umfang werden als `ÄA-*` dokumentiert.

---

## 11 Eskalationsweg

```text
Projektmitarbeiter
        │
        ▼
Teilprojektverantwortlicher
        │
        ▼
Projektleitung
        │
        ▼
Projektlenkung
        │
        ▼
Projektauftraggeber
```

Eskalationen erfolgen, wenn

- Termine gefährdet sind,
- Anforderungen widersprüchlich sind,
- Ressourcen nicht ausreichen,
- Entscheidungen nicht auf Arbeitsebene getroffen werden können,
- Risiken den Projektauftrag gefährden.

---

## 12 Kommunikationsstruktur

| Termin | Zweck | Rhythmus |
|---|---|---|
| Projektteamtermin | operative Abstimmung | wöchentlich |
| Fachworkshop | Klärung fachlicher Anforderungen | nach Bedarf |
| Architekturtermin | technische Entscheidungen | nach Bedarf |
| Test- und Abnahmetermin | Prüfung der Ergebnisse | je Meilenstein |
| Lenkungstermin | Steuerung und Entscheidungen | monatlich / anlassbezogen |

---

## 13 Dokumentationsgrundsatz

Alle Projektdokumente werden im Projekthandbuch gepflegt.

Grundsätze:

- zentrale Ablage im GitHub-Repository,
- nachvollziehbare Versionierung,
- deutschsprachige Kürzel gemäß `00_Projektsteuerung/Deutsche_Kuerzel.md`,
- keine parallelen Schattenlisten,
- jede Anforderung erhält eine eindeutige Kennung,
- jede Entscheidung wird dokumentiert,
- jedes Arbeitspaket wird einem Projektbereich zugeordnet.

---

## 14 Steuerungsobjekte

Für die Projektsteuerung werden folgende Objekte verwendet:

| Objekt | Kürzel | Zweck |
|---|---:|---|
| Anforderung | `ANF` | Beschreibung fachlicher oder technischer Anforderungen |
| Arbeitspaket | `AP` | konkrete Umsetzungseinheit |
| Testfall | `TF` | prüfbarer Nachweis |
| Architekturentscheidung | `AE` | dokumentierte technische oder fachliche Entscheidung |
| Risiko | `RI` | Projektrisiko oder technisches Risiko |
| Klärungspunkt | `KP` | offener Punkt |
| Änderungsantrag | `ÄA` | Änderung am vereinbarten Umfang |
| Meilenstein | `MS` | prüfbares Zwischenergebnis |

---

## 15 Erfolgsfaktoren

Für den Projekterfolg sind besonders wichtig:

- klare Trennung zwischen Plattform und Portal,
- keine doppelte Fachlogik,
- konsequente Nutzung der bestehenden SportFM-Fachlogik,
- belastbare Anforderungsliste,
- frühzeitige fachliche Abnahmen,
- nachvollziehbare Aufwandsschätzung,
- konsequente Dokumentation von Änderungen,
- regelmäßige Abstimmung zwischen Fachbereich und IT.

---

## 16 Abgrenzung

Dieses Dokument beschreibt die organisatorische Struktur.

Nicht enthalten sind:

- detaillierte Rollenbeschreibungen,
- konkrete Personenbesetzung,
- detaillierte Zeitplanung,
- Aufwandsschätzung,
- technische Architektur.

Diese Inhalte werden in eigenen Dokumenten beschrieben.
