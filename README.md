# appsec-notes

Working notes from application security practice: drills, CVE teardowns, and lab writeups.

Written to be revisited, not to look busy. Each entry is short, dated, and ends with what I would actually do about it.

## Structure

| path | contents |
| --- | --- |
| `drills/` | Short, repeatable checks — things I can run against a target or a codebase in minutes |
| `cves/` | One vulnerability per file: root cause, exploit path, detection, fix |
| `labs/` | Deliberate-practice writeups (PortSwigger, HTB, TryHackMe) in my own words |
| `templates/` | Starting points for each note type |
| `bin/new-note` | Creates a dated file from the matching template |

## Index

Newest first.

### CVE teardowns

- **2026-11-06** — [Log4Shell (CVE-2021-44228)](cves/2026-11-06-log4shell.md) — JNDI lookup triggered by a logged string

### Drills

- **2026-11-06** — [HTTP response header audit](drills/2026-11-06-http-headers.md) — ten-minute pass over the headers that stop whole bug classes

### Labs

- nothing yet

## Method

1. `bin/new-note cve log4shell` → creates `cves/<date>-log4shell.md` from the template
2. Fill it in the same session — a half-written note beats a perfect one I never start
3. Link sources; if I cannot explain a claim without the link, it is not mine yet
4. Add a line to the index above

Cadence: one or two sessions a week.
