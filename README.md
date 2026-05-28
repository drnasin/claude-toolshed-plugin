# claude-toolshed-plugin

Claude Code plugin — workflow alati za svakodnevni razvoj.

## Što uključuje

- **codex-debate-partner** agent — Claude + Codex adversarial review
- **github-issue-implementor** agent — end-to-end GitHub issue workflow
- `/debate` slash komanda
- `/issue` slash komanda
- Windows Toast notifikacije (zahtijeva BurntToast)

## Preduvjeti

- Claude Code
- Codex CLI
- BurntToast: `Install-Module -Name BurntToast -Scope CurrentUser -Force`

## Instalacija

```bash
claude plugin install https://github.com/drnasin/claude-toolshed-plugin
```

## Korištenje

```
/debate treba li ovaj servis koristiti strategy pattern?
/issue 14
/issue 23
```
