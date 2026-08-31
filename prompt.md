#1 

I want to build: <one-line description>

WHAT I HAVE (connect these into a coherent design)
- <bullets>

LOCKED — already decided, do not redesign
- <anything settled, even if just "use Postgres" or "must be read-only">

OPEN — genuinely undecided, want your thinking here
- <the real unknowns>

WHAT I WANT BACK
1. Full architecture broken into phases
2. Each phase: what it does, why, inputs/outputs
3. Step-by-step numbered flow within each phase
4. Dependencies between phases
5. Flag conflicts in my points — don't silently resolve them

STRESS-TEST IT
- Check this against realistic scale/edge cases before finalizing — where would it break, and why? (Give me real numbers, not "should be fine")

CURRENCY CHECK
- If anything depends on a current tool, library, or technique, search the web first rather than relying on memory — things move fast and I want the current best option, not a stale one

TEACHING MODE
- Explain new terms in plain English with a small example before using them
- Show "before this phase / after this phase" wherever useful

RULES
- Say explicitly when you're guessing or filling a gap
- Don't write code unless asked
- One clarifying question max, only if truly needed




#2
I want to build: <one-line description>

WHAT I HAVE
- <bullets>

DATA SHAPE / PROBLEM CHARACTERISTICS (fill this in even if rough)
- e.g. "the two things being compared are usually very different sizes"
- e.g. "one side is always a subset of the other"
- e.g. "expected scale: X rows, Y comparisons"

LOCKED (if anything is already decided)
- <bullets, or "nothing locked yet">

WHAT I WANT BACK
1. Phased architecture, step-by-step within each phase
2. For EVERY technique/library/algorithm you recommend:
   a. State the specific mathematical assumption or metric it's optimized for
   b. Explicitly check that assumption against my DATA SHAPE section above
   c. If it's a mismatch (even partial), say so and name the specific variant that fits — do not default to the most commonly-cited/popular version if a more specific variant exists
3. Do this assumption-check even if nothing is LOCKED yet — check against my data shape, not just against prior decisions

CURRENCY CHECK
- Search the web for current tools/libraries before recommending — don't rely on memory

RULES
- State explicitly when you're recommending the "default/popular" choice vs. a shape-specific variant, and why
- Flag when you're uncertain whether a specific variant is warranted — don't present a plausible-sounding generic answer as verified

#3
Session 6 — Teradata dry run
Just plug the already-built crawler into your organization's real Teradata and watch what happens. No new code — just "does this actually work against a real system, or does something break." Think of it like a test drive after building the car.

Session 7 — Candidate generation + gates
This is where cross-database matching starts. Take every key-shaped column from MSP and every key-shaped column from Indigo, and instead of comparing all of them against all of them (too slow), use a fast filter (LSH — the "phone book" trick from earlier) to narrow it down to a short list of candidates worth actually scoring. Output: a shortlist, not final answers yet.

Session 8 — Scoring + edge emission
Take that shortlist from session 7 and actually run the real scoring — containment, semantic similarity, name matching — on each candidate pair. Write the results as "edges" (relationships) into the OKF markdown files, tagged [inferred] with a confidence level. This is the session your build-status doc flagged as blocked by the spec 02 vs spec 00 conflict — needs that resolved first.

Session 9 — Staleness + confirmed writeback
Two jobs: (1) detect when a relationship needs to be re-checked because the underlying data changed significantly, and (2) properly write human confirmations back into the files without erasing anything — this depends on session 5's carry-forward fix already being in place.

Session 10 — Query Builder: field resolution (part 1)
Given a plain question like "loans with failed review," figure out which actual database fields are needed to answer it, using the OKF map built in sessions 7–9.

Session 11 — Query Builder: SQL generation
Take the resolved fields and relationships, and generate the actual SQL query — including the transforms (like LPAD to fix leading zeros) discovered during matching.

Session 12 — Query Builder: validation/reconciliation
Run the generated SQL against real data, check the match rate makes sense (not suspiciously low), before trusting it.

Session 13 — Query Builder: grading + packaging
Assign a grade (A = fully confirmed, ready to run automatically; B = one gap, needs a human glance) and package the whole tool as something a QA agent can call.

#4
"Read CLAUDE.md and BUILD-PLAN.md first. Resolve the specs/02 vs specs/00 containment/Jaccard conflict before starting session 7. Build only the session marked NEXT — don't do extra work."
