# SUPPORT

Dieses Dokument beschreibt, wie Support-Anfragen für **DS-SA-SV – Engineering & Analytics** gestellt und bearbeitet werden.

## 1. Wofür Support geleistet wird

Support umfasst:

- R Packages und interne R-Funktionen.
- Datenpipelines, Parsing, ETL und Datenqualitätschecks.
- Quarto-, R Markdown- und Reporting-Repositories.
- Shiny-Apps und Analytics-Anwendungen.
- GitHub Actions und wiederverwendbare Workflows.
- Reproduzierbarkeits-, Installations- und Betriebsfragen.

Security-Themen, Secrets oder versehentliche Datenveröffentlichungen sind keine normalen Support-Fälle. Sie werden gemäss `SECURITY.md` vertraulich gemeldet.

## 2. Support-Kanäle

### 2.1 GitHub Issues

Für Bugs, Requirements, Datenqualitätsprobleme, Dokumentationsfehler und technische Schulden wird ein GitHub Issue im betroffenen Repository eröffnet.

Verwende das passende Issue-Formular:

- Bug Report
- Feature Request
- Datenqualitätsproblem
- Dokumentation
- Betrieb / Deployment
- Frage

### 2.2 GitHub Discussions

Falls im Repository aktiviert, können allgemeine Fragen, Ideen oder Lösungsskizzen über Discussions geklärt werden.

### 2.3 Direkter Kontakt

Für organisatorische oder bereichsübergreifende Fragen:

```text
DS-SA-SV Maintainer-Team
Kontakt: fabian.berger@sa.zh.ch

```

### 2.4 Security und Datenschutz

Nicht öffentlich melden:

- Schwachstellen.
- Secrets, Tokens, Passwörter oder Schlüssel.
- Verdacht auf veröffentlichte Personen- oder besondere Personendaten.
- Verdacht auf veröffentlichte Verzeichnisse, Listen, Register, Dossiers, Aktenstrukturen oder sonstige schutzwürdige Informationen.
- Screenshots, Logs oder Artefakte mit sensiblen Inhalten.

Diese Fälle gemäss `SECURITY.md` melden.

## 3. Keine sensiblen Inhalte in Support-Anfragen

In Issues, Discussions, PR-Kommentaren, Screenshots, Logs und Attachments dürfen keine produktiven oder schutzwürdigen Inhalte enthalten sein.

Nicht posten:

- Besondere Personendaten gemäss IDG Zürich.
- Namen, Adressen, E-Mail-Adressen, Telefonnummern, Personalnummern, Fallnummern, Kundennummern oder andere Identifikatoren.
- Verzeichnisse, Listen, Register, Dossier- oder Aktenstrukturen.
- Rohdaten, Fachapplikations-Exporte, Produktionslogs oder BI-Exports.
- Interne Pfade, Hostnamen, Datenbanknamen, Berechtigungstabellen oder Sicherheitsdetails.
- Secrets, Tokens, `.Renviron`, `.env`, private Schlüssel oder Verbindungsstrings.

Stattdessen verwenden:

- Synthetische Minimalbeispiele.
- Reduzierte Strukturbeispiele ohne echte Werte.
- Fehlerauszüge, in denen alle sensiblen Werte ersetzt wurden.
- Reproduzierbare Schritte mit Dummy-Daten.

## 4. Vor dem Eröffnen eines Issues

Bitte prüfen:

- Gibt es bereits ein ähnliches Issue?
- Ist das Repository gemäss README eingerichtet?
- Wurde `renv::restore()` ausgeführt, falls `renv` verwendet wird?
- Tritt der Fehler mit aktuellen Abhängigkeiten aus `renv.lock` weiterhin auf?
- Ist das Beispiel mit synthetischen Daten reproduzierbar?
- Wurden Logs, Screenshots und Fehlermeldungen auf sensible Inhalte geprüft?

## 5. Gute Bug Reports

Ein guter Bug Report enthält:

```markdown
## Beschreibung
Kurze Beschreibung des Problems.

## Erwartetes Verhalten
Was hätte passieren sollen?

## Tatsächliches Verhalten
Was ist passiert?

## Reproduktion
1. ...
2. ...
3. ...

## Minimalbeispiel
R-Code mit synthetischen Daten.

## Umgebung
- Betriebssystem:
- R-Version:
- Repository-Version / Commit SHA:
- Package-Versionen oder renv.lock:

## Logs / Fehlermeldung
Nur bereinigte Logs ohne Secrets, Personen-, Verzeichnis- oder schutzwürdige Informationen.

## Zusatzkontext
Relevante fachliche oder betriebliche Hinweise.
```

## 6. Reproduzierbare R-Beispiele

Für R-Fragen bevorzugen wir ein kleines Beispiel mit synthetischen Daten.

Beispiel:

```r
library(tibble)
library(dplyr)

example_data <- tibble(
  id = 1:3,
  category = c("a", "b", "a"),
  value = c(10, 20, 30)
)

example_data |>
  group_by(category) |>
  summarise(total = sum(value), .groups = "drop")
```

Hilfreich ist auch:

```r
sessionInfo()
```

Vor dem Posten müssen Pfade, Benutzernamen, Hostnamen, Tokens und andere sensible Werte entfernt werden.

## 7. Datenqualitätsprobleme

Bei Datenqualitätsproblemen bitte keine Echtdaten posten. Beschreibe stattdessen:

- Betroffene Pipeline oder Regel.
- Erwartete fachliche Regel.
- Art der Abweichung.
- Anzahl betroffener Datensätze nur aggregiert und ohne kleine identifizierende Gruppen.
- Synthetisches Beispiel, das die Abweichung nachstellt.
- Zeitpunkt oder Release, ab dem das Problem beobachtet wurde.

Beispiel für ein synthetisches Datenqualitätsproblem:

```r
library(tibble)

input <- tibble(
  record_id = c("synthetic-1", "synthetic-2"),
  reporting_year = c(2024, 2024),
  amount = c(100, -50)
)
```

## 8. Support-Prioritäten

Richtwerte für die Triage:

| Priorität | Beschreibung | Zielreaktion |
|---|---|---|
| Kritisch | Produktionsausfall, Sicherheitsvorfall, veröffentlichte sensible Inhalte | sofortige Eskalation gemäss `SECURITY.md` oder Betriebsprozess |
| Hoch | Produktive Pipeline, App oder Report blockiert; wichtiger Termin gefährdet | 1 Arbeitstag |
| Normal | Bug ohne unmittelbaren Produktionsausfall, Feature Request, Datenqualitätsregel | 3 Arbeitstage |
| Niedrig | Dokumentation, Refactoring, Verbesserungsvorschlag | Best effort |

Die Zielreaktionen sind Richtwerte und keine Garantie. Kritische Security- oder Datenschutzvorfälle werden nicht öffentlich über Issues geführt.

## 9. Betriebsunterstützung

Für produktionsnahe Pipelines, Reports oder Shiny-Apps muss die Anfrage zusätzlich enthalten:

- Umgebung: Entwicklung, Test, Produktion.
- Zeitpunkt des Fehlers.
- Betroffener Workflow, Job oder Report.
- Letzter bekannter erfolgreicher Lauf.
- Bereinigter Fehlerauszug.
- Ob Wiederanlauf bereits versucht wurde.
- Auswirkungen auf Nutzerinnen, Nutzer oder Folgeprozesse, nur aggregiert und ohne sensible Details.

## 10. Was ausserhalb des Supports liegt

Nicht über den normalen Support bearbeitet werden:

- Freigabe produktiver Daten für GitHub.
- Analyse von Echtdaten in Issues oder Discussions.
- Veröffentlichung von Verzeichnissen, Listen, Registern oder Dossierinformationen.
- Austausch von Secrets oder Zugangsdaten.
- Security Incidents und Schwachstellenmeldungen.
- Rechtsverbindliche Datenschutz- oder IDG-Beurteilungen.

Diese Themen werden über die zuständigen Fach-, Security-, Compliance- oder Rechtsstellen behandelt.

## 11. Umgangston

Wir arbeiten sachlich, respektvoll und lösungsorientiert. Gute Support-Anfragen helfen allen Beteiligten: klarer Kontext, reproduzierbares Beispiel, keine sensiblen Inhalte.
