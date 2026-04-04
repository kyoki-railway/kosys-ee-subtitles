---
name: translate-subtitles
description: Translate all Japanese .srt subtitle files into a specified language. Use when translating subtitles, creating localized subtitle files, or when the user mentions a language code like zh-Hans, ko, fr, de.
argument-hint: <language-code>
disable-model-invocation: true
---

# Translate Subtitles

Translate all Japanese `.srt` subtitle files in this project into **$ARGUMENTS**.

## Step 1: Identify source files

Find all `*.srt` files in the project root excluding files that already have a language suffix (e.g., `*.en-US.srt`, `*.zh-Hans.srt`).

Separate into two groups:
- **Episode files**: `kosys_ee_ep*.srt`, `kosys_ee_lite_ep*.srt` — translate from scratch
- **Compilation film**: `kosys_ee_movie_eirin.srt` — assemble from episode translations last (see Step 3b)

## Step 2: Read glossary and references

Read [GLOSSARY.md](../../../GLOSSARY.md) for standardized character names, organization names, technical terms, catchphrases, and cultural references. This is the single source of truth.

Read the reference scripts in `references/scripts/` and character profiles in `references/characters/` to understand who is speaking and their personality.

## Step 3: Translate episodes in parallel

Launch one translation agent per episode (or group of shorter episodes) to maximize throughput. Each agent MUST:

1. Read the source `.srt` file
2. Read `GLOSSARY.md`
3. Translate following these rules:
   - **SRT format**: Keep subtitle numbering, timestamps, and blank line separators exactly as-is
   - **Character names**: Use romanized names from the glossary (Akane, Hiroka, Yuina, Lt. Commander). For CJK target languages, use appropriate localized names if conventional, otherwise keep romanized.
   - **Speaker labels**: Convert parenthetical labels like （アカネ） to the glossary-standard name
   - **Technical terms**: Keep IT/security terms (DoS, SQL injection, CSIRT, BadUSB, etc.) as-is
   - **Railway terms**: Translate using the glossary's English equivalents as reference, then localize to the target language
   - **Catchphrases**: Preserve "Mottainai" as a recognizable Japanese loanword. Translate the rest naturally.
   - **Tone**: Preserve character voices — Akane is sarcastic/dry, Lt. Commander is dramatic, CIO is spineless, Katsuragi is blunt/aggressive
   - **Puns**: Adapt creatively for the target language. Refer to the "Notable Puns" section in the glossary.
   - **Do NOT** include original Japanese in the output
4. Write output as `<original_basename>.$ARGUMENTS.srt` (e.g., `kosys_ee_ep01.$ARGUMENTS.srt`)

## Step 3b: Assemble the compilation film

`kosys_ee_movie_eirin.srt` is a compilation film that reuses dialogue from ep01-ep06 with different timestamps. Translate it by reusing completed episode translations:

1. Read `kosys_ee_movie_eirin.srt` (Japanese source)
2. For each subtitle, find the matching Japanese line in the already-translated episode files and reuse its translation
3. Keep the movie's own timestamps
4. For lines unique to the movie (title cards like "数ヶ月後", transitions), translate fresh
5. Write output as `kosys_ee_movie_eirin.$ARGUMENTS.srt`

## Step 4: Review against glossary

Search all output files for known incorrect name variants and fix them:

```bash
grep -rn 'Keikawa\|Yuna[^m]\|Mannokura\|Manokura\|Tsukida\|Amarube' *.$ARGUMENTS.srt
```

Known correct forms (see GLOSSARY.md for full list):
- 敬川 = **Uyagawa** (NOT Keikawa)
- 垂水 結菜 = **Yuina** (NOT Yuna)
- 万能倉 = **Managura** (NOT Mannokura/Manokura)
- 月田 = **Tsukita** (NOT Tsukida)
- 余部 = **Yobe** (NOT Amarube)

Also verify:
- No untranslated Japanese text remains
- Organization names match glossary (e.g., "Zaō Kōsoku Electric Railway")

Fix all issues using `sed` or equivalent.

## Step 5: Report

List all output files with subtitle counts. Note any issues found and fixed.

## Reference

- Existing `*.en-US.srt` files serve as reference translations
- For languages where macrons (ō, ū) are impractical, use plain ASCII (o, u)
