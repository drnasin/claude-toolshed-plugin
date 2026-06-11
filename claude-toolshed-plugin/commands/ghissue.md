---
description: Pokreni github-issue-implementor agenta za end-to-end implementaciju GitHub issuea
---

Pokreni `github-issue-implementor` agenta za sljedeći GitHub issue:

$ARGUMENTS

Prije planiranja i implementacije obavezno provjeri postoji li project-local AI context:

- `.ai/invariants.md`
- `.ai/conventions.md`
- `.ai/known-pitfalls.md`
- `.ai/release-checklist.md`
- `.ai/review-packet-template.md`
- `.ai/smoke-tests.md`

Koristi te fileove kao repository evidence za:
- scope control
- invariants
- known pitfalls
- verification commands
- review packet
- release/rollback caution

Ne širi scope izvan issuea osim ako repository evidence jasno pokazuje da je nužno.