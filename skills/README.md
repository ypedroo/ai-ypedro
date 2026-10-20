# skills

Single master repository for my agent skills, shared across every AI agent CLI
on this machine. Each harness's `skills/` directory stays a **real directory**
whose entries are symlinks into this repo — so one edit here reaches all of them.
Inspire by Fabio Akita's [my-skills](https://github.com/akitaonrails/my-skills).  

## Layout

Flat: one directory per skill, each with a `SKILL.md`.

## Consumers

| Harness | Skills directory | How it links |
| --- | --- | --- |
| Claude Code | `~/.claude/skills` | per-skill symlinks |
| Codex | `~/.codex/skills` | per-skill symlinks |
| Cursor | `~/.cursor/skills` | per-skill symlinks |
| opencode | `~/.config/opencode/skills` | real dir: per-skill symlinks + opencode-only entries |
| opencode (checkout) | `~/opencode/.opencode/skills` | per-skill symlinks |
| Kimi | `~/.kimi-code/skills` | whole-dir symlink → this repo |
| shared | `~/.agents/skills` | per-skill symlinks |
| alias | `~/skills` | symlink → `~/.claude/skills` |

Per-skill symlinks rather than replacing each `skills/` with one directory
symlink: the harness directories also hold entries that must **not** live here
(see below), and per-skill links let those sit alongside untouched.

## External components (inventory + update procedure)

Third-party skills and plugins installed around this repo. None of them
auto-update; re-check occasionally.

| Component | Source | Where it lives | Update procedure |
| --- | --- | --- | --- |
| humanizer | [blader/humanizer](https://github.com/blader/humanizer) | vendored in this repo (`humanizer/`) | re-pull upstream, strip `.git`, run `python3 humanizer/scripts/validate-package.py` |
| simplify | adapted from Addy Osmani's [agent-skills](https://github.com/addyosmani/agent-skills) | this repo (`simplify/`) | one-time adaptation; diff upstream manually when it drifts |


## Rules

`.gitignore` blocks `.venv/`, `__pycache__/`, binaries (`*.pdf`, images,
archives), and credential-shaped files (`.env`, `*.key`, `*token*`,
`*secret*`). Assessment PDFs and anything resembling a key or token must never
be committed here — the repo is public.

`humanizer` is vendored from <https://github.com/blader/humanizer> with its
`.git` stripped. Re-pull upstream manually if it needs updating.

## Adding a skill

```sh
mkdir ~/Projects/my-skills/<name>          # add SKILL.md
for h in ~/.claude/skills ~/.agents/skills ~/.codex/skills ~/opencode/.opencode/skills; do
  ln -s ~/Projects/my-skills/<name> "$h/<name>"
done
```

Harnesses read `SKILL.md` at startup, so restart a running agent to pick up a
newly added skill.
