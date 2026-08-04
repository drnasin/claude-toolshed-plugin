---
description: Pokreni github-issue-implementor agenta za end-to-end implementaciju GitHub issuea
---

Pokreni `github-issue-implementor` agenta za sljedeći GitHub issue:

$ARGUMENTS

Prije planiranja i implementacije obavezno provjeri postoji li project-local AI context:

- pregledaj relevantne datoteke u `.ai/` direktoriju ako postoji
- tretiraj samo potvrđeni, ne-placeholder sadržaj kao repository evidence
- slijedi pravila agenta za project map, conventions, invariants, pitfalls, regressions, verification, browser flows, review i release checks

Ne širi scope izvan issuea osim ako repository evidence jasno pokazuje da je nužno.

Svaku poruku korisniku prepričaj jasnim, normalnim hrvatskim rečenicama. Ne
kopiraj tehničke kratice, nizove podebljanih riječi ili sažetke agenta koje
korisnik ne može razumjeti bez čitanja koda.

Ako issue traži neriješenu poslovnu, pravnu, novčanu ili drugu korisnički
vidljivu odluku, agent ne smije sam izabrati. Mora objasniti najviše tri
mogućnosti, postaviti jedno jasno pitanje i pričekati odgovor prije izmjena.

Ova komanda ne odobrava merge sama po sebi. Mergeaj samo ako je to izričito navedeno u `$ARGUMENTS` ili ga korisnik naknadno odobri.
