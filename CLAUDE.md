# Kōshisu! (こうしす！) Subtitle Project

This repository contains Japanese subtitle files (`.srt`) for the anime "Kōshisu!" (こうしす！), an IT security education anime set at the fictional Kyōki Railway (京姫鉄道).

## Repository Structure

```
*.srt                    # Japanese source subtitles
*.en-US.srt              # English reference translations
GLOSSARY.md              # Standardized terms, character names, and translation guidelines
references/scripts/      # Original episode scripts (who speaks each line)
references/characters/   # Character profile sheets
.claude/skills/          # Project skills (invoke with /skill-name)
```

## Key Files

- **GLOSSARY.md** — The single source of truth for all translations. Contains character names (with correct romanization), organization names, railway/IT terms, catchphrases, and pun adaptation notes. Always consult this before translating or reviewing.
- **kosys_ee_movie_eirin_ja-JP.srt** — Compilation film (映倫版). Reuses dialogue from ep01-ep06 with different timestamps. Translated last by matching lines against completed episode translations, ensuring consistency. Lines unique to the movie are translated fresh.

## Translation Workflow

Use `/translate-subtitles <language-code>` to translate all episodes into a new language.

Example:
```
/translate-subtitles zh-Hans
/translate-subtitles ko
/translate-subtitles fr
```

This skill handles the full pipeline: source identification, parallel translation with glossary compliance, review, and batch correction.

## Common Pitfalls

Character names that are frequently misread:

| Character | Correct | Common mistakes |
|---|---|---|
| 敬川 康 | **Uyagawa** Yasu | Keikawa, Keigawa |
| 垂水 結菜 | Tarumi **Yuina** | Yuna, Yūna |
| 万能倉 まな | **Managura** Mana | Mannokura, Manokura, Manōkura |
| 月田 万作 | **Tsukita** Bansaku | Tsukida |
| 余部 静夫 | **Yobe** Shizuo | Amarube |

## Editing Guidelines

- Always keep SRT format intact (numbering, timestamps, blank line separators)
- After any batch edit, run a grep to confirm no old variants remain
- When adding a new character or term, update `GLOSSARY.md` first, then apply to all translations
