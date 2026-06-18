# GOVERNANCE

Dieses Dokument beschreibt die Governance und den ALM-Prozess von **DS-SA-SV – Engineering & Analytics** für R-, Analytics-, Datenpipeline-, Reporting- und Automatisierungs-Repositories.

## 1. Zweck

Die Organisation stellt Code, Workflows und Dokumentation bereit, die reproduzierbar, auditierbar und betriebsfähig sind. GitHub wird für Engineering-Artefakte genutzt, nicht als Ablage für produktive Daten, besonders schützenswerte Informationen, Verzeichnisse, Listen, Register oder Dossiers.

## 2. Leitprinzipien

- **Code offen nachvollziehbar, Daten geschützt:** Repositories enthalten Code, Tests, Dokumentation und synthetische Beispiele. Produktive oder schutzwürdige Daten bleiben in freigegebenen Datenplattformen und Fachsystemen.
- **R first:** R, Quarto, R Markdown, Shiny, testthat, renv und GitHub Actions sind zentrale Bausteine.
- **Reproduzierbarkeit:** Ergebnisse müssen mit dokumentierten Versionen, Parametern und Abhängigkeiten reproduzierbar sein.
- **Auditierbarkeit:** Entscheidungen, Reviews, Tests, Freigaben und Releases werden nachvollziehbar dokumentiert.
- **Betriebsfähigkeit:** Pipelines, Reports und Apps haben klare Verantwortlichkeiten, Monitoring- und Supportwege.
- **Security & Privacy by default:** Minimale Berechtigungen, keine Secrets in Git, keine sensiblen Inhalte in Issues, PRs, Logs oder Artefakten.

## 3. Geltungsbereich

Diese Governance gilt für:

- R Packages und interne R Libraries.
- ETL-, Parsing- und Datenqualitäts-Pipelines.
- Quarto-, R Markdown- und Reporting-Repositories.
- Shiny-Apps und Analytics-Anwendungen.
- GitHub Actions und wiederverwendbare Workflows.
- Infrastruktur- und Deployment-Code, soweit in GitHub verwaltet.

## 4. Rollen

### 4.1 Organization Owner

Verantwortlich für:

- Organisationseinstellungen, Teams und Berechtigungen.
- Repository-Erstellung und Archivierung.
- Durchsetzung von Branch Protection, Rulesets und Security-Baselines.
- Eskalation bei Security- oder Compliance-Vorfällen.

### 4.2 Repository Maintainer

Verantwortlich für:

- Fachliche und technische Qualität des Repositories.
- Triage von Issues und Pull Requests.
- Review und Merge-Entscheide.
- Release Notes, Versionierung und Betriebsdokumentation.
- Sicherstellen, dass keine unzulässigen Daten oder Secrets versioniert werden.

### 4.3 Contributor

Verantwortlich für:

- Arbeit über Issues, Branches und Pull Requests.
- Einhalten von `CONTRIBUTING.md`.
- Tests, Dokumentation und Datenschutzprüfung der eigenen Änderungen.
- Sofortige Meldung bei versehentlichem Commit von Daten, Secrets oder schutzwürdigen Informationen.

### 4.4 Reviewer

Verantwortlich für:

- Prüfung von Code, Tests, Dokumentation und Reproduzierbarkeit.
- Prüfung auf Risiken in Datenschutz, Security und Betrieb.
- Nachvollziehbare Review-Kommentare und Freigaben.

### 4.5 Data Steward / Fachverantwortliche Stelle

Wird beigezogen bei:

- Datenmodellen, Kennzahlen und fachlichen Regeln.
- Datenqualitätschecks.
- Fragen zu Anonymisierung, Pseudonymisierung und Datenfreigabe.
- Berichten oder Auswertungen mit fachlicher Entscheidungswirkung.

### 4.6 Security / Compliance Kontakt

Wird beigezogen bei:

- Schwachstellen.
- Secrets in Git-Historie, Logs oder Artefakten.
- Verdacht auf Veröffentlichung von Personen- oder besonderen Personendaten.
- Verdacht auf Veröffentlichung von Verzeichnissen, Listen, Registern, Dossierstrukturen oder sonstigen schutzwürdigen Informationen.
- Änderungen an Berechtigungen, GitHub Actions, Deployment oder Sicherheitskonfiguration.

## 5. Repository-Klassen

### 5.1 Code-only Repository

Enthält Code, Tests und Dokumentation. Keine produktiven Daten. Standard für R Packages, Workflows und Tools.

Mindestanforderungen:

- `README.md`
- `CONTRIBUTING.md`
- `SECURITY.md`
- `.gitignore`
- CI-Checks für R, Tests und Linting, sofern sinnvoll
- Branch Protection oder Ruleset für `main`

### 5.2 Analytics / Reporting Repository

Enthält Quarto-, R Markdown- oder Analysecode. Keine produktiven Outputs mit schutzwürdigen Inhalten.

Zusätzliche Anforderungen:

- Dokumentierte Datenquellen ohne Secrets.
- Parameterisierung statt lokaler Pfade.
- Reproduzierbarer Render-Prozess.
- Synthetische oder anonymisierte Beispieldaten, falls Beispiele nötig sind.
- Keine gerenderten Reports mit Personen-, Fall-, Verzeichnis- oder internen Strukturinformationen.

### 5.3 Pipeline / ETL Repository

Enthält Parsing-, ETL- und Datenqualitätslogik. Keine Rohdaten, Exporte oder Produktionslogs.

Zusätzliche Anforderungen:

- Datenqualitätsregeln dokumentiert.
- Testfixtures synthetisch.
- Fehler- und Logging-Konzept ohne personenbezogene oder schutzwürdige Inhalte.
- Betriebsdokumentation für Scheduling, Monitoring und Wiederanlauf.

### 5.4 Shiny / App Repository

Enthält App-Code und Deployment-Konfiguration ohne Secrets.

Zusätzliche Anforderungen:

- Input-Validierung.
- Rollen-/Zugriffskonzept ausserhalb des Codes dokumentiert.
- Keine produktiven Daten im Repository.
- Logs und Fehlermeldungen ohne sensible Details.
- Review von Security/Compliance bei extern erreichbaren Apps.

### 5.5 Workflow / Automation Repository

Enthält GitHub Actions, wiederverwendbare Workflows oder Deployment-Automation.

Zusätzliche Anforderungen:

- Minimale Berechtigungen für `GITHUB_TOKEN`.
- Keine Secrets in YAML-Dateien.
- Secrets nur über GitHub Secrets oder freigegebene Secret Stores.
- Artefakte und Logs auf schutzwürdige Inhalte prüfen.
- Änderungen an produktionsrelevanten Workflows benötigen Maintainer- und Security-Review.

## 6. ALM-Prozess

### 6.1 Intake

Neue Anforderungen, Bugs und Risiken werden als Issue erfasst. Das Issue enthält:

- Problem oder Ziel.
- Kontext und betroffene Komponenten.
- Akzeptanzkriterien.
- Priorität und gewünschter Termin, falls relevant.
- Daten-, Security-, Compliance- oder Betriebsrelevanz.

### 6.2 Triage

Maintainer klassifizieren Issues nach:

- Typ: Bug, Feature, Datenqualität, Security, Dokumentation, Betrieb, technische Schuld.
- Priorität: kritisch, hoch, normal, niedrig.
- Risiko: Datenschutz, Security, fachliche Wirkung, Betriebswirkung.
- Zuständigkeit und Review-Bedarf.

Issues ohne ausreichende Beschreibung werden zurückgegeben oder mit Rückfragen ergänzt.

### 6.3 Planung

Für grössere Änderungen wird ein kurzer Umsetzungsplan dokumentiert:

- Lösungsansatz.
- Betroffene R Packages, Pipelines, Reports oder Workflows.
- Teststrategie.
- Rollback- oder Wiederanlaufplan.
- Migrations- oder Kommunikationsbedarf.

Architekturentscheidungen werden als ADR in `docs/adr/` dokumentiert.

### 6.4 Umsetzung

Umsetzung erfolgt auf Feature-, Fix- oder Chore-Branches. Direkt-Pushes auf `main` sind nicht erlaubt, ausser in dokumentierten Notfällen durch berechtigte Maintainer.

### 6.5 Review und Freigabe

Mindestfreigabe:

- Ein fachlich oder technisch zuständiger Maintainer.
- Erfolgreiche CI-Checks.
- Erfüllte PR-Checkliste.

Zusätzliche Freigaben:

- **Data Steward / Fachreview:** bei Kennzahlen, Datenmodellen, Datenqualitätsregeln oder fachlich relevanten Reports.
- **Security Review:** bei Authentisierung, Berechtigungen, Secrets, GitHub Actions, Deployment, externen Schnittstellen oder Schwachstellenbehebung.
- **Compliance Review:** bei Datenschutz-, IDG-, Archivierungs- oder Publikationsfragen.
- **Operations Review:** bei produktionsrelevanten Pipelines, Scheduling, Monitoring oder Shiny-Deployments.

### 6.6 Merge

Ein PR darf gemergt werden, wenn:

- Akzeptanzkriterien erfüllt sind.
- Alle required checks bestanden sind.
- Erforderliche Reviews vorliegen.
- Keine offenen Blocker bestehen.
- Datenschutz- und Security-Check bestätigt ist.

Merge-Strategie:

- Kleine lineare Änderungen: Squash merge.
- Grössere Feature-Historien: Merge commit, wenn Nachvollziehbarkeit dadurch besser ist.
- Reverts: eigener PR mit Begründung, ausser bei dringenden Incidents.

### 6.7 Release

Releases werden versioniert und dokumentiert.

Empfehlung:

- R Packages: SemVer, zum Beispiel `v1.4.2`.
- Pipelines/Reports: datierte Releases oder SemVer, abhängig vom Betriebskontext.
- Breaking Changes: klar markieren und Migrationshinweise liefern.
- Release Notes enthalten Änderungen, Tests, bekannte Einschränkungen und Betriebsrelevanz.

### 6.8 Betrieb

Für produktionsnahe Komponenten sind zu dokumentieren:

- Owner und Supportweg.
- Ausführungsplan oder Trigger.
- Benötigte Umgebungsvariablen ohne Werte.
- Monitoring und Alerting.
- Wiederanlauf nach Fehlern.
- Datenqualitätsprüfungen.
- Abhängigkeiten und externe Schnittstellen.

### 6.9 Archivierung und Stilllegung

Repos werden archiviert, wenn sie nicht mehr aktiv genutzt werden.

Vor Archivierung prüfen:

- Keine offenen produktiven Abhängigkeiten.
- Dokumentation des letzten gültigen Zustands.
- Security Advisories abgeschlossen.
- Keine schutzwürdigen Daten in Issues, PRs, Artefakten oder Historie.
- Repository auf read-only setzen oder archivieren.

## 7. Branch- und Schutzregeln

Für `main` gelten standardmässig:

- Pull Request erforderlich.
- Mindestens ein approving review.
- Required status checks.
- Keine offenen Conversations.
- Keine Force Pushes.
- Keine Deletes.
- Branches müssen aktuell sein, wenn das Repository dies verlangt.

Für produktionskritische Repositories zusätzlich:

- Zwei Reviews oder CODEOWNERS-Regeln.
- Security-/Compliance-Review bei relevanten Pfaden.
- Required checks für Tests, Linting, Secret Scanning und ggf. Quarto Render.
- Merge nur durch Maintainer.

## 8. CI/CD Mindeststandard für R

Je nach Repository-Typ sollen GitHub Actions mindestens prüfen:

- Installation der R-Abhängigkeiten.
- `renv::restore()` oder äquivalenter Restore.
- `testthat` Tests.
- `lintr` Linting.
- `devtools::check()` bei R Packages.
- Quarto Render bei Reporting-Repositories.
- Keine versehentliche Veröffentlichung von Artefakten mit Daten, Logs oder Reports.

CI darf keine produktiven Daten abrufen, ausser dies ist für eine freigegebene interne Umgebung ausdrücklich erlaubt und technisch abgesichert. Logs müssen frei von Personen-, Verzeichnis-, Secret- und schutzwürdigen Informationen bleiben.

## 9. Datenschutz- und Datenfreigabe

GitHub ist nicht der Ort für produktive Daten. Dies gilt unabhängig davon, ob ein Repository öffentlich, privat oder intern ist.

Nicht zulässig sind insbesondere:

- Besondere Personendaten gemäss IDG Zürich.
- Personendaten oder indirekt identifizierende Daten.
- Verzeichnisse, Listen, Register, Fallübersichten, Akten-/Dossierstrukturen.
- Fachapplikations-Exporte, Rohdaten, Produktionslogs und Screenshots mit Echtdaten.
- Interne Sicherheits-, Betriebs- oder Architekturinformationen mit Schutzbedarf.

Zulässig sind synthetische oder anonymisierte Beispiele nur dann, wenn:

- kein Personen-, Fall- oder Organisationsbezug rekonstruierbar ist;
- keine kleinen Zellzahlen oder eindeutigen Kombinationen Re-Identifikation ermöglichen;
- die Datenquelle und der Zweck dokumentiert sind;
- die Freigabe im PR nachvollziehbar ist.

## 10. Entscheidungsregeln

- Kleine technische Änderungen: Maintainer entscheidet im PR.
- Fachliche Änderungen: Maintainer plus Fachreview.
- Datenschutz- oder Security-relevante Änderungen: Maintainer plus Security/Compliance.
- Architekturentscheide: ADR und Review durch betroffene Maintainer.
- Uneinigkeit: Eskalation an Organization Owner oder definiertes Steering-Gremium.

Entscheide werden im Issue, PR oder ADR dokumentiert.

## 11. Ausnahmen

Ausnahmen von dieser Governance sind möglich, aber nur wenn sie:

- im Issue oder ADR begründet sind;
- zeitlich oder sachlich begrenzt sind;
- von den zuständigen Rollen genehmigt wurden;
- keine unzulässige Veröffentlichung von Daten, Verzeichnissen, Secrets oder schutzwürdigen Informationen bewirken.

## 12. Review-Zyklus

Diese Governance wird mindestens jährlich oder bei wesentlichen Änderungen an Organisation, Technologie, Recht, Betrieb oder Sicherheitslage überprüft.
