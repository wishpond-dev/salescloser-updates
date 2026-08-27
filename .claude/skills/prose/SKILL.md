---
name: prose
description: Make writing dynamic by forcing variance in sentence length, paragraph size, verb tense, voice, person, sentence openers, and rhythm. Includes measurable targets, an opener taxonomy, and a scoring pass. Use on any draft that reads flat, evenly-paced, or monotonous.
---

# Prose

Flat writing is not usually a vocabulary problem. It is a **variance** problem.
Machine prose converges: every sentence 18 words, every paragraph 3 sentences,
every clause active, every opener a subject.

This skill measures the variance and fixes it.

---

## 1. Measure first

Before editing, count. Do not eyeball it.

```bash
# sentence word counts, one per line
tr '\n' ' ' < FILE | sed 's/\([.!?]\)  */\1\n/g' | awk 'NF{print NF, $0}' | cut -d' ' -f1

# sentence count and length stats
tr '\n' ' ' < FILE | sed 's/\([.!?]\)  */\1\n/g' | awk 'NF{n++;w=NF;s+=w;ss+=w*w;if(w<min||!min)min=w;if(w>max)max=w}
END{m=s/n;printf "sentences=%d mean=%.1f sd=%.1f min=%d max=%d\n",n,m,sqrt(ss/n-m*m),min,max}'

# paragraph line counts
awk 'BEGIN{RS=""} {print NR": "gsub(/[.!?]/,"&")" sentences, "length($0)" chars"}' FILE

# first word of every sentence, to spot opener repetition
tr '\n' ' ' < FILE | sed 's/\([.!?]\)  */\1\n/g' | awk 'NF{print $1}' | sort | uniq -c | sort -rn | head
```

### Targets

| Metric | Bad (machine) | Target |
|---|---|---|
| Sentence length mean | 18 to 22 | 12 to 17 |
| Sentence length sd | under 5 | **8 or more** |
| Shortest sentence | 8+ words | 1 to 4 words |
| Longest sentence | under 28 | 30 to 45 words, once |
| Sentences per paragraph | all 3 or 4 | mixed 1 to 5 |
| Same opener word repeats | 3+ times | at most 2 |
| Passive constructions | 0, or over 30% | 5 to 15% |

Standard deviation is the single most useful number. If sd is under 6, the text
is machine-flat regardless of how good the words are.

---

## 2. Sentence length

Write in **bursts**, not a steady beat. Good rhythm patterns:

- `long, short, medium, short` (drives forward)
- `short, short, long` (setup, setup, payoff)
- `medium, long, very short` (the very short one lands the point)

Never `medium, medium, medium`.

A one-word sentence is legal. Use one per piece, maximum two.

```
The migration ran overnight and by the time anyone looked at the dashboard on
Monday morning the replica had been silently dropping writes for eleven hours,
which nobody noticed because the alert routed to a Slack channel that had been
archived in March. Eleven hours. We rebuilt from the WAL.
```

45 words, then 2, then 5. That is the shape.

---

## 3. Paragraph size

Vary deliberately:
- **1 line** - lands a beat, creates a pause
- **2 to 3 lines** - the workhorse
- **4 to 6 lines** - only when developing one continuous argument
- **7+ lines** - split it

Two adjacent paragraphs must not have the same sentence count. If paragraphs 3 and 4
both have three sentences, move a sentence.

---

## 4. Verb tense

One tense throughout is the flattest possible choice. Mix by function:

| Tense | Use for |
|---|---|
| Simple past | narrating what happened (`We shipped it Friday.`) |
| Present | stating what is true now, insight, principle (`Approval is not validation.`) |
| Present perfect | connecting past to now (`I have never seen a rollback go that badly.`) |
| Past continuous | setting a scene (`We were still arguing about the schema.`) |
| Future / conditional | stakes, hypothetical (`If it happens again we lose the account.`) |
| Present as narrative | dropped into a past account for immediacy (`So there I am, 2am, no backup.`) |

A good short piece uses at least three. Shift at paragraph boundaries, not mid-sentence,
unless the shift is the effect.

---

## 5. Voice

Active by default. But **zero** passive is its own tell, and passive is correct when:
- the actor is unknown or irrelevant: `The config was changed in June.`
- you are deliberately withholding blame: `Mistakes were made.`
- the object is the topic: `The service was rebuilt three times.`

Target 5 to 15% passive. One or two per short piece.

---

## 6. Person

Shifting person creates movement. A common effective arc:

`I` (my specific experience) -> `you` (the reader's situation) -> `we` (shared condition)

Do not shift randomly. Pick the arc and let it progress. Avoid the impersonal
`one` entirely.

---

## 7. Sentence openers

The biggest single source of monotony: every sentence opening with its subject.

### Opener taxonomy, rotate through these

| Type | Example |
|---|---|
| Subject | `The team missed the deadline.` |
| Prepositional phrase | `By Thursday, nobody was pretending.` |
| Subordinate clause | `Because the alert was muted, we found out from a customer.` |
| Participial phrase | `Staring at the dashboard, I understood.` |
| Infinitive | `To be fair, the spec was ambiguous.` |
| Adverb | `Predictably, it broke in production.` |
| Coordinating conjunction | `But nobody asked the support team.` |
| Fragment | `Eleven hours.` |
| Question | `Who approved it? Everyone.` |
| Dialogue | `"Ship it," he said.` |
| There / It expletive | `There was no rollback plan.` (use once at most) |

Rule: no two consecutive sentences may use the same opener type. No opener type may
appear three times in a row anywhere in the piece.

---

## 8. Concreteness

Dynamic prose is specific prose. Abstraction flattens rhythm because abstract nouns
have no length variety.

- Replace category nouns with instances: `stakeholders` -> `Marcus from finance`
- Replace magnitudes with numbers: `a lot of users` -> `1,400 users`
- Replace evaluations with observations: `it went badly` -> `four people clicked it`
- Delete `very`, `quite`, `somewhat`, `fairly`, `extremely`
- Cut `-ly` adverbs where the verb can carry it: `walked quickly` -> `hurried`

One metaphor per piece, maximum, and keep it inside one domain. Mixed metaphors
(`the runway for this pipeline is a minefield`) read as generated.

### 8.1. Never invent a number

The push for numbers above applies **only to numbers you actually have.** A fabricated
specific is worse than an honest vague, and worse than the abstraction it replaced,
because it is now a false claim that somebody can check.

- Bad: `three form fields` when you do not know how many fields there are
- Good: `form fields`

Watch for this exact failure mode: you are told to be concrete, no real number is
available, and a plausible small integer appears. Three, four, and 40% are the usual
suspects. If you cannot source it, use the plain noun.

Rules:
- Never insert a number, percentage, duration, or headcount that was not supplied.
- Never sharpen a supplied vague quantity (`a couple of weeks` stays vague, or becomes
  the range the author gave you).
- If the sentence badly needs a number, leave a marker and ask: `[X]% of ...`.
- This binds hardest when writing as an employee about their own employer. A made-up
  metric in that position is a liability, not a rhetorical flourish.

### 8.2. Intensifiers about people are exempt

The `very / quite / extremely` ban is about padding abstractions. It does not apply to
warmth directed at named humans or a specific team.

- Padding, delete: `a very robust process`, `an extremely important insight`
- Voice, keep: `our awesome account executives`, `my incredible team`, `she is great`

This reads as cringe when measured against style rules and as sincere when read by a
person. Sincere wins. Do not optimize it away.

Related: use the **full job title**, not the insider shorthand. `account executives`
not `reps`, `engineers` not `devs`. Shortening a job title in public reads as
dismissive of the person holding it.

### 8.3. Final position carries the weight

The last noun before a full stop is what the reader is holding when they stop reading.
Put the concrete thing there and let the abstraction sit mid-sentence.

- Weak: `...three form fields and a guess about what the lead meant.`
- Strong: `...instead of trying to guess what a lead meant from form fields.`

Identical content, reordered. Apply this to the last sentence of every paragraph, and
twice as hard to the last sentence of the piece.

---

## 9. Editing procedure

1. Run the measurement commands. Write down sd, min, max, opener histogram.
2. **Fix the extremes first.** Find the longest sentence, make it longer by merging
   the next one. Find a mid-length sentence, cut it to 3 words. This raises sd fastest.
3. Walk the opener list. Rewrite every third or fourth sentence to use a different
   opener type.
4. Check paragraph sentence counts. Move one sentence across a boundary to break ties.
5. Do a tense pass. Ensure at least three tenses appear and each is doing a job.
6. Do a concreteness pass. Every abstract noun gets a challenge: name an instance or
   delete. No invented numbers (8.1). Concrete noun in final position (8.3).
7. Re-measure. If sd is still under 8, go back to step 2.
8. **Subtractive pass. Do not skip this one.** Delete the final sentence and read the
   piece again. Then delete the final paragraph and read it again. Keep the shortest
   version that is not actually worse.

   This exists because every additive instinct survives the earlier steps. Steps 2
   through 7 all *add* something: a longer sentence, a different opener, a specific
   detail. Nothing above removes material, so the draft only grows.

   Expect to cut 20 to 40% on this pass. If you cut nothing, you did not do it. A draft
   that got shorter three rounds in a row is normal and is not a sign the earlier
   rounds were wasted.

   Under ~80 words, stop enforcing the variance targets in Section 1. Two sentences
   cannot have a standard deviation of 8. Short and clear beats padded to hit a metric.

---

## 10. Before and after

**Before** (sd = 3.1, all subject openers, all simple past, all paragraphs 3 sentences):

```
We decided to migrate the database over the weekend to minimize disruption.
The team prepared a detailed runbook and tested the process in staging twice.
Everyone felt confident about the plan going into Friday evening.

The migration started at 10pm and appeared to complete without any errors.
We monitored the dashboards for two hours and saw normal traffic patterns.
The team went home believing the operation had been a complete success.
```

**After** (sd = 12.4, six opener types, four tenses, paragraphs of 1, 3, and 2):

```
We moved the database on a Saturday.

The runbook was 14 pages. We rehearsed it twice in staging, and by Friday evening
everyone had that specific kind of confidence you only get from a document nobody
has read end to end. Marcus brought pizza.

It started at 10pm. Green across the board, normal traffic, no errors, so we watched
the dashboards for two hours and then went home.

The replica had been dropping writes since 10:04.
```

Changes: opened with a 6-word sentence, added a 39-word sentence, added a 3-word
concrete beat (`Marcus brought pizza.`), used a passive (`The replica had been
dropping writes`), added past perfect and a fragment-ish opener, broke paragraph
uniformity, replaced `the team` with a name.

---

## 11. Self-check

- [ ] sd of sentence length is 8 or higher
- [ ] shortest sentence is under 5 words
- [ ] longest sentence is 30+ words
- [ ] at least 5 different opener types used
- [ ] no opener type appears 3 times consecutively
- [ ] no two adjacent paragraphs have the same sentence count
- [ ] at least 3 verb tenses, each doing a job
- [ ] 1 to 2 deliberate passives
- [ ] person arc is intentional
- [ ] at least 3 concrete specifics (name, number, object)
- [ ] every number in the piece is real, none invented (8.1)
- [ ] concrete noun in final position of the last sentence (8.3)
- [ ] subtractive pass run, something was actually cut (step 8)
- [ ] full job titles, no insider shorthand (8.2)
- [ ] at most 1 metaphor, domain-consistent
- [ ] zero `very` / `quite` / `somewhat`, except warmth about people (8.2)

## Order

Run `prose` after `deslopification` and before `linkedin-voice`. It is also the
main defense against statistical watermarks, since rewriting for rhythm changes
token choices at scale. See `dewatermarkization` Section 3.
