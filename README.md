# Word Wall

A vocabulary learning app that feels like popping bubble wrap — not grinding through flashcards.

**Live demo → [demianman.github.io/word-wall](https://demianman.github.io/word-wall/)**

---

## The idea

Most vocab apps present words one at a time, like a queue you have to process. That works, but it requires willpower to start. The goal here was a lower activation energy: you open the page, the whole wall is already there, and you just start popping words whenever one catches your eye.

The interaction mirrors the satisfying feel of popping bubble wrap — the whole sheet is visible at once, you can work in any order, and each pop gives a small hit of completion. When the wall clears, a fresh one loads.

---

## What's in it

**Word cloud layout**
Words are placed using an Archimedean spiral algorithm that starts at the center and works outward, with collision detection to prevent overlap. Six font-size tiers (13–62px) distributed across ~42 words per wall. Random color from a 10-color palette, slight random rotation per word.

**Per-word popup**
Click any word to expand a card showing:
- IPA phonetic transcription (American English)
- Part of speech
- Chinese definition
- Example sentence with the word highlighted

**Audio pronunciation**
Uses the Web Speech API with voice priority: Samantha → Ava → Alex → any `en-US` local voice. Auto-plays when a card opens; replay button in the card.

**Pop mechanic**
- Click the card again, or press `Space` → word bursts into particles and disappears
- `Esc` → close card without popping
- Progress bar fills as you clear the wall
- When all words are cleared, a new wall randomizes from the pool

**Custom word import**
Top-right "导入词书" button accepts four formats, auto-detected:

| Format | Structure |
|--------|-----------|
| `.txt` | One word per line |
| `.csv` | Header row: `word, meaning, phonetic, pos, example` |
| `.tsv` | Tab-separated — direct Anki export compatible |
| `.json` | `[{ "word": "", "meaning": "", "phonetic": "", "pos": "", "example": "" }]` |

A template CSV is available via the "下载模板 CSV" button in the import dialog. Imported word lists persist in `localStorage` across sessions. Switch back to the built-in list anytime with "恢复默认词书".

**Built-in word list**
~110 high-frequency everyday English words curated for active vocabulary (words you'd use in conversation and writing, not GRE lists). Each entry includes a manually written IPA transcription and a contextual example sentence.

---

## Tech

Single self-contained HTML file. No dependencies, no build step, no backend. Works offline after first load.

- Layout: absolute positioning + JS spiral placement with AABB collision detection
- Audio: Web Speech API (`SpeechSynthesisUtterance`)
- Persistence: `localStorage`
- Hosting: GitHub Pages

---

## Run locally

Just open `index.html` in a browser. No server needed.

```bash
open index.html   # macOS
```

---

## Import format reference

Minimal CSV (meaning only):
```
word,meaning
ephemeral,短暂的；转瞬即逝的
tenacious,坚韧的；不屈不挠的
```

Full CSV:
```
word,meaning,phonetic,pos,example
serendipity,意外发现美好事物的能力,/ˌsɛrənˈdɪpɪti/,n,Finding that coffee shop was pure serendipity.
```

Anki TSV export (Basic note type) works directly — the first field becomes the word, the second becomes the meaning.
