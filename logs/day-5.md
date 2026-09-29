## Runs
### Claude routine
- **Task and start commit:** A cloud routine that runs the test suite on a private training copy of the repo and writes a report. Training repo `main` branch, with head matching the dispute resolver at [bc66f32](https://github.com/kleros/dispute-resolver/commit/bc66f32c2cbbe378e7326545d33fe25c27b301e3).
- **Done when:** Two runs on identical input, with the second producing no duplicate effects. One run on a changed fixture. Missing or invalid output must be visible. The schedule disabled at the end.
- **Configuration:** Claude Code routine (cloud), Sonnet 5, no connectors, training repo only.
- **Cost and time:** 11 total minutes, 3 minutes mine. $1.71 API equivalent cost. Values include all 4 runs.
- **Routine contract:**
  - **Trigger:** Run now, plus a daily schedule, paused at the end.
  - **Input version:** The training repo's `main` HEAD, recorded in the report.
  - **Permissions:** The training repo only, no connectors. A custom environment was needed to customize network access, namely allowlisting `repo.yarnpkg.com`.
  - **Output path:** `reports/test-report.md` on the branch `claude/test-report`.
  - **Validation:** Commit, status, counts and failing test names. I compared these with my own local test run.
  - **Owner:** Me.
  - **Failure behaviour:** An ERROR report with the reason and no source changes.
- **Result and evidence:**
  - **Run 1:** ERROR. The default environment blocked the Yarn download, and the routine refused to run an alternative download. It still pushed a visible ERROR report, while the run list showed "Completed". I forgot the project required a specific `yarn` version when creating the routine. Fixed by allowing `repo.yarnpkg.com` in a custom environment.
  - **Run 2:** PASS, 269/269 on `bc66f32`, matching my local run. Report produced correctly and pushed to `claude/test-report` branch.
  - **Run 3:** Identical input: "No change", no new commit.
  - **Run 4:** After changing a court name in a fixture: FAIL, 265 passed and 4 failed with the same four test names as my local run. Report produced correctly and pushed to `claude/test-report` branch.
- **Learnings/Notes:** As the resources mention, the run's "Completed" status only indicates that the infrastructure worked, evidenced by the first run completing while its task failed.

### CLI wrapper
- **Task and start commit:** A script that runs Claude Code non-interactively and decides success itself, in the training repo. Commit [e651259](https://github.com/kyrers/kleros-ai-course-routine-training/commit/e651259037433e296c665cefe35a40f916d2529f).
- **Done when:** It takes a task as input, uses structured output, enforces an outer timeout, parses the result, verifies the required artifact independently, and shows either success, or failure with the reason.
- **Configuration:** Script written by Opus 5.5 on high. The runs use Sonnet 5 through `claude -p` with my subscription login.
- **Cost and time:** 5 total minutes, 1 minute mine. $0.44 API equivalent cost. Includes wrapper creation and the two runs.
- **Inspection first:** The training repo has no project hooks and no `.mcp.json`, and my user settings have no hooks, so print mode loads nothing that runs unasked.
- **Result and evidence:** The `scripts/run-agent-task.sh` has JSON output with a schema, `timeout` around the run, `jq` parsing, and its own checks that the artifact exists, was written during this run, is valid JSON and has the required keys.
  - **Success:** Gave it the task to run the tests and report in a specific format. Result was `PASS`, exit 0. The report artifact contained exactly what was asked.
  - **Failure:** A 5-second timeout gave `FAIL: agent timed out after 5s`, exit 1.
- **Learnings/Notes:** The agent's own "done" isn't trusted. The script checks the output file itself, so an agent claiming success without producing a valid, fresh file still gets a FAIL. That independent check is what makes it safe to run unattended.

### Capstone
- **Task and start commit:** The held-back feature, a chain switcher dropdown with improved logic, from a fresh brief to an accepted change. Commit [bc66f32](https://github.com/kleros/dispute-resolver/commit/bc66f32c2cbbe378e7326545d33fe25c27b301e3).
- **Done when:** Tests cover switching with a wallet, without one and a rejected or failed switch. The page shows the new chain's data without a reload. Tests, build, and the `verifying-ui-changes` skill pass. The independent reviewer finds no problems and my own inspection passes.
- **Configuration:**
  - Implementer: Claude Code, Fable 5.1, high.
  - Reviewer: Codex, GPT-6 Astra, high, a fresh session given only the task, the diff and the acceptance criteria.
- **Cost and time:**
    - Implementer: 1h24 total, 27 minutes mine. $43.60 API equivalent cost.
    - Reviewer: 12 total minutes, 2 minutes mine. Weekly usage left moved from 88% to 85%.
- **Clarification:** The implementer asked 8 questions before writing code. Before doing so, it inspected the code and scope, and asked for clarifications and specific decisions before starting the implementation.
- **Review rounds:**
  - Round 1: Without the implementer agent rationale, it found 2 issues. Fixed, and confirmed by the reviewer.
    - Anchoring experiment: After its first verdict, I shared the implementer's reports. It kept both findings, explained why the implementer's evidence didn't cover them, and even checked the implementer's other claims against the code.
  - Round 2: This was done on the fixes from my own inspection. It reported 4 issues, with different degrees of severity. I decided to fix only 3 of them.
  - Round 3: This found 2 issues, including the highest severity issue of every round, having to do with verifying the responder from the scripts loaded inside the `iframe` in a case page. This was introduced in the second round of fixes, where the script was rewritten. Both issues were fixed and confirmed by the reviewer.
- **My inspection:** Did a thorough inspection of the app, navigating across every page on different screen sizes, trying different actions, and using the app with and without a wallet. Found 3 issues, one having to do with data fetching logic on chain switching, and the other two were minor UI taste issues. At the end of round 3, asked both the implementer and reviewer sessions for a test plan, on which they converged to a surprising degree. After running the test plan, I found no issues.
- **Result and evidence:** Committed at [5e37a52](https://github.com/kleros/dispute-resolver/commit/5e37a52). 341 tests, build, and the `verifying-ui-changes` skill passing. The [chain switcher dropdown UI](/evidence/day-5/chain-switcher-ui.png);
- **Unfinished:**
  - Pre-existing, but after a chain switch, old-chain contract queries still complete in the background. The results are discarded. The user gets no visual problem from this.
- **Learnings/Notes:** The initial questions asked by the implementer did help, and it's another indication that grilling has a place in this type of task. The independent reviewer found real issues in every round, including a security issue introduced by one of the fixes. Cannot make a generalization of this single example, but it made no difference knowing or not knowing the implementer's rationale. My own inspection still found the most serious bug, but this was a relatively complex task because of the poor logic implemented in the codebase for a long time.