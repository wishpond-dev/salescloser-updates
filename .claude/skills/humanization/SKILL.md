---
name: humanization
description: Inject controlled imperfection into text so it reads as typed by a person, using a rotating pool of typo, grammar, punctuation, and self-interruption defects with a density budget, placement rules, and a state file so the same trick never repeats across posts. Use as the final pass before publishing.
---

# Humanization

Perfect text is the tell. Real people typing into a web box produce a small, uneven
number of defects. The goal is a plausible defect *profile*, not a sprinkle of typos.

Two failure modes to avoid:
- **Too clean.** Zero defects across 400 words of casual writing is machine-made.
- **Always the same trick.** If every post has exactly one missing apostrophe, the
  pattern itself becomes the fingerprint. Rotation is mandatory, not optional.

---

## 1. Density budget

| Text length | Defects |
|---|---|
| under 80 words | 0 or 1 |
| 80 to 200 words | 1 or 2 |
| 200 to 400 words | 2 or 3 |
| 400 to 700 words | 3 to 5 |
| over 700 words | roughly 1 per 150 words, capped at 7 |

Vary within the range across posts. A run of posts all sitting at exactly 2 defects
per 200 words is its own pattern. Some posts should have zero. That is realistic.

Never place two defects in adjacent sentences. Never two of the same *type* in one piece.

---

## 2. Defect pool

Pick from different families each time. Families matter more than individual items.

### Family T: typos
- `T1` adjacent-key slip: `waht` (t/h), `teh`, `nad`, `jsut`, `abotu`, `porblem`
- `T2` doubled letter: `reallly`, `commmit`, `finallly`
- `T3` dropped letter: `becuse`, `managment`, `diferent`, `recomend`
- `T4` transposition: `recieve`, `beleive`, `wierd`, `seperate`
- `T5` fat-finger space: `the re`, `some thing`, `any way`
- `T6` missing space after comma: `first,second`
- `T7` doubled word across a line break: `the the`, `to to` (use rarely, once per 10 posts)

### Family P: punctuation
- `P1` missing apostrophe: `dont`, `wont`, `im`, `thats`, `youre`, `cant`
- `P2` missing terminal period on the last line only
- `P3` comma splice: `We shipped it Friday, nobody noticed.`
- `P4` extra comma: `And the thing is, that nobody checked.`
- `P5` double space between two words, once
- `P6` lowercase sentence start after a line break
- `P7` unclosed parenthesis in an aside (once per 15 posts, it looks careless not fake)

### Family G: grammar
- `G1` its / it's swap
- `G2` your / you're swap (risky, some readers judge hard, use sparingly)
- `G3` then / than swap
- `G4` subject-verb slip in a long clause: `The list of things that broke were long.`
- `G5` sentence starting with `And` or `But`
- `G6` dangling `which` clause
- `G7` number style drift: `3 people` early, `three people` later in the same piece

### Family S: speech artifacts
- `S1` self-interrupt and correct in place: `Two years. Three, actually.`
- `S2` trailing afterthought: `anyway.` / `lol` / `so yeah` / `idk`
- `S3` filler intensifier where the rest of the text has none: `pretty much`, `kind of`
- `S4` mid-sentence restart: `The thing is, I mean, nobody asked.`
- `S5` orphan fragment as its own paragraph: `Still annoyed about it.`
- `S6` parenthetical aside in a different register: `(and yes I checked twice)`
- `S7` abbreviation drift: `Q4` early, `fourth quarter` later

### Family F: formatting
- `F1` inconsistent list punctuation: some bullets end with a period, some do not
- `F2` a single line that runs a bit too long compared to its neighbors
- `F3` uneven blank lines: one double gap where the rest are single
- `F4` inconsistent capitalization of the same term: `Product Manager` then `product manager`

---

## 3. Placement rules

**Never place a defect in:**
- the first sentence (the hook carries the whole post)
- a person's name, a company name, or a product name
- a number that carries the argument (revenue, headcount, dates)
- the call to action or the closing question
- a quoted line attributed to someone else
- anything that changes meaning or creates real ambiguity

**Prefer placing defects in:**
- the middle third of the text
- long sentences (a typo in a 4-word sentence is conspicuous)
- low-stakes descriptive clauses
- the second half of a bullet, not the first word

**Legibility gate:** a reader skimming at speed must not stumble. If a defect makes
them re-read, it is too loud. Remove it and pick another.

---

## 4. Rotation

Keep a state file at the project root: `.humanization-state.json`

```json
{
  "posts": [
    { "n": 1, "families": ["P", "S"], "items": ["P1", "S2"], "count": 2 },
    { "n": 2, "families": ["T"], "items": ["T3"], "count": 1 },
    { "n": 3, "families": [], "items": [], "count": 0 }
  ],
  "cooldown": { "P1": 2, "S2": 2, "T3": 1 }
}
```

Rules:
1. Read the file before choosing defects. Create it on first use.
2. An **item** used in the last 4 posts is unavailable.
3. A **family** used in the last 2 posts is deprioritized. Do not use the same family
   twice in a row unless the pool is exhausted.
4. Every 5th or 6th post gets **zero** defects. Pick which one unpredictably, not on
   a fixed cycle.
5. Vary the count, not just the type. Do not always sit mid-range.
6. Append the post's record and decrement cooldowns after each run.

If no state file is permitted, at minimum log the chosen items in the output so the
user can track them manually.

---

## 5. Voice consistency

Defects have to belong to one person. Decide once and keep it:

- Does this writer drop apostrophes? If yes, they drop them *often*, not once.
  Pick a habit and be consistent inside a single post.
- Register: casual-lowercase, or professional-with-slips? A post with `Furthermore`
  in paragraph 2 and `idk` in paragraph 3 reads as two authors, unless the whiplash
  is the joke.
- Non-native-speaker profile is a valid, coherent choice: article slips, preposition
  choices (`in the weekend`), literal idiom translation. If you pick it, apply it as a
  *system* across the whole piece, not as one random error.

---

## 6. Worked example

Before, 2 defects, families P and S, count 2:

```
I spent three weeks building a dashboard nobody opened. The data was correct.
The charts were clean. My manager approved the spec himself. On launch day,
four people clicked it, and two of those were me. I deleted it in November.
```

After:

```
I spent three weeks building a dashboard nobody opened. The data was correct.
The charts were clean. My manager approved the spec himself, on launch day four
people clicked it and two of those were me.

I deleted it in November. anyway
```

Applied: `P3` comma splice, `S2` trailing afterthought with `P6` lowercase start.
Hook untouched. Numbers untouched. Nothing became ambiguous.

---

## 7. Self-check

- [ ] Defect count matches the length budget, and is not the same count as last post
- [ ] Every family used differs from the previous post's families
- [ ] No item on cooldown was reused
- [ ] Hook, names, key numbers, and CTA are clean
- [ ] No two defects in adjacent sentences
- [ ] No defect creates real ambiguity
- [ ] The defects sound like one person
- [ ] State file updated

## Order

Run this **last**, after `deslopification`, `prose`, and `linkedin-voice`. Earlier
passes rewrite sentences and would delete or relocate the defects.

Exception: `dewatermarkization` character verification runs after this, because
an editor may autocorrect a typo into a smart quote.
