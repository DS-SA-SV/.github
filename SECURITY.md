# Security Policy

## Supported Versions
Wir unterstützen grundsätzlich den aktuellen `main`-Stand. Für Releases/Tags siehe jeweiliges Repo.

## Reporting a Vulnerability
Bitte **keine Sicherheitslücken öffentlich** als Issue melden.

Stattdessen:
- E-Mail an: sozialversicherungen@sa.zh.ch
- Betreff: `[SECURITY] <Repo/Komponente> – kurze Beschreibung`
- Bitte enthalten:
  - Reproduktionsschritte / Proof-of-Concept (wenn möglich)
  - Impact-Einschätzung (z.B. Datenabfluss, Privilege Escalation)
  - Betroffene Version/Commit
  - Vorschlag für Fix/Mitigation (optional)

## Response Prozess (Ziel)
- Eingangsbestätigung: innerhalb von **5 Arbeitstagen**
- Erste Einschätzung: innerhalb von **10 Arbeitstagen**
- Fix/Release je nach Kritikalität und Aufwand

## Umgang mit sensitiven Daten
- Keine Personendaten oder Secrets in:
  - Issues / PRs / Logs / Screenshots
- Falls zwingend: nur anonymisiert oder stark reduziert, und über sicheren Kanal.

## Known Security Considerations (typisch)
- Secrets: nur via GitHub Secrets / sichere Secret Stores, niemals im Repo
- Zugriffe: minimal notwendige Rechte (least privilege)
- Audit: Evidenz & Änderungsnachweise über PRs/Actions/Logs
