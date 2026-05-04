# Interview Transcript Feedback Engine — Classifier Notes

> This file is the source of truth for how `classifyQuestionLevel()` works.  
> Read this before touching the classifier. Read this before disputing a classification with a student.

---

## The Framework (Our Terms)

The classifier measures one thing: **what posture does the question ask the respondent to take?**

| Level | Name | The Ask | Respondent Is... |
|-------|------|---------|-----------------|
| L1 | Reporting | "What happened? What did you feel then?" | A witness reporting facts |
| L2 | Analyzing | "How did it work? Why did you do that?" | A character inside the story making sense of it |
| L3 | Meaning-Making | "Looking back, what does this say about who you are?" | A narrator outside the story evaluating its meaning |

The key distinction is **temporal position**:
- L1 and L2 both live *inside* the story's timeline
- L3 requires a **temporal shift** — the respondent must step outside the timeline and look back from the present

---

## L1 — Reporting (The "What")

The respondent retrieves a static fact, snapshot, or emotional data point. There is essentially one correct answer. They are not required to interpret, connect, or evaluate.

**Fires on:**
- Biographical facts: where, when, who, how old
- Emotional snapshots: "How did you feel when..." / "How did that make you feel?"
- Scene-setting openers: "Tell me about yourself," "What's it like working here?"
- Broad opinion snapshots about external things: "Do you think the school is great?" / "How do you feel about the district decisions?"
- Yes/no or positive/negative assessments: "Would you say it was challenging?" / "Was it a good experience?"

**Examples:**
- "Where were you when that happened?" → L1
- "How did that make you feel?" → L1
- "Was there ever a difficult moment in your career?" → L1
- "Do you think Pinole Valley is a great school?" → L1
- "How do you feel about the working system right now?" → L1 (snapshot opinion about external thing)
- "Would you say most of your students were likable?" → L1

**L1 vs L2 boundary:**
"How did you feel?" = L1 (snapshot).  
"How did those feelings change over the next month?" = L2 (evolution).  
The word "feel" doesn't determine the level — the *shape of the answer required* does.

---

## L2 — Analyzing (The "How/Why")

The respondent stays inside the story's timeline but moves beyond bare facts. They explain motivations, trace change, compare two states, or describe how something worked. They are a **character in their own story** making sense of events — not yet evaluating what those events made true about their identity.

**Fires on:**
- Causal analysis within the story: "Why did you choose X?" / "What made you decide to...?"
- Emotional evolution: "How did those feelings change?" / "How did you handle that?"
- Comparison between two states: "How did that compare to...?" / "What was different about...?"
- Evaluative assessments of a period: "How has that experience been?" / "How well does the school listen to staff?"
- Sequence / process / logistics: "Walk me through..." / "What happened next?"
- Specific recall: "Do you remember a time when...?" / "Can you give me an example?"

**Examples:**
- "Why did you choose that school?" → L2
- "How did those feelings change over time?" → L2
- "How has that experience been — have teachers been open to you?" → L2
- "How would you say your experience coming to the school was?" → L2
- "How well would you say the school listens to staff?" → L2
- "Has that ever affected your personal life?" → L2
- "How would you say the education levels at this school are?" → L2

---

## L3 — Meaning-Making (The "So What")

The respondent is asked to **step outside the story's timeline** and look back from the present. The signal is a **temporal bridge** (looking back, since then, now that) or a call for **universal wisdom** (identity, purpose, what it means to be). The respondent is not recalling or analyzing — they are evaluating what the experience made true about who they are.

**Auto-fires on:**
- Temporal bridges: "looking back," "in retrospect," "with hindsight," "now that," "since then," "from where you stand now"
- Universalizing nouns: "wisdom," "meaning," "identity," "purpose," "legacy," "what it means to be"
- Identity probes: "How has that changed who you are?" / "Who are you now because of that?"
- Advice-giving: "What would you tell someone going through this?" / "What would you tell a younger version of yourself?"
- Counterfactuals: "If you could go back..." / "What would you do differently?"
- Meaning questions: "What does [X] mean to you?" / "What does home mean to you?"
- Learning/wisdom: "What have you learned from...?" / "What has that taught you?"

**Examples:**
- "Looking back, how has that shaped who you are?" → L3
- "What would you tell a younger version of yourself?" → L3
- "If you could go back and change anything, what would you do differently?" → L3
- "Why do you think you kept fighting through all of that?" → L3
- "What does home mean to you?" → L3
- "What have you learned from going through that?" → L3

---

## Key Boundary Rules

### The Temporal Shift Rule
`nowadays`, `since`, `still`, `now` only trigger L3 **if paired with personal-change language**:
- ✓ L3: "Since then, how have you changed?" (`since` + `changed`)
- ✗ NOT L3: "How would you say your experience was, since they've had years working together?" (`since` = causal context, not temporal bridge)
- ✗ NOT L3: "Do you think that's a lost thing nowadays?" (`nowadays` = general observation about society, not about the respondent's own past vs. present)

### The "How Do You Feel About X" Rule
"How do you feel about [external thing]?" = **L1**, not L2.  
It's a snapshot opinion. The respondent isn't tracing evolution or explaining mechanics.
- "How do you feel about the working system?" → L1
- "How do you feel about AI in education?" → L1  
- "How did those feelings evolve over time?" → L2 (evolution, not snapshot)

### The "Why" Split
- "Why did you **choose/decide/leave/stay**..." → L2 (concrete action within the story)
- "Why do you think you **kept going / are resilient / stayed strong**..." → L3 (identity probe)
- "Why is **[value]** important to you?" → L3 (universalizing)
- "Why did **that change you**?" → L3 (transformation signal)

### The Benefit of the Doubt
At the L2/L3 boundary, ambiguous questions **lean up to L3**.  
At the L1/L2 boundary, ambiguous questions **stay at L1**.  
Only promote to L3 if there's a genuine structural signal, not just a hard word.

### Broad Evaluative Openers = L1
Questions asking for a general take on an external thing — a school, a system, a policy, a career overall — are L1 even if they use "do you think" or "would you say."
- "Do you think the school is doing well academically?" → L1
- "Would you say teaching has been challenging?" → L1
- "Do you think AI will change education?" → L1

---

## Current Test Suite (24/24 passing)

These are the anchor cases. If you touch the classifier, run these first.

```
L1 — Do you think overall, Pinole Valley high school is a great school?
L1 — Do you think that is an advantage compared to other schools?
L1 — How do you feel about the working system that's going on right now at school?
L2 — How would you say was your experience coming to the school, not knowing how it works since they've had years working together?
L2 — How has that experience been? How have other teachers been open to you when you ask for help?
L1 — Would you overall say that has been like a good experience, or like positive or negative?
L1 — Was there ever a difficult moment in your years of being a teacher where you thought was this really the right choice?
L2 — Has that ever happened to you where it has affected your personal life?
L2 — Has that ever happened where you missed out on a friendship because you were too focused on one thing?
L2 — How would you say are the education levels of this school?
L1 — Do you think that ratio would affect our future doctors and healthcare workers?
L2 — How well would you say the school listens to the staff's wants, especially after the strike?
L1 — Would you say teaching has been challenging for you?
L1 — Would you say majority of your students you've had in your past career were likable?
L1 — Do you think that's a really lost thing nowadays? Do you think not a lot of people have the confidence to get a human connection?
L3 — If you could go back before your teaching career or you could change anything, what would you do differently?
L1 — Where were you when that happened?
L1 — How did that make you feel?
L2 — Why did you choose that school?
L2 — How did those feelings change over time?
L3 — Looking back, how has that shaped who you are?
L3 — What would you tell a younger version of yourself?
L3 — Why do you think you kept fighting through all of that?
L3 — What does home mean to you?
```

---

## Known Edge Cases to Watch

### Still Fragile
- **"How has teaching changed since you started?"** — currently L3 (hits `since` + `changed`). This *is* arguably L3 (temporal bridge + personal evolution), but some would call it L2. Watch for student disputes here.
- **"Do you think connection is important?"** — hits the `important` fallback and goes L3. Defensible but borderline.
- **"How would you describe yourself as a teacher?"** — hits `how (would you) describe` → L2. Could argue L1 (biographical self-description) or L3 (identity). Currently L2, which is the right middle ground.

### Intentionally L1 (Students May Dispute)
- Any "do you think X is good/great/challenging?" question is L1 — it's a snapshot opinion, not analysis
- "How do you feel about [external thing]?" is L1 regardless of how meaty the answer is. The *question* asks for a snapshot; the respondent may go deeper on their own. The tool measures the question, not the answer.

### Intentionally L3 (Students May Dispute)
- Counterfactuals ("If you could go back...") are always L3 — they require the respondent to construct an alternate self, which is identity evaluation
- "What does X mean to you?" is always L3 for abstract nouns (home, family, strength, love) — this is the core meaning-making move in Narrative Medicine

### Parser Edge Cases (Separate from Classifier)
- Google Docs Shift+Enter inserts U+2029 (Line Separator) — handled in cleanup chain
- Transcripts with no colon after speaker names need the no-colon repair pass
- The continuation guard fires when a new speaker's turn starts lowercase mid-sentence — gated on whether the previous turn ended with `.?!`

---

## File Location
`/Users/user/Accessible To Claude/GitHub/Interview-Transcript-Feedback-Engine/index.html`  
All logic is in a single file. The classifier is `classifyQuestionLevel()` starting around line 1640.

## Git Log (recent)
```
Rebalance L1/L2/L3: fix false L3 on opinion questions, tighten temporal bridge, add how-has/how-well/how-would-you-say L2 patterns
Add Interview Questioning Framework card; rename L1/L2/L3 labels to Reporting/Analyzing/Meaning-Making
Rewrite question level classifier with Narrative Medicine logic
Fix mis-labeled continuation guard firing on clean sentence boundaries
Fix U+2029 line separator from Google Docs breaking parser
Fix question quality scoring: count L3s, not ratio
Update doc header and input section copy
```
