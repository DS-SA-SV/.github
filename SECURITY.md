# SECURITY

Dieses Dokument beschreibt den Umgang mit Schwachstellen, Secrets, Datenvorfällen und schutzwürdigen Informationen in **DS-SA-SV – Engineering & Analytics**.

## 1. Unterstützte Inhalte und Versionen

Security-Meldungen werden für aktiv gepflegte Repositories und Versionen bearbeitet.

| Status | Support |
|---|---|
| `main` / aktuelle Entwicklung | Security-Fixes werden priorisiert geprüft |
| Aktuelle veröffentlichte Version | Security-Fixes oder Hotfix-Releases möglich |
| Ältere Versionen | Best effort, sofern betrieblich erforderlich |
| Archivierte Repositories | Nur kritische Hinweise; Wiederaufnahme nach Entscheid der Owner |

Repository-spezifische Abweichungen können im jeweiligen README oder in Release Notes dokumentiert sein.

## 2. Security-Vorfälle vertraulich melden

Bitte keine öffentlichen Issues, Pull Requests oder Discussions für Security- oder Datenschutzvorfälle erstellen.

Melde vertraulich an:

```text
Security-Kontakt: fabian.berger@sa.zh.ch
```

Falls im Repository aktiviert, kann auch GitHub Private Vulnerability Reporting verwendet werden.

Bitte angeben:

- Betroffenes Repository.
- Betroffene Komponente, Datei oder Funktion.
- Beschreibung des Problems.
- Reproduzierbare Schritte mit synthetischen Daten.
- Mögliche Auswirkungen.
- Ob Secrets, Personen-, besondere Personen-, Verzeichnis- oder sonstige schutzwürdige Informationen betroffen sein könnten.
- Kontakt für Rückfragen.

Bitte nicht mitsenden:

- Echtdaten.
- besondere Personendaten gemäss IDG Zürich.
- Verzeichnisse, Listen, Register, Dossiers oder Aktenstrukturen.
- Tokens, Passwörter oder private Schlüssel, ausser nach expliziter sicherer Anweisung.
- Vollständige produktive Logs oder Dumps.

## 3. Was als Security- oder Datenschutzvorfall gilt

Melde insbesondere:

- Veröffentlichung oder Verdacht auf Veröffentlichung von Secrets.
- Veröffentlichung oder Verdacht auf Veröffentlichung von Personendaten.
- Veröffentlichung oder Verdacht auf Veröffentlichung besonderer Personendaten gemäss IDG Zürich.
- Veröffentlichung oder Verdacht auf Veröffentlichung von Verzeichnissen, Listen, Registern, Dossiers, Aktenstrukturen, internen Pfaden oder sonst schutzwürdigen Informationen.
- Zugriffsmöglichkeiten auf Daten oder Systeme, die nicht vorgesehen sind.
- Schwachstellen in Shiny-Apps, APIs, GitHub Actions, Packages, Deployment oder Berechtigungen.
- Unsichere Defaults, die zu Datenabfluss, Rechteausweitung oder unbeabsichtigter Veröffentlichung führen können.
- Logs, Artefakte oder Reports, die sensible Informationen enthalten.

## 4. Datenklassifizierung für GitHub

GitHub-Repositories dieser Organisation sind für Engineering-Artefakte vorgesehen. Sie sind nicht für produktive Datenablage bestimmt.

### 4.1 In GitHub erlaubt

- R-Code, Package-Strukturen, Tests und Dokumentation.
- Quarto-/RMarkdown-Quellen ohne schutzwürdige Outputs.
- GitHub Actions Workflows ohne Secrets.
- Synthetische Testdaten.
- Vollständig anonymisierte Beispiele, sofern Re-Identifikation ausgeschlossen ist.
- Konfigurationstemplates ohne Werte.

### 4.2 In GitHub verboten

- Besondere Personendaten gemäss IDG Zürich.
- Personendaten und indirekt identifizierende Daten.
- Verzeichnisse, Listen, Register, Fallübersichten, Dossierstrukturen oder Akteninformationen.
- Produktive Rohdaten, Exporte, BI-Dumps, Datenbank-Dumps oder Logs.
- Screenshots mit Echtdaten, internen Strukturen oder Berechtigungsinformationen.
- Interne System-, Netzwerk-, Plattform- oder Architekturinformationen mit Schutzbedarf.
- Zugangsdaten, Tokens, Schlüssel, Zertifikate, Verbindungsstrings oder `.Renviron`/`.env` Inhalte.

Diese Regeln gelten auch für private und interne Repositories sowie für Issues, PRs, Kommentare, Attachments, Wiki-Seiten, GitHub Actions Logs und Artefakte.

## 5. Secrets Management

Secrets dürfen nie im Repository gespeichert werden.

Nicht committen:

```text
.Renviron
.env
*.pem
*.key
*.p12
*.pfx
*.crt
secrets.yml
credentials.json
service-account*.json
```

Richtlinien:

- Lokale Secrets in `.Renviron` oder einem freigegebenen Secret Store verwalten, aber nicht committen.
- In GitHub Actions nur GitHub Secrets oder freigegebene OpenID-Connect-/Secret-Store-Mechanismen verwenden.
- Secrets nie in Logs ausgeben.
- Tokens mit minimalen Berechtigungen und begrenzter Laufzeit verwenden.
- Secrets bei Verdacht auf Offenlegung sofort rotieren oder widerrufen.

Beispiel für R:

```r
api_token <- Sys.getenv("SERVICE_API_TOKEN")
if (!nzchar(api_token)) {
  stop("SERVICE_API_TOKEN is not set", call. = FALSE)
}
```

## 6. Sofortmassnahmen bei versehentlichem Commit

Wenn Daten, Secrets oder schutzwürdige Informationen versehentlich committet wurden:

1. **Nicht weiterpushen**, wenn der Commit noch lokal ist.
2. **Sofort Security-Kontakt informieren**.
3. **Secret widerrufen oder rotieren**, falls Zugangsdaten betroffen sind.
4. **Betroffene Branches, PRs, Issues, Logs und Artefakte identifizieren**.
5. **Keine öffentlichen Diskussionen mit Details führen**.
6. **Entfernung koordinieren**. Das Löschen einer Datei im nächsten Commit reicht nicht aus, weil Inhalte in der Git-Historie bleiben können.
7. **Impact Assessment durchführen**: Art der Information, Umfang, Zugriffe, betroffene Systeme, notwendige Meldungen.
8. **Massnahmen dokumentieren** in einem vertraulichen Incident-Kanal.

Tools zur Historienbereinigung dürfen nur koordiniert eingesetzt werden, zum Beispiel `git filter-repo` oder ein durch die Organisation freigegebener Prozess. Nach einer Historienbereinigung müssen alle betroffenen Secrets trotzdem rotiert werden.

## 7. Secure R Development

Für R-Code gelten folgende Sicherheitsanforderungen:

- Keine Secrets hardcodieren.
- Keine produktiven Daten in Tests, Examples, Vignettes oder Quarto-Outputs.
- Eingaben validieren, insbesondere in Shiny-Apps, APIs und ETL-Pipelines.
- Fehlermeldungen und Logs ohne Personen-, Verzeichnis-, Secret- oder interne Systeminformationen.
- Keine unkontrollierte Ausführung dynamischen Codes, zum Beispiel über `eval(parse(...))`, wenn Eingaben nicht vollständig kontrolliert sind.
- Shell-Aufrufe nur mit validierten Parametern und ohne Secrets in Kommandozeilenargumenten.
- Externe Downloads nur aus vertrauenswürdigen Quellen und mit dokumentierter Herkunft.
- Abhängigkeiten regelmässig prüfen und nicht benötigte Packages entfernen.
- `renv.lock` aktuell halten, wenn `renv` verwendet wird.

## 8. Shiny, APIs und Reports

Für Shiny-Apps, APIs und Reports gilt zusätzlich:

- Authentisierung und Autorisierung müssen vor produktiver Nutzung geklärt sein.
- Server-seitige Validierung ist erforderlich; UI-Validierung allein reicht nicht.
- Keine internen Fehlerdetails an Nutzerinnen und Nutzer ausgeben.
- Downloads aus Apps dürfen keine schutzwürdigen Inhalte enthalten, ausser die App, Berechtigung und Datenbearbeitung sind dafür freigegeben.
- Quarto-/RMarkdown-Outputs vor Veröffentlichung prüfen.
- Caches, temporäre Dateien und Artefakte dürfen keine sensiblen Inhalte enthalten.

## 9. GitHub Actions und CI/CD

Für Workflows gilt:

- `permissions:` explizit und minimal setzen.
- Secrets nicht in Logs ausgeben.
- Keine produktiven Daten in CI laden, ausser ein freigegebener interner Betriebsprozess erlaubt dies ausdrücklich.
- Artefakte nur speichern, wenn sie keine sensiblen Inhalte enthalten.
- Pull Requests aus Forks erhalten keinen Zugriff auf produktive Secrets.
- Wiederverwendbare Workflows versionieren.
- Abhängige Actions regelmässig prüfen und für kritische Workflows restriktiv pinnen.
- Required checks für produktionsrelevante Branches aktivieren.

Beispiel für minimale Workflow-Berechtigungen:

```yaml
permissions:
  contents: read
```

## 10. Branch Protection und Reviews

Für schutzbedürftige und produktionsrelevante Repositories gelten:

- Protected `main` Branch oder aktives Ruleset.
- Pull Request vor Merge.
- Mindestens ein Review; bei Security-/Datenschutzrelevanz zusätzliche Freigabe.
- Required status checks.
- Keine Force Pushes auf geschützte Branches.
- CODEOWNERS für kritische Pfade, falls sinnvoll.

Kritische Pfade können sein:

```text
.github/workflows/
R/auth*
R/deploy*
R/db*
config/
inst/shiny/
renv.lock
DESCRIPTION
```

## 11. Abhängigkeiten und Supply Chain

- R-Abhängigkeiten werden über `renv`, `DESCRIPTION` oder dokumentierte Setup-Dateien nachvollziehbar verwaltet.
- Neue Abhängigkeiten müssen fachlich oder technisch begründet sein.
- Abhängigkeiten aus unbekannten Quellen sind zu vermeiden.
- Security-Hinweise zu Abhängigkeiten werden geprüft und priorisiert.
- Nicht mehr gepflegte oder unnötige Packages werden ersetzt oder entfernt, wenn dies verhältnismässig ist.

## 12. Reaktion und Koordination

Nach Eingang einer vertraulichen Meldung:

1. Eingang bestätigen, sofern Kontaktangaben vorhanden sind.
2. Schweregrad und Betroffenheit einschätzen.
3. Zuständige Maintainer, Owner, Security und Compliance beiziehen.
4. Fix oder Mitigation in privatem Arbeitsmodus koordinieren.
5. Tests und Regressionen durchführen.
6. Release, Hotfix oder Konfigurationsänderung bereitstellen.
7. Betroffene Stellen informieren, soweit erforderlich.
8. Lessons Learned dokumentieren und Präventionsmassnahmen ableiten.

Richtwerte:

| Schweregrad | Beispiele | Zielreaktion |
|---|---|---|
| Kritisch | aktive Ausnutzung, veröffentlichte Secrets, veröffentlichte besondere Personendaten | sofortige Eskalation |
| Hoch | unautorisierter Zugriff möglich, Datenabfluss plausibel | 1 Arbeitstag |
| Mittel | Schwachstelle mit begrenzter Auswirkung | 3 Arbeitstage |
| Niedrig | Hardening, Best-Practice-Abweichung | Best effort |

## 13. Verantwortungsvolle Offenlegung

Wir bitten meldende Personen:

- Informationen vertraulich zu behandeln, bis eine Lösung koordiniert wurde.
- Nur minimale synthetische Nachweise zu liefern.
- Keine produktiven Daten abzurufen, zu verändern oder zu veröffentlichen.
- Keine Systeme zu stören oder Lasttests ohne Freigabe durchzuführen.
- Keine öffentlichen Details zu veröffentlichen, bevor der Vorfall bewertet und behoben ist.

## 14. Kontaktpflege

Die Security-Kontakte und Eskalationswege werden mindestens jährlich überprüft oder sofort aktualisiert, wenn Rollen wechseln.
