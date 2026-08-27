---
name: dewatermarkization
description: Remove machine-detectable watermarks from generated text. Strips zero-width and invisible Unicode, variation selectors, bidi marks, smart typography, and non-ASCII whitespace; explains what character stripping cannot fix (token-distribution watermarks like SynthID) and how to defeat that by rewriting. Use before publishing or pasting generated text anywhere.
---

# Dewatermarkization

Two different things get called "watermarks." They need different fixes.

| Kind | What it is | Fix |
|---|---|---|
| **Character watermarks** | Invisible or unusual codepoints inserted in the text | Strip them (Section 1) |
| **Statistical watermarks** | Biased token sampling, e.g. SynthID-Text; the *word choices themselves* carry the signal | Cannot be stripped. Must rewrite (Section 3) |

Do both. Section 1 is mechanical. Section 3 is the one people skip.

---

## 1. Character watermarks

### 1.1 Invisible codepoints (always remove)

| Codepoint | Name |
|---|---|
| U+200B | zero width space |
| U+200C | zero width non-joiner |
| U+200D | zero width joiner |
| U+2060 | word joiner |
| U+FEFF | zero width no-break space / BOM |
| U+00AD | soft hyphen |
| U+180E | Mongolian vowel separator |
| U+061C | Arabic letter mark |
| U+200E U+200F | LTR / RTL mark |
| U+202A-U+202E | bidi embedding and override |
| U+2066-U+2069 | bidi isolates |
| U+FE00-U+FE0F | variation selectors |
| U+E0100-U+E01EF | variation selectors supplement |
| U+E0000-U+E007F | tag characters (used to smuggle whole hidden strings) |
| U+2800 | braille blank |
| U+3164 | Hangul filler |
| U+115F U+1160 | Hangul choseong/jungseong filler |

### 1.2 Suspicious whitespace (normalize to plain space)

| Codepoint | Name |
|---|---|
| U+00A0 | no-break space |
| U+2000-U+200A | en/em/thin/hair spaces |
| U+202F | narrow no-break space |
| U+205F | medium mathematical space |
| U+3000 | ideographic space |
| U+1680 | Ogham space mark |

### 1.3 Smart typography (fold to ASCII)

These are not invisible, but they are a strong machine and human tell because most
people typing in a normal input box produce ASCII.

| From | To |
|---|---|
| `—` U+2014, `―` U+2015 | `. ` or `, ` (rewrite, do not just insert `-`) |
| `–` U+2013 | `-` |
| `’` U+2019, `‘` U+2018 | `'` |
| `“` `”` U+201C/D | `"` |
| `…` U+2026 | `...` |
| `′` `″` | `'` `"` |
| `×` | `x` |
| `→` `⇒` | `->` |
| `•` U+2022 | `-` |
| `™ © ®` | delete unless legally needed |
| ligatures `ﬁ ﬂ` | `fi` `fl` |
| `½ ¼ ¾` | `1/2` `1/4` `3/4` |
| non-breaking hyphen U+2011 | `-` |

Note: an em dash is both a watermark-ish artifact and a style tell. See `deslopification` A1 for the rewrite, not just the substitution.

### 1.4 Detect

```bash
# any non-ASCII, with line numbers and the offending bytes
grep -nP '[^\x00-\x7F]' FILE

# name every non-ASCII codepoint found
perl -CSD -ne 'while(/([^\x00-\x7F])/g){printf "line %d: U+%04X %s\n",$.,ord($1),$1}' FILE

# invisible-only scan (the ones you will never see by eye)
perl -CSD -ne 'print "line $.: U+".sprintf("%04X",ord($1))."\n" while /([\x{200B}-\x{200F}\x{202A}-\x{202E}\x{2060}-\x{2064}\x{2066}-\x{2069}\x{FEFF}\x{00AD}\x{180E}\x{061C}\x{FE00}-\x{FE0F}])/g' FILE

# count, for a quick pass/fail
grep -cP '[^\x00-\x7F]' FILE
```

### 1.5 Strip

```bash
perl -CSD -i -pe '
  s/[\x{200B}-\x{200F}\x{202A}-\x{202E}\x{2060}-\x{2064}\x{2066}-\x{2069}\x{FEFF}\x{00AD}\x{180E}\x{061C}\x{2800}\x{3164}\x{115F}\x{1160}]//g;
  s/[\x{FE00}-\x{FE0F}]//g;
  s/[\x{E0000}-\x{E01EF}]//g;
  s/[\x{00A0}\x{2000}-\x{200A}\x{202F}\x{205F}\x{3000}\x{1680}]/ /g;
  s/[\x{2018}\x{2019}\x{201B}\x{2032}]/'"'"'/g;
  s/[\x{201C}\x{201D}\x{201F}\x{2033}]/"/g;
  s/\x{2026}/.../g;
  s/\x{2013}/-/g;
  s/\x{2011}/-/g;
  s/\x{2022}/-/g;
  s/\x{00D7}/x/g;
  s/[\x{2192}\x{21D2}]/->/g;
' FILE
```

Em dashes are deliberately **not** in that script. Auto-replacing `—` with `-`
produces a different tell. Fix each one by hand or via `deslopification`.

### 1.6 Other mechanical tells

- **Double space after a period.** Generated text is usually perfectly single-spaced,
  or perfectly double. Real typing is inconsistent. Normalize to single, then let
  `humanization` reintroduce an inconsistency or two.
- **Trailing whitespace** at line ends, and a trailing newline at EOF.
- **Markdown residue** in a plain-text destination: `**bold**`, `- ` bullets, `###`
  headings pasted into LinkedIn render literally. Strip or convert.
- **HTML clipboard payload.** Copying from a chat UI carries `<span data-*>`,
  `class="..."`, and sometimes a hidden citation span. Paste through a plain-text
  step (`pbpaste | pbcopy` on macOS) or retype.
- **Metadata.** If shipping a file: `exiftool -all= FILE` for images, and check
  document properties for author/producer strings.

---

## 2. Verification

```bash
# must return 0
grep -cP '[^\x00-\x7F]' FILE
# must return nothing
grep -nP '[ \t]+$' FILE
grep -nP '\.  +[A-Z]' FILE
```

If you need real non-ASCII (Spanish accents, names, a legitimate `€`), whitelist
those specific codepoints and re-run the scan so the count is *expected*, not zero.

---

## 3. Statistical watermarks (the part stripping does not fix)

SynthID-Text and similar schemes bias the sampler so the *sequence of chosen tokens*
carries a signal. There are no special characters. Deleting whitespace does nothing.
The watermark lives in which synonym got picked, in what order clauses landed, in
how long each sentence ran.

Detection strength decays with edit distance. So the defense is edits that change
token choices, not edits that change bytes.

What actually degrades it, in rough order of effect:

1. **Rewrite sentence by sentence in your own words.** Do not paraphrase mechanically.
   Change subject, change verb, change clause order. Aim to keep the meaning and
   replace most content words.
2. **Merge and split sentences.** Two short into one long, one long into three.
   Sentence-boundary changes disrupt long-range signal.
3. **Reorder paragraphs** where argument order permits.
4. **Change person and tense.** Third to first, past to present.
5. **Delete 15 to 25% of the words.** Compression forces new token paths and usually
   improves the writing anyway.
6. **Add material that was never generated:** a specific number, a name, a date, a
   real anecdote. Human-authored spans have no watermark at all, and they dilute the
   per-token evidence the detector needs.
7. **Introduce real typos and irregular grammar.** See `humanization`. Off-distribution
   tokens are the strongest local disruption.

What does **not** work: swapping a few adjectives, adding hashtags, changing
capitalization, running it through another model with "make this undetectable"
(you are just re-watermarking with a different key), or the invisible-character
strip in Section 1.

Rule of thumb: if you could not honestly say "I wrote this sentence," it is still
carrying whatever signal it came with.

---

## Order of operations

1. Strip characters (Section 1).
2. Deslop the style (`deslopification`).
3. Rewrite for prose dynamics (`prose`) - this is also the statistical-watermark defense.
4. Add human imperfection (`humanization`).
5. Re-run Section 2 verification last, because the later steps can reintroduce
   smart quotes if any editor autocorrects.

## Scope

This is for making your own writing pass as your own writing and for stripping
tracking characters out of text you are about to publish. It is not for
misrepresenting authorship where authorship is contractually or legally required
(academic submissions, disclosures, court filings). If that is the use, stop.
