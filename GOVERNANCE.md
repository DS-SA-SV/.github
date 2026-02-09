# Governance (ALM-Baseline)

Dieses Dokument beschreibt die minimalen Regeln für Application Lifecycle Management (ALM)
in dieser Organisation: von Intake (Issues) über Umsetzung (PRs) bis Betrieb (Release/Monitoring).

## Ziele
- Einheitliches Intake (Requirements/Bugs) mit nachvollziehbarer Entscheidungskette
- Reproduzierbarkeit (Build/Run), Nachweisbarkeit (Evidenz), Betriebsfähigkeit (Runbook/Monitoring)
- Security/Compliance-by-design (insb. Personendaten / Audit-Relevanz)

## Geltungsbereich
Gilt für alle Repositories in der Organisation, sofern ein Repo keine expliziten Abweichungen dokumentiert.

## Rollen & Verantwortlichkeiten
- **Owner (Issue Owner)**: fachlich/inhaltlich verantwortlich für ein Issue; definiert Akzeptanzkriterien und Evidenz.
- **Maintainer**: technisch verantwortlich für Repo; entscheidet über Prioritäten, Merge-Freigaben.
- **Reviewer**: prüft Änderungen (Qualität, Risiken, Tests, Doku).
- **Operations** (falls vorhanden): Betrieb/Monitoring/Incident-Response.

## Artefakte (Standard)
- **Issues** (Requirement/Bug) mit Formular
- **Pull Requests** mit PR-Template
- **Evidenz**: Logs, Screenshots, Testreports, GitHub Actions Runs, Artefakte im Repo
- Optional: **ADR** (Architecture Decision Records) für größere Entscheide

## Prozess: Issue → PR → Release

### 1) Intake
Wir nutzen Issue-Forms:
- **Requirement**: lösungsneutral, messbare Akzeptanzkriterien, Testnachweis, Compliance-Bezug
- **Bug**: Steps to reproduce, expected vs actual, Umgebung, Evidenz, Impact

**Namenskonventionen**
- Issue Titel-Prefix: `REQ-` bzw. `BUG-`
- Branch: `feature/REQ-<nr>-kurztext` oder `fix/BUG-<nr>-kurztext`
- PR Titel enthält Issue-Referenz: `REQ-123: ...` / `BUG-456: ...`

**Kategorien (Beispiele)**
- FR Funktional / DR Datenregeln / NFR Nicht-funktional / SR Security / BR Betrieb

### 2) Planung & Priorisierung
- Maintainer priorisiert gemeinsam mit Owner.
- Für SR/Datenschutz/Audit: ggf. zusätzliche Review-Pflicht und Evidenz.

### 3) Umsetzung
- Änderungen erfolgen über Pull Requests.
- PR muss die Checklist aus dem PR-Template erfüllen (Tests/Evidenz/Doku/Impact/Rollback).

### 4) Qualitätssicherung (Quality Gates)
Minimum:
- Akzeptanzkriterien sind testbar und erfüllt
- Evidenz/Logs sind verlinkt oder als Artefakt vorhanden
- Relevante Doku aktualisiert (README/Runbook/Changelog)

Zusatz (wenn relevant):
- Parser-/Schemaänderung dokumentiert (Migration/Backward-Compat)
- Security/Compliance geprüft (SR oder Personendatenbezug)
- Monitoring/Alerting angepasst

### 5) Release / Betrieb
- Release erfolgt je nach Repo über Tag/Release Notes oder über “main” als deploybarer Stand.
- Bei data pipelines: Release sollte Laufzeit-/Erfolgskennzahlen dokumentieren (SLO optional).

## Dokumentationsstandard
Minimum pro Repo:
- `README.md`: Zweck, Inputs/Outputs, Quickstart
- `config`/Secrets Hinweise (keine Geheimnisse im Repo)
- Runbook/Operations-Notizen, wenn produktiv betrieben

Optional:
- `docs/adr/` für Architekturentscheide (ADR-Template)
- `CHANGELOG.md` bei häufigen Releases

## Datenschutz / Personendaten
Wenn Personendaten betroffen sind:
- Datenminimierung: nur erforderliche Felder
- Logging: keine sensitiven Daten in Logs (oder nur maskiert)
- Zugriff: Rollen/Berechtigungen dokumentieren
- Aufbewahrung/Löschung: Regeln dokumentieren

## Incident / Problem Management (Kurz)
- Incident: akuter Betriebsausfall / kritischer Datenfehler
- Ticket/Issue anlegen (BUG-…), Evidenz sichern, Fix-PR, Post-Mortem kurz dokumentieren:
  - Ursache, Impact, Fix, Preventive Actions

## Abweichungen
Repo-spezifische Abweichungen müssen im Repo (z.B. `docs/governance.md`) dokumentiert sein.
