## Skill
### verifying-ui-changes
- **What:** `.claude/skills/verifying-ui-changes/`, from the Day 2 repeated verification procedure. Runs tests and build, per-page fixture checklists stored in `references/`, real mode, and checks at least three widths. Produces an evidence table, and has stop or escalate rules.
- **Customization:** `disallowed-tools` makes it report only. !`git status --short -- . ':(exclude).claude'` lists uncommitted changes, so it asks when there's nothing to verify.
- **Time:** 10 minutes to write.

## Runs
### Skill evaluation: skill-creator
- **Task:** Evaluate the skill with `skill-creator`.
- **Done when:** It reviews the skill and uses the validations in `skill-creator`. Every issue is either fixed or explained. The final rating for the skill must be at least a 9.
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 25 total minutes, 5 minutes mine. $38.88 API equivalent cost.
- **Result and evidence:** Pass after two rounds of fixes. First grade was 7/10, because it found some unrunnable checks. After the fix, round two was rated 9/10, as it claimed the description was not pushy enough, one of the references was missing an instruction, and the original command to check the diff didn't filter unnecessary diffs. Third round got 10/10 after these fixes were applied.
- **Learnings/Notes:** My prompt only said to use the `skill-creator` to evaluate the `verifying-ui-changes` skill. Maybe this was not a good prompt, because the run was surprisingly long and heavy. It not only evaluated it, but ran each test twice, with and without the skill and compared them.

### Skill smoke test
- **Task and start commit:** Three prompts, each in a fresh session. Commit [90343af](https://github.com/kleros/dispute-resolver/commit/90343af9c0f7977d8494462e1f0f4fc28348522c).
- **Done when:** The skill activates on a verification request, doesn't activate on a similar non verification request, and asks when no change is specified.
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 13 minutes total, 0 minutes mine. The session that should activate the skill took most of the time (11 minutes). $5.64 API equivalent cost, again with the session that should activate the skill spending most of it ($4.55).
- **Result and evidence:** Pass. Should activate session loaded the skill and ran the full procedure. The similar session that shouldn't activate the skill didn't, and just delivered a commit summary. The missing input session loaded the skill, detected the missing input, and asked which change to verify.
- **Learnings/Notes:** All sessions worked as expected. Although I must say that my prompt for the session that should activate the skill probably wouldn't have worked if the `skill-creator` review before didn't tell me to be more pushy in the description. It also provided useful information. The other two sessions behaved as expected.

### Workaround retest: CLAUDE.md scope rule
- **Workaround to retest:** Removing "Don't rewrite, restructure or reformat code outside what the task needs" from `CLAUDE.md`.
- **Task and start commit:** Fix the Escrow V1 unhandled rejection. Commit [90343af](https://github.com/kleros/dispute-resolver/commit/90343af9c0f7977d8494462e1f0f4fc28348522c).
- **Done when:** Both fix it and tests and build pass.
- **Configuration:** Claude Code, Fable 5.1, extra-high. Two copies, same prompt.
- **Cost and time:** 
    - With the rule: 12 total minutes, 2 minutes mine. $4.02 API equivalent cost. 
    - Without: 10 total minutes, 2 minutes mine. $4.05 API equivalent cost.
- **Result and evidence:** Both passed, with tests and build working. Small differences between the two:
    - With the rule: 2 files, +43/−7. 
    - Without: the same 2 files, +46/−6, with one extra change related to the fix and nothing outside it. Merged this version, as it carries the simplified `CLAUDE.md` and also has a stronger test.
- **Learnings/Notes:** The rule prevented no demonstrated failure, so I removed it. The "keep changes to the task" rule already covers it. This was a very simple and clear task, so I'll keep an eye out for this.

### Goal run: Create page (slice 3)
- **Task and start commit:** Redesign the Create page with `/goal`, including fixture support, tests and a Create checklist for the `verifying-ui-changes` skill. Commit [0dee790](https://github.com/kleros/dispute-resolver/commit/0dee790157304c4a1b99ef17796c61d650fd3aff).
- **Done when:** The goal condition is met, with evidence in the transcript. The fixture mode covers the page, the design matches the other pages, behaviour and create dispute calls are unchanged, placeholders and feedback states work, tests and build pass, and the skill's evidence table passes every row. Bounded by 30 turns or 60 minutes and 3 retries per check, in natural language, as R16 suggests, so not enforced.
- **Configuration:** Claude Code, Fable 5.1, extra-high, auto mode, `/goal` with the default Haiku evaluator.
- **Cost and time:** 1h05 total, 25 minutes mine. $33.80 API equivalent cost (loop session cost included, but negligible).
- **Loop:** A read-only 2-minute check (`git status` and `git diff --stat`) in a second session. The first run showed 11 changed files. The second run showed three new test files. Stopped after two iterations.
- **Interruption and recovery:** Saved a progress note in `next.md`, interrupted, and later resumed the session. `/goal` showed the full original prompt still active, so no acceptance criteria were lost and the progress note was not needed.
- **Forced failure:** Changed a message the new tests check, and the suite failed. Restored it, and the suite passed. A test run matching no files reports that no tests were found, which isn't a pass. My `/goal` condition already ruled that out by requiring tests explicitly and for them to be named.
- **Result and evidence:** Pass. The goal was met with evidence. Two follow-up rounds were needed: UI taste adjustments, a pre-existing single party limit (which I asked to fix), and a data-loss case my own tests found (two parties with the same address silently lost one). [Form](/evidence/day-3/create-page-redesign-form.png) and [Review](/evidence/day-3/create-page-redesign-review.png) pages redesigned. Committed at [344a90d](https://github.com/kleros/dispute-resolver/commit/344a90d5b5b96157d75cefd709e63ff4607ce303).
- **Learnings/Notes:** The goal worked unattended and survived an interruption with its full context. Its first turn ran over 20 minutes without any evaluation, since the evaluator only runs between turns. The single party limit wasn't fixed until I asked, most likely because my prompt said to keep the existing fields and `CLAUDE.md` says not to fix unrelated issues. The data loss came after this fix, and the skill's checks missed it because the agent only tested with different addresses.