# Contributing Guide

Danke fürs Mitwirken! Ziel: Änderungen sollen reproduzierbar, prüfbar und betrieblich tragbar sein.

## 1) Bevor du startest
- Gibt es ein passendes Issue (REQ-/BUG-)? Falls nicht: Issue erstellen.
- Für größere Änderungen: kurz im Issue absprechen (Scope/Approach/Evidenz).

## 2) Branching
- `main` ist der stabile Stand.
- Feature Branch:
  - `feature/REQ-<nr>-kurztext`
  - `fix/BUG-<nr>-kurztext`

## 3) Commits
Empfohlen: klare, kleine Commits.
Optional: Conventional Commits (z.B. `feat: ...`, `fix: ...`, `docs: ...`).

## 4) Pull Request (Pflicht)
- PR muss Issue referenzieren: `Closes #<issue>`
- PR Template vollständig ausfüllen (Tests/Evidenz/Impact/Rollback/Compliance).

## 5) Tests / Evidenz
Minimum:
- Nachvollziehbare Testschritte oder automatisierte Tests
- Evidenz verlinken (Action-Run, Log-Auszug, Screenshot, Datei im Repo)

Für Datenpipelines:
- Smoke-Test (kleiner Datensatz) + Ergebnisvergleich
- Parser-/Schemaänderung: Migrationshinweise/Backward-Compat prüfen

## 6) R / Data-Pipeline Konventionen (wenn zutreffend)
- Reproduzierbarkeit:
  - wenn verwendet: `renv.lock` gepflegt
  - Konfiguration über `config.yml` / Umgebungsvariablen (keine Secrets committen)
- `targets`:
  - Targets klar benennen, `description` nutzen
  - deterministische Outputs (Inputs deklarieren)
- Logging:
  - keine Personendaten in Logs; ggf. maskieren

## 7) Dokumentation
Wenn Verhalten/Interface sich ändert:
- README/Runbook aktualisieren
- Beispielaufrufe/Parameter aktualisieren
- ggf. Changelog/Release Notes

## 8) Code Review
- Reviewer prüft: Korrektheit, Lesbarkeit, Testbarkeit, Risiken (Datenschutz/Security), Doku.
- Bei SR/Personendaten: ggf. zweite Review-Stimme.

## 9) “Definition of Done”
- Akzeptanzkriterien erfüllt
- Evidenz vorhanden
- Relevante Doku aktualisiert
- Keine Secrets im Repo
- (wenn relevant) Migration/Rollback beschrieben
