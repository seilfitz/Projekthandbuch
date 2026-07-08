\
# Feature-Abhängigkeiten

| Dokument | 03_Domaenen/Feature_Abhaengigkeiten.md |
|-----------|----------------------------------------|
| Version | 1.0 |
| Stand | 08.07.2026 |
| Status | Entwurf |

# 1 Ziel

Dieses Dokument beschreibt die fachlichen und technischen Abhängigkeiten zwischen allen Domänen und Features von SportFM. Es dient als Grundlage für:

- Implementierungsreihenfolge
- Arbeitspaketplanung
- Aufwandsschätzung
- Sprintplanung
- Testreihenfolge
- Risikoanalyse

---

# 2 Abhängigkeitsarten

| Typ | Beschreibung |
|------|--------------|
| F | Fachliche Abhängigkeit |
| D | Datenabhängigkeit |
| W | Workflowabhängigkeit |
| S | Schnittstellenabhängigkeit |
| T | Technische Abhängigkeit |

---

# 3 Domänenabhängigkeiten

| Von | Nach | Typ | Beschreibung |
|------|------|-----|--------------|
| Benutzer | Vereine | F | Ansprechpartner und Rollen |
| Benutzer | Portal | F | Anmeldung und Registrierung |
| Vereine | Antrag | F | Antragsteller ist Verein oder Organisation |
| Sportanlagen | Reservierung | F | Reservierung benötigt Sportanlage |
| Reservierung | Antrag | W | Antrag erzeugt Reservierung |
| Antrag | Buchung | W | Genehmigter Antrag erzeugt Buchung |
| Buchung | Dokumente | W | Bescheide entstehen aus Buchungen |
| Buchung | Auswertungen | D | Belegungsdaten |
| Buchung | Schnittstellen | S | SAP, ePayBL |
| Veranstaltungen | Reservierung | W | Sperrzeiten beeinflussen Reservierungen |
| Portal | Antrag | F | Online-Antrag |
| Portal | Sportanlagen | F | Anzeige freier Zeiten |
| Schnittstellen | Portal | S | Karten, Payment, Formcycle |

---

# 4 Featureabhängigkeiten

## Benutzer

| Feature | Abhängig von |
|---------|---------------|
| Rollen verwalten | Benutzer anlegen |
| Berechtigungen | Rollen |
| Passwort zurücksetzen | Benutzer |

## Vereine

| Feature | Abhängig von |
|---------|---------------|
| Ansprechpartner | Verein |
| Dokumente | Verein |
| Mitgliedschaften | Verein |

## Sportanlagen

| Feature | Abhängig von |
|---------|---------------|
| Sportflächen | Sportanlage |
| Ausstattung | Sportfläche |
| Kartenposition | Sportanlage |
| Verfügbarkeiten | Sportfläche |

## Reservierung

| Feature | Abhängig von |
|---------|---------------|
| Konfliktprüfung | Sportanlage, Zeitraum |
| Prioritätsprüfung | Verein, Nutzer |
| Saisonlogik | Sportanlage |
| Warteliste | Konfliktprüfung |
| Historie | Reservierung |

## Antrag

| Feature | Abhängig von |
|---------|---------------|
| Validierung | Antrag |
| Anhänge | Antrag |
| Statuswechsel | Validierung |
| Archivierung | Status abgeschlossen |

## Buchung

| Feature | Abhängig von |
|---------|---------------|
| Serienbuchung | Buchung |
| Gebühren | Buchung |
| Widerruf | Buchung |
| Umbuchung | Buchung |
| Historie | Buchung |

## Dokumente

| Feature | Abhängig von |
|---------|---------------|
| Gebührenbescheid | Gebühren |
| Widerrufsbescheid | Widerruf |
| PDF | Dokument |
| Archiv | Dokument |

## Portal

| Feature | Abhängig von |
|---------|---------------|
| Registrierung | Benutzer |
| Anmeldung | Benutzer |
| Freie Zeiten | Sportanlage, Reservierung |
| Online-Antrag | Antrag |
| Benutzerkonto | Benutzer |

## Auswertungen

| Feature | Abhängig von |
|---------|---------------|
| Tagesplan | Buchung |
| Wochenplan | Buchung |
| Monatsplan | Buchung |
| Gebührenübersicht | Gebühren |
| Excel/PDF | Auswertung |

## Schnittstellen

| Schnittstelle | Voraussetzung |
|---------------|---------------|
| SAP | Buchung, Gebühren |
| ePayBL | Gebühren |
| Themenstadtplan | Sportanlagen |
| Formcycle | Antrag |
| LSBS | Vereine |
| Cardo | Sportanlagen |
| Schuldatenbank | Schulen |

---

# 5 Kritischer Implementierungspfad

1. Benutzerverwaltung
2. Rollen/Berechtigungen
3. Vereine
4. Sportanlagen
5. Sportflächen
6. Reservierung
7. Antrag
8. Buchung
9. Dokumente
10. Portal
11. Schnittstellen
12. Auswertungen

---

# 6 Risiken

| Risiko | Ursache | Auswirkung |
|---------|----------|------------|
| Verzögerte Sportanlagen | Stammdaten fehlen | Reservierung blockiert |
| Schnittstellen verspätet | Externe Systeme | Portal eingeschränkt |
| Gebührenlogik unvollständig | Satzungsänderungen | SAP-Anbindung verzögert |
| Rollenmodell unvollständig | Berechtigungen | Backend und Portal betroffen |

---

# 7 Nutzung

Dieses Dokument dient als verbindliche Grundlage für:

- Projektstrukturplan
- Arbeitspaketdefinition
- Sprintplanung
- Aufwandsschätzung
- Testplanung
- Releaseplanung

Jedes Arbeitspaket muss seine fachlichen und technischen Vorgänger berücksichtigen.
