# merch-brief

A Claude Code skill for producing bespoke branded-merch creative briefs and complete deliverables — visual tech pack (range grid, product heroes, lookbook, embellishment macros, contextual placement), a cinematic hero image, a branded single-page presentation website with OG card, the real brand logo, and optional GitHub repo + Vercel deploy. Modeled on what a high-end merch agency hands a client.

Given a brand and an optional creative direction, the skill researches the brand, writes a tech-pack-grade `BRIEF.md`, runs a 7-round image-generation pipeline on Higgsfield (`nano_banana_pro` end-to-end), composes a branded single-page site with OG card, fetches the real brand logo, and optionally ships the whole thing to GitHub + Vercel.

## Install

### Option A — `npx skills` (recommended for most users)

```bash
npx skills add davidvictor/merch-brief-skill
```

This auto-detects your installed AI agents (Claude Code, Cursor, etc.) and installs the skill into the right location for each. To update later, re-run the same command.

### Option B — manual clone + symlink (recommended for the skill author / maintainers)

This is what the author uses on their own machine. The local install at `~/.claude/skills/merch-brief` becomes a symlink to the cloned working copy, so any local edit `git push`-able from the repo is instantly live in Claude Code without a copy step.

```bash
# clone the repo somewhere stable
git clone https://github.com/davidvictor/merch-brief-skill.git ~/dev/merch-brief-skill

# symlink the skill folder into Claude Code's global skills directory
ln -s ~/dev/merch-brief-skill/skills/merch-brief ~/.claude/skills/merch-brief

# verify the skill loads (start a Claude Code session and run /merch-brief — should appear in the skills list)
```

To update: `cd ~/dev/merch-brief-skill && git pull`. The symlink keeps the global install current.

### Option C — direct copy

```bash
git clone https://github.com/davidvictor/merch-brief-skill.git
cp -r merch-brief-skill/skills/merch-brief ~/.claude/skills/merch-brief
```

Updates require re-cloning and re-copying.

## Requirements

- **Claude Code** (this skill is designed for it; the Agent Skills standard means it should also work in Cursor / Codex / Gemini CLI etc., but it's only tested in Claude Code)
- **Higgsfield MCP** — image generation backbone (`nano_banana_pro` and `soul_2` models). Connect via the Claude Code MCP integration.
- **`uv`** — to run the OG card builder (`uv run --with Pillow build_og.py`). Install: `brew install uv` or `curl -LsSf https://astral.sh/uv/install.sh | sh`.
- **`gh` CLI** — only required for the optional ship phase (GitHub repo creation).
- **`vercel` CLI** — only required for the optional ship phase (Vercel deploy).

## What the skill produces

A typical run on `tier=standard` (~21 images / ~46 Higgsfield credits) generates:

| Phase | Output |
|---|---|
| 1 — Research | Higgsfield brand-kit fetch + web research |
| 2 — Brief | `BRIEF.md` — palette, motifs, range plan, embellishment plan |
| 3 — Pipeline | 21 images across 7 sequenced rounds (Cast portraits → Locations → Mnemonics → Mockups → Line sheet → Lookbook → Hero) |
| 4 — Real logo | `logo-<brand>.svg` from Wikimedia / Higgsfield brand-kit |
| 5 — Deliverable | `index.html` (branded single-page site), `og.png` (1200×630 social card), `INDEX.md` (gallery of job IDs + prompts) |
| 6 — Ship *(optional)* | Private GitHub repo + Vercel deploy |

## Repository layout

```
merch-brief-skill/
├── README.md          (this file)
├── LICENSE            (MIT)
└── skills/
    └── merch-brief/
        ├── SKILL.md       (workflow, ~810 lines)
        ├── glossary.md    (materials & technique vocabulary, ~240 lines, 10 sections)
        └── examples/
            └── line-sheet-stagecoach.png  (structural reference for tier=standard line sheet)
```

## Usage

In a Claude Code session, invoke directly:

```
/merch-brief
```

or invoke via natural language:

```
make merch for Bacardi for the Puerto Rico vs Mexico match
build a tour merch deck for Keshi's Requiem tour, dark gothic vintage washed, deluxe tier
Red Bull Air Race × Venice Beach edition, paddock language elevated for international, ship it
```

The skill gates on a brief sign-off before generation and gates again before any GitHub/Vercel push.

## License

MIT. See [LICENSE](./LICENSE).
