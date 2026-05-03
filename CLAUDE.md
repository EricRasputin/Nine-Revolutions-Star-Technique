# Project: Nine Revolutions Star Technique — English Fan Translation

This repo contains the raw Chinese chapters (`raws/N.md`), English translations (`translations/N.md`), and the assets that drive the translation pipeline (`assets/`).

## Workflow

Claude Code is the translator and editor for this project. Two documents define the workflow end-to-end:

- [`TRANSLATING.md`](./TRANSLATING.md) — raw Chinese → English draft.
- [`EDITING.md`](./EDITING.md) — draft → publishable chapter.

When the user provides raw Chinese (pastes it in chat, or points at `raws/N.md`, or simply says "translate chapter N"), follow `TRANSLATING.md` end-to-end without asking for confirmation at each step:

1. **Translate.** Read `assets/translation_prompt.md` and use it as the operating instructions. Read `assets/glossary.json` and apply established terminology. Produce the English draft.
2. **Save the draft.** Write to `translations/N.md` with the navigation header (`<!-- NAV START --> ... <!-- NAV END -->` block linking N-1, TOC, N+1) and the chapter heading (`# Chapter N: Title`) per Step 4 of `TRANSLATING.md`.
3. **Edit.** Run the three editing passes from `EDITING.md` (fidelity check, language cleanup, voice and rhythm) on the draft. Apply the find-and-fix checklist at the end of `EDITING.md` before declaring the chapter done.
4. **Update the glossary.** Per Step 6 of `TRANSLATING.md`, using the schema in `assets/glossary_prompt.txt`. Append new entries to `assets/glossary.json`.
5. **Commit and push.** Per Step 7 of `TRANSLATING.md`. Direct pushes to main are pre-authorized for this repo (see `.claude/settings.local.json`) — no need to ask.

Do not skip the editing passes. The raw AI draft is not the final output; nothing lands on main until it's been through `EDITING.md`.

Do not paste the contents of `assets/translation_prompt.md`, `assets/glossary.json`, or `assets/glossary_prompt.txt` into chat — read them from disk on demand.

## Project conventions

- **Author name:** "My Dog-Skin Plaster (我的狗皮膏药)" — use this exact rendering wherever the author is named (README, LICENSE, anywhere). Do not pinyin-romanize the name as "Wode Goupi Gaoyao."
- **Style sheet:** lives in `EDITING.md` under "Style Decisions." When you make a new judgment call that future chapters will need to follow, add a line there rather than re-deciding each time.
- **Glossary discipline:** all character, sect, place, technique, item, and rank names must resolve to a `glossary.json` entry by the time the chapter is committed. New names introduced in a chapter get appended in Step 4 above.
- **Structural fidelity:** keep the same paragraph count and sentence count as the raw Chinese. This is enforced in `EDITING.md` Pass 1 and is non-negotiable — downstream tooling assumes it.
