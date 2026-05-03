# Translation Workflow

This document explains, step by step, how to produce a first-draft English chapter from the raw Chinese.

The workflow is performed by **Claude Code reading this repo**. You do not need to copy-paste prompts or the glossary into a separate AI chat. Provide the raw Chinese (paste it in chat or point at `raws/N.md`) and Claude Code will load the translation prompt, glossary, and editing checklist from disk and run the steps below.

The output of this workflow is a **draft**, not a finished chapter. Once you have a draft saved in `translations/N.md`, switch to [`EDITING.md`](./EDITING.md) for the editing passes that turn the AI draft into a publishable chapter. Do not commit a chapter until it has been through the editing workflow.

---

## Step 1: Get the raw Chinese text

1.  Copy the Chinese text of the chapter you want to translate from the source website.
2.  Create a new file in the `raws/` folder.
3.  Name it by chapter number: `1.md`, `2.md`, `3.md`, etc.
4.  Paste the Chinese text into the file and save.

**Example filename:** `raws/1.md`

---

## Step 2: Run the translation

The translation pass is performed by Claude Code, using two files in this repo:

-   [`assets/translation_prompt.md`](./assets/translation_prompt.md) — the system prompt. Translation principles, fidelity constraints, style directives, naming rules, and output format. Treat this as the operating instructions for any translation pass.
-   [`assets/glossary.json`](./assets/glossary.json) — the term dictionary. Characters, sects, places, techniques, items, ranks. Ensures consistency across chapters.

Do not paste either file into the conversation. Claude Code reads both from disk when it sees a translation task. To translate a chapter, just say something like:

> Translate `raws/7.md`.

…or paste the raw Chinese directly into chat. Claude Code will load `translation_prompt.md` and `glossary.json`, run the translation, and produce the draft. Keeping the prompt and glossary as files (not inline pasted text) means edits to them take effect on every future chapter automatically.

If a single chapter needs extra guidance (e.g., "this is a flashback — keep the prose more contemplative"), add that as a one-line note in your message. Treat it as an addendum to `translation_prompt.md`, not a replacement.

---

## Step 3: Quick sanity check

Before saving, give the output a fast scan to catch the catastrophic failures — anything more thorough belongs in the editing pass.

-   **Paragraph count** matches the raw (the editing pass enforces this strictly, but catch the obvious mismatches now).
-   **No whole sections were dropped or hallucinated.** The AI sometimes silently skips a paragraph or invents one.
-   **Output is in English.** Yes, this happens — sometimes the AI returns half the chapter still in Chinese.

Do not try to polish prose, fix pronouns, hunt ESL artifacts, or enforce style here. That is what `EDITING.md` is for. The goal of this step is just to confirm you have a usable draft worth saving.

---

## Step 4: Save the translation

1.  Create a new file in the `translations/` folder.
2.  Name it by chapter number: `1.md`, `2.md`, `3.md`, etc.
3.  Add the navigation header at the top (update the chapter numbers):

```markdown
<!-- NAV START -->
[Previous Chapter](./0.md) | [TOC](./) | [Next Chapter](./2.md)
<!-- NAV END -->
```

4.  Add the chapter heading:

```markdown
# Chapter 1: Rebirth
```

5.  Paste the translated text below the heading.

The file in `translations/N.md` at this point is a **draft**. It is not ready to publish.

---

## Step 5: Edit the chapter

Now switch to [`EDITING.md`](./EDITING.md) and run the three editing passes (fidelity check, language cleanup, voice and rhythm) on the draft.

Do not skip this step. AI translations consistently produce ESL-pattern errors — bad articles, missing plurals, ambiguous pronouns, weak intensifiers, passive overuse, flat combat — that the model will not catch on its own. `EDITING.md` is the project's checklist for fixing them while staying faithful to the source.

You should re-enter this `TRANSLATING.md` workflow at Step 6 (glossary update) only after the editing passes are complete.

---

## Step 6: Update the glossary

The glossary update follows the same handoff pattern as Step 2. The schema and instructions live in [`assets/glossary_prompt.txt`](./assets/glossary_prompt.txt) — Claude Code reads this from disk, you don't need to paste it.

Just say:

> Update the glossary for chapter N.

Claude Code will:

1.  Load `assets/glossary_prompt.txt` for the entry schema.
2.  Diff the chapter against `assets/glossary.json` to find genuinely new terms (new characters, places, techniques, items, ranks).
3.  Append the new entries to `assets/glossary.json`, ensuring each has `en`, `cn`, `pinyin`, `type`, `gender`, and `file` (the chapter number where it first appeared).

Skip this step only if you've already verified no new terms appeared in the chapter.

---

## Step 7: Commit and push

Only commit a chapter that has been through `EDITING.md`. The commit message should reflect that — use `(edited)` or omit it if obvious — but a commit landing on `main` should always be a publishable chapter, never a raw AI draft.

```bash
git add raws/1.md translations/1.md assets/glossary.json
git commit -m "Chapter 1: Rebirth"
git push
```

---

## Tips

-   **Translate in order.** The glossary builds on itself. Jumping ahead means you'll miss terms.
-   **Batch glossary updates.** It's fine to translate 5-10 chapters and then update the glossary in one go.
-   **Keep the raw files.** Even if the source is online, having a local copy means you can always re-translate or compare later.
-   **Don't try to edit during translation.** Resist the urge to clean up prose while saving the draft. Keep translation and editing as separate phases — that is what `EDITING.md` is structured around, and mixing the two is how fidelity errors slip through.
-   **Combat scenes and pronouns are the usual failure modes.** Don't try to fix them here — flag them mentally and handle them in `EDITING.md` Pass 1 (pronouns) and Pass 2l (combat).
