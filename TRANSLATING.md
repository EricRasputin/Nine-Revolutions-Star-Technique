# Translation Workflow

This document explains, step by step, how to translate a chapter using this repo's setup.

---

## Step 1: Get the raw Chinese text

1.  Copy the Chinese text of the chapter you want to translate from the source website.
2.  Create a new file in the `raws/` folder.
3.  Name it by chapter number: `1.md`, `2.md`, `3.md`, etc.
4.  Paste the Chinese text into the file and save.

**Example filename:** `raws/1.md`

---

## Step 2: Prepare the AI prompt

When you send the chapter to your AI translator (e.g., Claude, ChatGPT), you need to give it three things in this order:

1.  **The system/translation prompt** — Copy the contents of `assets/translation_prompt.md` and paste it as the system prompt or at the start of your message. This tells the AI *how* to translate.

2.  **The glossary** — Copy the contents of `assets/glossary.json` (or the relevant portion of it) and include it. This tells the AI *what terms to use*. For early chapters, the entire glossary will be small. As it grows to thousands of entries, you may want to filter it to only include terms from nearby chapters.

3.  **The raw chapter text** — Paste the Chinese text from the raw file.

**Example message to AI:**

```
[Paste translation_prompt.md here]

## Glossary
[Paste glossary.json here]

## Chapter to Translate
[Paste Chinese chapter text here]
```

---

## Step 3: Review the translation

Read through what the AI produces. Check for:

-   **Name consistency** — Are character names matching the glossary?
-   **Paragraph count** — Does the translation have the same number of paragraphs as the original?
-   **Readability** — Does it flow naturally in English?
-   **Missing content** — Did the AI skip or merge any sentences?

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

---

## Step 5: Update the glossary

After translating a chapter, check if any new terms appeared (new characters, places, techniques, items). If so:

1.  Use `assets/glossary_prompt.txt` — paste it after your translation and ask the AI to output the new terms in glossary format.
2.  Copy the new entries and add them to `assets/glossary.json`.
3.  Make sure each entry has: `en`, `cn`, `pinyin`, `type`, `gender`, and `file` (the chapter number where it first appeared).

---

## Step 6: Commit and push

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
-   **Review combat scenes carefully.** AI tends to make fights sound flat. Re-read and punch up the language if needed.
-   **Check pronoun usage.** Chinese doesn't distinguish he/she/it as strictly. The AI sometimes gets genders wrong — the glossary's `gender` field helps, but double-check.
