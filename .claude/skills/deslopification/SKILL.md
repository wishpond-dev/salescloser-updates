---
name: deslopification
description: Strip AI-style writing tells from text. Kills em-dash habit, "not X, it's Y" antithesis, rule-of-three padding, LLM vocabulary, transition-word spam, and symmetrical sentence rhythm. Use when text reads like it was generated, or before any text ships as human-written.
---

# Deslopification

Remove the *stylistic* fingerprint of AI writing. This is about taste and syntax,
not hidden characters (that is `dewatermarkization`).

## Method

Run three passes. Do not try to do all of it in one edit.

1. **Pass 1 (structural)** - kill the constructions in Section A.
2. **Pass 2 (lexical)** - swap the vocabulary in Section B.
3. **Pass 3 (rhythm)** - break the symmetry in Section C.

Then run the checklist at the bottom. Any unchecked box means go back.

---

## A. Banned constructions

### A1. The em dash habit
LLMs use `—` as a universal joint. Humans mostly use commas, periods, or parens.

- Bad: `The launch failed — nobody told marketing.`
- Good: `The launch failed. Nobody told marketing.`
- Good: `The launch failed because nobody told marketing.`

Budget: **zero em dashes.** If a sentence truly needs an aside, use a comma pair or
parentheses. Also drop the en dash `–` outside numeric ranges.

### A2. Antithesis scaffolding
The single loudest tell. Two forms. Most people only catch the first one.

**A2a. Explicit, with a negation.**
- `It's not about X. It's about Y.`
- `This isn't just a tool, it's a movement.`
- `Less X, more Y.`
- `Not because X, but because Y.`
- `X? No. Y.`

**A2b. Bare antithesis, no negation.** Two adjacent clauses, parallel syntax,
abstract subjects, a contrast verb pair, and zero new information. This is the
form that survives a naive edit pass, because there is no `not` to grep for.

- `The reply speed gets the attention. The notes are what closes.`
- `X is the easy part. Y is the hard part.`
- `X gets the headlines. Y does the work.`
- `Speed is the cheap half. The expensive half is Y.`
- `Anyone can X. Few can Y.`
- `X is what you say. Y is what you do.`
- `X is cheap. Y is not.`
- `Everyone talks about X. Nobody talks about Y.`
- `No performance, no guarding the budget question.`
- `No meetings, no politics.`

That last pair is the **parallel negation fragment**: `No X, no Y.` It looks like a
punchy human fragment, which is why it survives editing. It is the same machine shape.
Two abstract nouns, negated in parallel, restating what the previous sentence already
implied. If deleting it loses no information, it was cadence.

**Signature to look for**, independent of wording:
1. two short clauses, adjacent, roughly equal length
2. subjects are abstract nouns (`speed`, `notes`, `culture`, `talent`)
3. the second clause reveals nothing the reader did not already infer
4. it could be swapped into any other post on any other topic

Test 4 is decisive. If the couplet would work verbatim under a post about
hiring, logistics, or parenting, it is not an insight, it is a shape.

Fix: state the second half only, as concrete fact. Delete the first half. If the
contrast genuinely matters, put the first half several sentences earlier as its
own claim and let the reader close the gap.

- Bad: `It's not a hiring problem, it's a management problem.`
- Good: `We blamed hiring for two quarters. The managers were the problem.`
- Bad: `The reply speed gets the attention. The notes are what closes.`
- Good: `Our reps started asking for the transcript before the call.`

Hard cap: **one** contrast construction per piece, in either form, and only if
nothing else works. It may **never** be the closing line. See A10.

### A3. Rule of three
LLMs pad to triads because triads feel complete.

- Bad: `faster, cleaner, and more maintainable`
- Good: `faster` (or `faster, and it stopped waking me up at 3am`)

Rule: lists get **one, two, or four** items. Three only when the three things are
genuinely three things (a real enumeration), never for cadence.

### A4. Fake pivot fragments
- `Here's the thing:`
- `The result?`
- `The bottom line?`
- `And that changed everything.`
- `Why does this matter?`
- `Let that sink in.`

Delete the pivot. Start the next sentence with its content.

### A5. Symmetrical parallelism
Bad: `We shipped fast. We shipped clean. We shipped on time.`
Anaphora three times reads generated. Keep at most two, or vary the verb.

### A6. The wrap-up paragraph
LLMs restate the argument at the end. Humans stop when they run out of things to say.
Delete any final paragraph that contains no new information. Kill `In conclusion`,
`Overall`, `Ultimately`, `At the end of the day`, `To sum up`, `Key takeaway`.

### A7. Hedge stacking
`arguably`, `it's worth noting that`, `that said`, `it's important to remember`,
`some would say`, `in many ways`, `to a certain extent`. Delete. Make the claim or cut it.

### A8. Over-scaffolded formatting
- No bolding of random noun phrases mid-sentence.
- No `Heading: Subtitle` colon headings.
- No emoji bullets in prose (LinkedIn is the one exception, handled elsewhere).
- No numbered "3 takeaways" list unless the reader asked for a list.

### A9. Assistant residue
`I hope this helps`, `Great question`, `Let's dive in`, `Let me break this down`,
`Feel free to`, `Certainly`, `As an AI`, `In today's fast-paced world`,
`In the ever-evolving landscape of`.

### A10. The aphoristic closer
The highest-frequency slop position in the whole piece, and the hardest to feel,
because a punchy ending *feels* like good writing.

Models are trained to land. So they manufacture a maxim: a short, balanced,
quotable couplet that restates the argument as folk wisdom. It reads like a
conclusion and contains nothing.

Symptoms of a manufactured closer:
- it is the shortest paragraph, and it is exactly two sentences
- both sentences are parallel (see A2b)
- it introduces no fact, name, number, or event
- delete it and the piece loses nothing but the sense of an ending
- it would fit under a different post entirely

Examples to kill on sight:
- `The reply speed gets the attention. The notes are what closes.`
- `Culture is not what you write on the wall. It is what you tolerate.`
- `Tools change. Fundamentals don't.`
- `That is the whole game.`
- `And that is what nobody tells you.`
- `Simple, but not easy.`

**Fix. Option 1 is the default, not the first of three equals.**

Read this before choosing: in practice, options 2 and 3 are what you reach for when
you cannot bring yourself to do option 1. Both were tried on the same comment and both
were rejected by the author, who then cut the closer entirely. If you find yourself
picking 2 or 3, that is usually the additive instinct wearing a fix.

Gate before keeping any closer: **name the fact it adds that the piece did not already
contain.** If you cannot say it in five words, cut.

1. **Cut it. Default.** End on the last concrete thing you said. A6
   already bans the restating final paragraph; a two-line maxim is that paragraph
   wearing a better outfit. Real writing stops when the writer runs out of
   material, and that is allowed to feel abrupt.
2. **Replace it with one more fact.** Something specific, observed, and small.
   The reader draws the moral themselves, which is the whole reason it lands.
   - `Our reps started asking for the transcript before the call. That took about a week.`
3. **Replace it with a flat human reaction.** No polish, no symmetry.
   - `Still not sure what to do with that.`
   - `Nobody on the team predicted that part.`

If you keep a closer, it must be **asymmetric**. Unequal sentence lengths, one
concrete noun minimum, and no verb pair in opposition.

**Legibility gate, and it outranks everything above.** The closer must land on the
first read, at skimming speed, with no re-reading and no guessing. Cutting slop out
of a closer tempts you to compress it into a riddle, which is worse than the maxim
you started with: a manufactured aphorism is at least instantly parseable.

Fail conditions:
- a pronoun (`that`, `it`, `this`) with no obvious antecedent
- more than one idea inside the final short sentence
- a number or duration whose subject is implied rather than stated
- anything a reader has to reverse-engineer

Bad: `Our reps started asking for the transcript before the call. That took about a week.`
(`that` = the reps changing habit? the build? unclear. Three ideas, four words.)

Good: `Our reps now read the transcript before they get on the call. Nobody told them to.`

If the closer needs a second read, cut it. Option 1 was always the best option.

### A11. Device repetition across pieces
A construction you avoid within a piece but reuse in every piece is still a
fingerprint. It is just a slower one. Three consecutive posts opening with a
number, or closing with a couplet, or built on the same reversal, read as one
generator.

Track devices the way `humanization` tracks defects. Keep
`.deslop-state.json` at the project root:

```json
{
  "pieces": [
    { "n": 1, "slug": "001-agentic-dev-flow", "devices": ["antithesis-A2b", "closer-couplet", "hook-deadpan-fact"] }
  ],
  "cooldown": { "antithesis-A2b": 3, "closer-couplet": 4 }
}
```

Rules:
- Read it before drafting. A device used in the last 3 pieces is unavailable.
- The **closer shape** is a device. Log it. Do not close two consecutive pieces
  the same way.
- The **hook formula** is a device. Log it.
- If the state file says the move you want is on cooldown, that is the signal to
  find a better one, not to override.

---

## B. Banned vocabulary

Replace on sight. Left column is the tell.

| Slop | Use instead |
|---|---|
| delve into | look at, dig into, read |
| leverage (verb) | use |
| utilize | use |
| robust | solid, or say what it survives |
| seamless / seamlessly | it just works, or delete |
| streamline | speed up, cut steps |
| landscape / realm / space / arena | market, industry, or the actual noun |
| tapestry / symphony / mosaic | delete the metaphor |
| testament to | proof of, or delete |
| underscore / highlight (fig.) | show |
| pivotal / crucial / vital / paramount | important, or say the consequence |
| navigate (fig.) | handle, deal with |
| foster | build, cause |
| harness | use |
| unlock / elevate | delete |
| embark on a journey | start |
| game-changer / paradigm shift | say what changed |
| myriad / plethora | lots, or a number |
| deep dive | look, review |
| holistic | whole, or delete |
| synergy | delete |
| resonate with | matter to |
| curated | picked, chosen |
| meticulous / meticulously | careful, or delete |
| nuanced | complicated, or say the nuance |
| transformative | say what it changed |
| empower | let, help |
| ensure | make sure |
| facilitate | help, run |
| commence | start |
| furthermore / moreover / additionally | and, also, or start a new sentence |
| however (sentence-initial, repeated) | but |
| indeed | delete |
| truly / genuinely / really | delete |
| a wide range of | delete or give a number |
| when it comes to | for, in, about |
| plays a key role in | delete, name the role |
| serves as | is |

Also: no `firstly/secondly/lastly`. No `In essence`. No `That being said`.

---

## C. Rhythm fixes

AI prose has low variance. Every sentence lands in the 15 to 25 word band, every
paragraph is 3 to 4 sentences.

Minimum fixes:
- Insert at least one sentence under 5 words.
- Insert at least one sentence over 30 words, with a real subordinate clause.
- Make at least one paragraph a single line.
- No two consecutive sentences may open with the same part of speech.
- No two consecutive paragraphs may have the same sentence count.

Full treatment lives in the `prose` skill. Do the minimum here; run `prose` for depth.

---

## D. Detection commands

```bash
# em/en dashes, ellipsis char, curly quotes
grep -nP '[\x{2014}\x{2013}\x{2026}\x{2018}\x{2019}\x{201C}\x{201D}]' FILE

# antithesis, explicit form (A2a)
grep -nEi "it'?s not (just )?(about )?|isn'?t (just )?a|not (only|just) .*(but|it'?s)" FILE

# antithesis, bare form (A2b) - contrast verb pairs with no negation
grep -nEi "(easy|hard|cheap|expensive|simple|difficult|fun|boring) (part|half|bit)|gets the (attention|headlines|credit|praise)|does the (work|damage|closing)|is what (closes|matters|counts|wins|sells)|anyone can .*\. (few|nobody|not everyone)|everyone talks about .*\. (nobody|few)" FILE

# parallel negation fragment (A2b) - "No X, no Y."
grep -nEi '(^|[.!?] )no [a-z]+( [a-z]+)?, no [a-z]+' FILE

# invented-number smell: bare small integers modifying a noun. Verify each is real.
grep -nEiw '(two|three|four|five|2|3|4|5) [a-z]+' FILE

# aphoristic closer (A10) - inspect the last 2 sentences by hand, every time
tail -c 400 FILE | tr '\n' ' ' | sed 's/\([.!?]\)  */\1\n/g' | tail -3
# then ask: any new fact? asymmetric lengths? would it fit under a different post?

# slop vocabulary
grep -nEiw 'delve|leverage|utilize|robust|seamless|streamline|landscape|realm|tapestry|testament|underscore|pivotal|crucial|vital|foster|harness|unlock|elevate|embark|myriad|plethora|holistic|synergy|resonate|curated|meticulous|nuanced|transformative|empower|ensure|facilitate|furthermore|moreover|additionally|ultimately|indeed' FILE

# pivot fragments and wrap-ups
grep -nEi "here'?s the thing|the result\?|bottom line|let that sink in|in conclusion|at the end of the day|key takeaway" FILE
```

---

## Checklist

- [ ] Zero em dashes and en dashes
- [ ] Zero "not X, it's Y" constructions (A2a) (or exactly one, justified)
- [ ] Zero bare antithesis couplets (A2b), counted against the same cap
- [ ] The contrast beat, if any, is not the closing line
- [ ] Closer passes A10: introduces a fact, or was cut. Not a two-line maxim
- [ ] Closer is asymmetric (unequal sentence lengths, no opposed verb pair)
- [ ] Swap test on the closer: it would NOT fit under a different post
- [ ] Devices logged in `.deslop-state.json`, none reused from the last 3 pieces
- [ ] Zero three-item cadence lists
- [ ] Zero pivot fragments
- [ ] No restating final paragraph
- [ ] Zero hedge stacks
- [ ] Slop grep returns nothing
- [ ] One sentence under 5 words exists
- [ ] One sentence over 30 words exists
- [ ] At least one single-line paragraph
- [ ] No two adjacent sentences share an opening structure

## Anti-goal

Do not overcorrect into choppy telegram prose. The target is a specific human who
writes unevenly, not a person with a word-count phobia. Sloppy-but-alive beats
polished-and-generated, but incoherent beats nothing.
