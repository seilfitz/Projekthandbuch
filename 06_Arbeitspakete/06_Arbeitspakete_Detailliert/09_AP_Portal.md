# Arbeitspakete – Portal

| Dokument | 06_Arbeitspakete/POR_Arbeitspakete.md |
|----------|-------------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

---

# 1 Ziel

Dieses Dokument enthält die detaillierten Arbeitspakete für die Domäne **Portal**.

---

# 2 Arbeitspakete

## Registrierung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-001 | Registrierungsformular erstellen | N | siehe Feature-Abhängigkeiten | Registrierungsformular |
| AP-POR-002 | REST-Endpunkt Registrierung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Registrierung |
| AP-POR-003 | Businesslogik Registrierung/Mailbestätigung/Freischaltung umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Registrierung/Mailbestätigung/Freischaltung |
| AP-POR-004 | Datenmodell Portalbenutzer/Registrierungsstatus erstellen | N | siehe Feature-Abhängigkeiten | Datenmodell Portalbenutzer/Registrierungsstatus |
| AP-POR-005 | Validierung E-Mail/Passwort/Pflichtdaten umsetzen | N | siehe Feature-Abhängigkeiten | Validierung E-Mail/Passwort/Pflichtdaten |
| AP-POR-006 | öffentlichen Zugriff absichern | N | siehe Feature-Abhängigkeiten | öffentlichen Zugriff absichern |
| AP-POR-007 | Registrierung protokollieren | N | siehe Feature-Abhängigkeiten | Registrierung protokollieren |
| AP-POR-008 | Tests Registrierung/Dublette erstellen | T | siehe Feature-Abhängigkeiten | Tests Registrierung/Dublette |
| AP-POR-009 | Dokumentation Registrierungsprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Registrierungsprozess ergänzen |

## Anmeldung

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-010 | Loginseite erstellen | N | siehe Feature-Abhängigkeiten | Loginseite |
| AP-POR-011 | REST-Endpunkt Anmeldung implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Anmeldung |
| AP-POR-012 | Businesslogik Anmeldung/Token/2FA umsetzen | N | siehe Feature-Abhängigkeiten | Businesslogik Anmeldung/Token/2FA |
| AP-POR-013 | Datenzugriff Benutzer/Loginstatus anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Benutzer/Loginstatus anbinden |
| AP-POR-014 | Zugangsdaten validieren | N | siehe Feature-Abhängigkeiten | Zugangsdaten validieren |
| AP-POR-015 | Loginversuche protokollieren | N | siehe Feature-Abhängigkeiten | Loginversuche protokollieren |
| AP-POR-016 | Tests erfolgreicher/fehlgeschlagener Login erstellen | T | siehe Feature-Abhängigkeiten | Tests erfolgreicher/fehlgeschlagener Login |
| AP-POR-017 | Dokumentation Anmeldeprozess ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Anmeldeprozess ergänzen |

## Freie Zeiten suchen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-018 | Suchmaske freie Zeiten erstellen | N | siehe Feature-Abhängigkeiten | Suchmaske freie Zeiten |
| AP-POR-019 | REST-Endpunkt freie Zeiten implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt freie Zeiten |
| AP-POR-020 | Businesslogik Suche an Availability delegieren | N | siehe Feature-Abhängigkeiten | Businesslogik Suche an Availability delegieren |
| AP-POR-021 | Datenzugriff OCC/Winner/Sportanlagen anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff OCC/Winner/Sportanlagen anbinden |
| AP-POR-022 | Zeitraum/Uhrzeit/Sportart validieren | N | siehe Feature-Abhängigkeiten | Zeitraum/Uhrzeit/Sportart validieren |
| AP-POR-023 | öffentliche/angemeldete Sicht umsetzen | N | siehe Feature-Abhängigkeiten | öffentliche/angemeldete Sicht |
| AP-POR-024 | Suchanfragen optional protokollieren | N | siehe Feature-Abhängigkeiten | Suchanfragen optional protokollieren |
| AP-POR-025 | Tests freie/belegte Zeiten erstellen | T | siehe Feature-Abhängigkeiten | Tests freie/belegte Zeiten |
| AP-POR-026 | Dokumentation Suchparameter ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Suchparameter ergänzen |

## Sportanlagen anzeigen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-027 | Sportanlagenliste/Detailseite erstellen | N | siehe Feature-Abhängigkeiten | Sportanlagenliste/Detailseite |
| AP-POR-028 | REST-Endpunkt Sportanlagen Portal implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Sportanlagen Portal |
| AP-POR-029 | Businesslogik portalgeeignete Sportanlagendaten liefern | N | siehe Feature-Abhängigkeiten | Businesslogik portalgeeignete Sportanlagendaten liefern |
| AP-POR-030 | Datenzugriff Ausstattung/Bilder anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Ausstattung/Bilder anbinden |
| AP-POR-031 | Filterparameter validieren | N | siehe Feature-Abhängigkeiten | Filterparameter validieren |
| AP-POR-032 | öffentliche Sicht absichern | N | siehe Feature-Abhängigkeiten | öffentliche Sicht absichern |
| AP-POR-033 | Tests Liste/Detail/Filter erstellen | T | siehe Feature-Abhängigkeiten | Tests Liste/Detail/Filter |
| AP-POR-034 | Dokumentation Portalansicht Sportanlagen ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Portalansicht Sportanlagen ergänzen |

## Vereinsdaten anzeigen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-035 | Vereinsprofil erstellen | N | siehe Feature-Abhängigkeiten | Vereinsprofil |
| AP-POR-036 | REST-Endpunkt Vereinsdaten Portal implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Vereinsdaten Portal |
| AP-POR-037 | Businesslogik sichtbare Vereinsdaten bereitstellen | N | siehe Feature-Abhängigkeiten | Businesslogik sichtbare Vereinsdaten bereitstellen |
| AP-POR-038 | Datenzugriff Verein/Ansprechpartner/Sportarten anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Verein/Ansprechpartner/Sportarten anbinden |
| AP-POR-039 | Sichtbarkeitsregeln validieren | N | siehe Feature-Abhängigkeiten | Sichtbarkeitsregeln validieren |
| AP-POR-040 | öffentliche/angemeldete Felder umsetzen | N | siehe Feature-Abhängigkeiten | öffentliche/angemeldete Felder |
| AP-POR-041 | Tests öffentliche/private Felder erstellen | T | siehe Feature-Abhängigkeiten | Tests öffentliche/private Felder |
| AP-POR-042 | Dokumentation Sichtbarkeitsregeln ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Sichtbarkeitsregeln ergänzen |

## Veranstaltungen anzeigen

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-043 | Veranstaltungskalender erstellen | N | siehe Feature-Abhängigkeiten | Veranstaltungskalender |
| AP-POR-044 | REST-Endpunkt Veranstaltungen Portal implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkt Veranstaltungen Portal |
| AP-POR-045 | Businesslogik öffentliche Veranstaltungen bereitstellen | N | siehe Feature-Abhängigkeiten | Businesslogik öffentliche Veranstaltungen bereitstellen |
| AP-POR-046 | Datenzugriff Events/Eventtyp Veranstaltung anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Events/Eventtyp Veranstaltung anbinden |
| AP-POR-047 | Zeitraum/Freigabe validieren | N | siehe Feature-Abhängigkeiten | Zeitraum/Freigabe validieren |
| AP-POR-048 | öffentliche Sicht absichern | N | siehe Feature-Abhängigkeiten | öffentliche Sicht absichern |
| AP-POR-049 | Tests Kalenderanzeige erstellen | T | siehe Feature-Abhängigkeiten | Tests Kalenderanzeige |
| AP-POR-050 | Dokumentation Veranstaltungsportal ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Veranstaltungsportal ergänzen |

## Online-Antrag

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-051 | Wizard Runtime im Portal erstellen | N | siehe Feature-Abhängigkeiten | Wizard Runtime im Portal |
| AP-POR-052 | REST-Anbindung Antrag/Wizard implementieren | N | siehe Feature-Abhängigkeiten | REST-Anbindung Antrag/Wizard |
| AP-POR-053 | Businesslogik Antragstyp laden/speichern/einreichen anbinden | I | siehe Feature-Abhängigkeiten | Businesslogik Antragstyp laden/speichern/einreichen anbinden |
| AP-POR-054 | Datenzugriff Antrag/Wizard-Konfiguration nutzen | N | siehe Feature-Abhängigkeiten | Datenzugriff Antrag/Wizard-Konfiguration nutzen |
| AP-POR-055 | Dynamische Validierung im Portal anzeigen | N | siehe Feature-Abhängigkeiten | Dynamische Validierung im Portal anzeigen |
| AP-POR-056 | Berechtigung Portalnutzer umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung Portalnutzer |
| AP-POR-057 | Antragsschritte protokollieren | N | siehe Feature-Abhängigkeiten | Antragsschritte protokollieren |
| AP-POR-058 | Tests vollständiger Online-Antrag erstellen | T | siehe Feature-Abhängigkeiten | Tests vollständiger Online-Antrag |
| AP-POR-059 | Dokumentation Antragstellung Portal ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Antragstellung Portal ergänzen |

## Benutzerkonto

| AP-ID | Arbeitspaket | Typ | Abhängigkeit | Ergebnis |
|-------|--------------|-----|--------------|----------|
| AP-POR-060 | Kontoübersicht erstellen | N | siehe Feature-Abhängigkeiten | Kontoübersicht |
| AP-POR-061 | REST-Endpunkte Mein Konto implementieren | N | siehe Feature-Abhängigkeiten | REST-Endpunkte Mein Konto |
| AP-POR-062 | Businesslogik Profil/Organisation/Dokumente/Rechnungen bündeln | D | siehe Feature-Abhängigkeiten | Businesslogik Profil/Organisation/Dokumente/Rechnungen bündeln |
| AP-POR-063 | Datenzugriff Portalbenutzer/Organisation anbinden | I | siehe Feature-Abhängigkeiten | Datenzugriff Portalbenutzer/Organisation anbinden |
| AP-POR-064 | Profilfelder validieren | N | siehe Feature-Abhängigkeiten | Profilfelder validieren |
| AP-POR-065 | Berechtigung angemeldeter Benutzer umsetzen | N | siehe Feature-Abhängigkeiten | Berechtigung angemeldeter Benutzer |
| AP-POR-066 | Änderungen protokollieren | N | siehe Feature-Abhängigkeiten | Änderungen protokollieren |
| AP-POR-067 | Tests Profildaten/Rechte erstellen | T | siehe Feature-Abhängigkeiten | Tests Profildaten/Rechte |
| AP-POR-068 | Dokumentation Kontoübersicht ergänzen | D | siehe Feature-Abhängigkeiten | Dokumentation Kontoübersicht ergänzen |
