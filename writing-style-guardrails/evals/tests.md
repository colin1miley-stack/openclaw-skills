# Writing Style — Self-Evaluation Tests

## Test 1: Banned Word Detection

**Input:**
> "We will foster a robust landscape of opportunities and leverage our holistic paradigm to streamline your workflow."

**Expected:**
- Flag: foster, robust, landscape, leverage, holistic, paradigm, streamline
- Rewrite: "We will create opportunities that actually pay and fix your workflow."

**Pass criteria:** All 7 banned words identified and replaced

---

## Test 2: Passive Voice Detection

**Input:**
> "The report was written by the team. The data was analyzed by software. The results were presented by the manager."

**Expected:**
- Flag: All three sentences are passive
- Rewrite: "The team wrote the report. Software analyzed the data. The manager presented the results."

**Pass criteria:** Passive voice identified, active voice rewrite provided

---

## Test 3: Setup Language Removal

**Input:**
> "In conclusion, the key takeaway is that we should focus on consistency. To summarize, consistency beats volume."

**Expected:**
- Flag: "In conclusion", "the key takeaway is", "To summarize"
- Rewrite: "Consistency beats volume."

**Pass criteria:** All setup phrases removed, content reduced to essential statement

---

## Test 4: Sentence Rhythm Variation

**Input:**
> "We believe in transparency. We believe in honesty. We believe in results. We believe in hard work. We believe in integrity."

**Expected:**
- Flag: 5 consecutive sentences with identical structure
- Rewrite varies structure:
> "We believe in transparency. Honesty too. Results? Non-negotiable. Hard work is table stakes. And integrity — that's the one we don't compromise on."

**Pass criteria:** At least 3 different sentence structures used

---

## Test 5: Specificity Over Abstraction

**Input:**
> "Many companies struggle with various challenges in their sales processes, leading to significant losses in potential revenue."

**Expected:**
- Flag: "Many" (vague), "various" (vague), "significant" (vague), "potential" (hedging)
- Rewrite with specifics:
> "67% of SaaS companies lose 23% of pipeline to slow follow-up. That's €50K per rep per quarter."

**Pass criteria:** All vague terms replaced with specific numbers or examples

---

## Test 6: Heading Specificity

**Input:**
> ## Key Takeaways
> ## Why It Matters
> ## Final Thoughts

**Expected:**
- Flag: All three are generic
- Rewrite:
> ## The One Decision That Changes Everything
> ## What This Costs If You Ignore It
> ## Do This Before Monday

**Pass criteria:** All headings reference specific content, not generic containers
