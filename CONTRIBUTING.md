# CONTRIBUTING

Willkommen bei **DS-SA-SV – Engineering & Analytics**. Dieses Dokument beschreibt, wie Änderungen an unseren R-, Analytics-, Datenpipeline- und Automatisierungs-Repositories eingebracht werden.

Unser Ziel: Beiträge sollen reproduzierbar, prüfbar, datenschutzkonform und betriebsfähig sein.

## 1. Grundsätze

Alle Beiträge folgen diesen Grundsätzen:

- **Issue zuerst:** Requirements, Bugs, technische Schulden und Datenqualitätsprobleme werden als GitHub Issue erfasst.
- **Pull Request statt Direkt-Push:** Änderungen an `main` erfolgen über Pull Requests.
- **Reproduzierbarkeit:** Analysen, Pipelines und Reports müssen mit dokumentierten Abhängigkeiten erneut ausführbar sein.
- **Auditierbarkeit:** Entscheidungen, Tests, Review-Kommentare und Evidenz bleiben im Issue/PR nachvollziehbar.
- **Datenschutz by default:** In GitHub gehören Code, Dokumentation, synthetische Beispieldaten und Konfigurationstemplates; keine produktiven, personenbezogenen oder sonst schutzwürdigen Daten.

## 2. Daten- und Geheimnisschutz

### 2.1 Nie auf GitHub hochladen

Folgende Inhalte dürfen nicht in Repositories, Issues, Pull Requests, Commit-Historien, Screenshots, Attachments, GitHub Actions Logs oder Artefakten erscheinen:

- **Besondere Personendaten gemäss IDG Zürich**, insbesondere Angaben zu Gesundheit, Intimsphäre, religiösen, weltanschaulichen, politischen oder gewerkschaftlichen Ansichten oder Tätigkeiten, ethnischer Herkunft, sozialer Hilfe, administrativen oder strafrechtlichen Verfolgungen oder Sanktionen.
- **Personendaten und identifizierende Merkmale**, zum Beispiel Namen, Privatadressen, E-Mail-Adressen, Telefonnummern, Geburtsdaten, Personalnummern, Fallnummern, AHV-Nummern, Kundennummern, Schüler-/Klientendaten, IP-Adressen oder andere direkt oder indirekt identifizierende Informationen.
- **Verzeichnisse, Listen und Register**, sofern sie Personen, Fälle, Organisationseinheiten, Standorte, Dossiers, Akten, Kunden, Klienten, Mitarbeitende, Lernende, Lieferanten oder interne Strukturen offenlegen.
- **Produktionsdaten, Rohdaten, Exporte und Logs** aus Fachapplikationen, Datenbanken, Fileshares, BI-Systemen, Shiny-Apps oder ETL-Jobs.
- **Schutzwürdige interne Informationen**, zum Beispiel Systemarchitekturen, Netzwerkpläne, interne Pfade, Hostnamen, Datenbanknamen, Berechtigungsmodelle, Aktenpläne, Verzeichnisstrukturen, Betriebsdokumentation mit Sicherheitsbezug oder Informationen, die Rückschlüsse auf Schwachstellen erlauben.
- **Secrets und Zugangsdaten**, zum Beispiel Passwörter, Tokens, API Keys, private Schlüssel, Zertifikate, Datenbankverbindungsstrings, Service-Accounts, OAuth-Secrets oder Inhalte aus `.Renviron`, `.env` und ähnlichen Dateien.

### 2.2 Erlaubt

Erlaubt sind:

- Code, Tests, Dokumentation und Konfigurationstemplates ohne Geheimnisse.
- Synthetische oder vollständig anonymisierte Beispieldaten, sofern keine Re-Identifikation möglich ist.
- Kleine Testdaten unter `inst/extdata/` oder `tests/testthat/fixtures/`, wenn sie ausdrücklich als synthetisch oder anonymisiert dokumentiert sind.
- Schemas, Datenmodelle und Variablenbeschreibungen, sofern daraus keine geschützten Inhalte, internen Verzeichnisse oder Fallbezüge ableitbar sind.

### 2.3 Vor jedem Commit prüfen

Vor jedem Commit ist zu prüfen:

```bash
git status
git diff --cached
```

Zusätzlich empfohlen:

```bash
gitleaks detect --source .
```

Wenn versehentlich Daten oder Secrets committet wurden: nicht weiterpushen, sofort Maintainer oder Security-Kontakt informieren und nach `SECURITY.md` vorgehen.

## 3. R-Entwicklungsstandard

### 3.1 Projektstruktur

Je nach Repository-Typ verwenden wir eine klare R-Struktur:

```text
.
├── R/                         # R-Funktionen
├── tests/testthat/            # Unit Tests
├── inst/extdata/              # kleine synthetische Beispieldaten, falls nötig
├── data-raw/                  # Skripte zur Erzeugung synthetischer oder freigegebener Daten
├── analysis/                  # reproduzierbare Analysen, falls relevant
├── reports/                   # Quarto-/RMarkdown-Quellen, keine vertraulichen Outputs
├── man/                       # generierte Dokumentation bei R Packages
├── DESCRIPTION                # Package-Metadaten oder Projektabhängigkeiten
├── renv.lock                  # reproduzierbare R-Abhängigkeiten, falls renv verwendet wird
└── README.md
```

Nicht in Git gehören lokale Arbeitsverzeichnisse wie `data/`, `raw/`, `input/`, `output/`, `exports/`, `private/`, `tmp/`, `logs/`, `dossiers/`, `akten/`, `register/` oder `verzeichnisse/`.

### 3.2 Abhängigkeiten

Für R-Projekte gilt:

- Projektabhängigkeiten werden bevorzugt mit `renv` fixiert.
- `renv.lock` wird committet, wenn das Repository reproduzierbar ausführbar sein soll.
- Neue Packages werden im PR begründet.
- Nicht benötigte Abhängigkeiten werden entfernt.
- Systemabhängigkeiten werden in `README.md`, `DESCRIPTION`, Dockerfile oder Setup-Dokumentation beschrieben.

Empfohlene Befehle:

```r
renv::restore()
renv::snapshot()
```

### 3.3 Stil und Wartbarkeit

R-Code soll verständlich, modular und testbar sein:

- Funktionen statt langer Skripte.
- Sprechende Namen, bevorzugt `snake_case`.
- Keine absoluten lokalen Pfade wie `C:/Users/...` oder `/home/name/...`.
- Pfade über `here::here()`, Projektkonfiguration oder Umgebungsvariablen verwalten.
- Keine Secrets im Code; Konfigurationswerte über Umgebungsvariablen oder sichere Secret Stores.
- Fehler klar behandeln und aussagekräftige Fehlermeldungen schreiben.
- Nebenwirkungen vermeiden, ausser sie sind dokumentiert.

Empfohlene Prüfungen:

```r
styler::style_pkg()
lintr::lint_package()
testthat::test_local()
devtools::check()
```

Für einfache Analyse-Repositories ohne Package-Struktur können äquivalente Checks verwendet werden, zum Beispiel `lintr::lint_dir()` und projektspezifische Testskripte.

### 3.4 Tests

Jede fachlich relevante Änderung braucht geeignete Tests oder nachvollziehbare Evidenz:

- Unit Tests mit `testthat` für Funktionen.
- Snapshot- oder Render-Tests für Reports, sofern sinnvoll.
- Datenqualitätschecks für ETL- und Parsing-Pipelines.
- Regressionstests für Bugfixes.
- Manuelle Evidenz nur, wenn automatisierte Tests unverhältnismässig sind; sie muss im PR dokumentiert werden.

Tests dürfen keine produktiven Daten verwenden. Testdaten müssen synthetisch, klein und dokumentiert sein.

### 3.5 Quarto, R Markdown und Reporting

Für Reports gilt:

- Quellen wie `.qmd`, `.Rmd` oder R-Skripte werden versioniert.
- Gerenderte Outputs werden nur versioniert, wenn sie öffentlich freigegeben und frei von schutzwürdigen Inhalten sind.
- Parameter, Datenquellen und Filter werden dokumentiert.
- Reports müssen ohne lokale Spezialpfade reproduzierbar sein.
- Caches und Render-Artefakte werden nicht committet, ausser es gibt einen dokumentierten Grund.

### 3.6 Shiny und Automatisierung

Für Shiny-Apps, APIs und Automatisierung gilt:

- Konfiguration über Umgebungsvariablen oder sichere Deployment-Konfiguration.
- Keine Tokens, Passwörter oder produktiven Verbindungsdaten im Repository.
- Eingaben validieren und Fehlermeldungen ohne sensible Details ausgeben.
- Logs dürfen keine personenbezogenen Daten, Secrets oder internen Verzeichnisse enthalten.
- GitHub Actions verwenden minimale Berechtigungen und schreiben keine sensiblen Artefakte.

## 4. Workflow

### 4.1 Issue erstellen

Nutze das passende Issue-Formular:

- Bug Report
- Feature Request
- Datenqualitätsproblem
- Technische Schuld
- Dokumentation
- Betriebs-/Deployment-Thema

Ein gutes Issue enthält:

- Ziel oder Problem.
- Betroffenes Repository, Package, Pipeline, Report oder Workflow.
- Erwartetes Verhalten.
- Reproduzierbare Schritte oder Beispiel mit synthetischen Daten.
- Akzeptanzkriterien.
- Relevante Compliance- oder Betriebsanforderungen.

### 4.2 Branch erstellen

Branch-Namen sollen kurz und nachvollziehbar sein:

```text
feature/<issue-nr>-kurzbeschreibung
fix/<issue-nr>-kurzbeschreibung
docs/<issue-nr>-kurzbeschreibung
chore/<issue-nr>-kurzbeschreibung
```

Beispiele:

```text
feature/42-add-parser-tests
fix/57-handle-empty-input
docs/61-update-renv-setup
```

### 4.3 Committen

Commits sollen klein und nachvollziehbar sein.

Empfohlenes Format:

```text
<typ>: <kurze beschreibung>
```

Beispiele:

```text
feat: add parser for quarterly export
fix: handle missing date column
 test: add regression case for empty input
docs: document renv restore workflow
chore: update github action versions
```

### 4.4 Pull Request eröffnen

Jeder PR enthält:

- Link zum Issue.
- Kurze Beschreibung der Änderung.
- Tests/Evidenz.
- Hinweise zu R-Abhängigkeiten und `renv.lock`.
- Datenschutz- und Security-Bestätigung.
- Screenshots nur, wenn sie vollständig anonymisiert und freigegeben sind.

PR-Titel:

```text
[<issue-nr>] <kurzer Titel>
```

### 4.5 Pull Request Checkliste

Vor Review muss die Checkliste erfüllt sein:

- [ ] Issue verlinkt und Akzeptanzkriterien adressiert.
- [ ] R-Code ist verständlich, modular und dokumentiert.
- [ ] Tests wurden ergänzt oder begründete Evidenz wurde dokumentiert.
- [ ] `testthat`, `lintr`, `devtools::check()` oder projektbezogene Checks laufen erfolgreich.
- [ ] `renv.lock` wurde aktualisiert, falls Abhängigkeiten geändert wurden.
- [ ] README, Quarto/RMarkdown, roxygen2-Dokumentation oder Betriebsdokumentation wurden bei Bedarf aktualisiert.
- [ ] Keine produktiven Rohdaten, Personendaten, besonderen Personendaten oder schutzwürdigen Informationen enthalten.
- [ ] Keine Verzeichnisse, Listen, Register, Dossierstrukturen oder internen Pfadangaben enthalten, sofern nicht ausdrücklich freigegeben.
- [ ] Keine Secrets, Tokens, Keys, `.Renviron`, `.env`, Logs oder lokalen Konfigurationsdateien enthalten.
- [ ] GitHub Actions Artefakte und Logs wurden auf schutzwürdige Inhalte geprüft.

### 4.6 Review

Mindestens ein Maintainer prüft den PR. Zusätzliche Reviews sind erforderlich bei:

- Änderungen an Datenmodellen, ETL-Logik oder Qualitätsregeln.
- Änderungen an Security, Deployment, GitHub Actions oder Berechtigungen.
- Änderungen mit fachlicher Wirkung auf Reports, Kennzahlen oder Entscheide.
- Änderungen an öffentlichen Schnittstellen, Package-APIs oder Shiny-Apps.
- Änderungen, die Datenschutz, IDG, Archivierung, Governance oder Compliance berühren.

Review-Kommentare werden sachlich und lösungsorientiert geführt. Änderungen werden erst gemergt, wenn Checks bestanden, offene Punkte geklärt und erforderliche Freigaben dokumentiert sind.

## 5. Definition of Done

Eine Änderung ist fertig, wenn:

- Akzeptanzkriterien erfüllt sind.
- Tests und relevante Checks bestanden sind.
- Dokumentation aktualisiert ist.
- Reproduzierbarkeit gegeben ist.
- Datenschutz- und Security-Check bestanden ist.
- PR genehmigt und gemergt wurde.
- Bei Releases: Version, Changelog und Release Notes aktualisiert sind.

## 6. Lokales Setup für R

Typischer Start:

```bash
git clone <repo-url>
cd <repo>
```

```r
install.packages("renv")
renv::restore()
```

Package-Projekte:

```r
devtools::load_all()
testthat::test_local()
devtools::check()
```

Quarto-Projekte:

```bash
quarto render
```

Shiny-Projekte:

```r
shiny::runApp()
```

## 7. `.gitignore` Mindeststandard

Repositories sollen mindestens folgende Muster ignorieren, sofern projektspezifisch nicht anders begründet:

```gitignore
# R local state
.Rhistory
.RData
.Ruserdata
.Rproj.user/

# Secrets and local config
.Renviron
.env
*.pem
*.key
*.crt
*.p12
*.pfx

# Data and exports
data/
raw/
raw-data/
input/
output/
outputs/
exports/
private/
tmp/
logs/
cache/

# Sensitive directories/lists/registers
dossiers/
akten/
register/
verzeichnisse/
falllisten/
personenlisten/

# Common data files; commit only synthetic approved fixtures by exception
*.csv
*.tsv
*.xlsx
*.xls
*.parquet
*.feather
*.rds
*.Rds
*.sav
*.dta
*.sas7bdat

# Quarto/R Markdown artefacts
*_cache/
*_files/
.quarto/
```

Wenn synthetische Testdaten versioniert werden müssen, ist dies im PR zu begründen und im README oder in einem `DATA.md` zu dokumentieren.

## 8. Fragen

Für allgemeine Fragen siehe `SUPPORT.md`. Für Schwachstellen, Secrets oder versehentliche Datenveröffentlichungen siehe `SECURITY.md`.
