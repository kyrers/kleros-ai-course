# Resources log

## Habit to retest (R02):

- **Asking the model to double check its work at the end of its run.**
  - **Day 1 evidence:** tested against the outcome brief on the search bar (see [logs/day-1.md](logs/day-1.md)). Both met the criteria, but the brief gave better evidence and flagged more risks, so I'm dropping "double check your work" in favour of explicit checks.

## AA table version + date (R04):

- AA Coding Agent Index v1.5, checked on September 22nd 2026. Same as the PDF.

## Plans / Limits (R05):

- **Claude Max 20x, €180/month (purchased as a business):** 
  - Note this has been my plan for a while and pre-dates the start of the course.
  - Limits:
    - 5 hours session limits, weekly limit for all models, weekly limit for Fable.
  - Starting quota (Sep 23rd):
    - 5 hour session: 3% used.
    - Weekly limit all models: 3% used.
    - Weekly limit Fable: 4% used.
- **ChatGPT Pro 5x, €83.74/month (purchased as a business):**
  - This is the second provider for the purpose of the course.
  - Limits:
    - Weekly limit shared across Codex, Work, Workspace Agents, and ChatGPT for Excel.
  - Starting quota (Sep 23rd):
    - 100% usage left on weekly limit.
- **Total €263.74/month, the "Recommended" bundle per the course.**
- **An extra plan would only be worth it if:**
  - I hit the limits on both my plans, and
  - saves ≥ 2.54 hours/month (approx. 2h32min)

## Claim to test (R25):

- **AIs have poor taste for UI.**
  - **Day 1 evidence:** same prompt to Claude and Codex gave clearly different designs. Codex's was better, but still needed minor taste corrections. Claude design not tried yet, but it might produce better results.
  - **Day 2 evidence:** same prompt to Claude and Codex gave different designs again. However, this time, Claude Code had the better design and followed the brief better. Still needed one intervention with taste corrections.
  - **Day 3 evidence:** Claude alone, via `/goal` produced a design I liked on the first run. It only needed small taste corrections. Could be because it had one more screen to look at and base off.
  - **Day 4 evidence:** UI changes were limited to footer and header, but the design matched the other pages correctly. Minor adjustments were needed. Although this day had the smallest UI overhaul so far, it does seem like having more examples is improving the agent's styling choices.
  - **Day 5 evidence:** The chain switcher matched the redesigned header on the first pass. Although, one minor taste correction was needed even for this simple task.

## Visual direction (R09):

- **Direction:** Consistent with the redesigned Ongoing page, within Kleros branding. No Refero style found that helps the Dispute Resolver after a quick 10 minute look.
- **Constraints for new UI:** No filler copy, plain factual labels, re-use existing components.
- **Papercuts to fix:**
  - ~~The filler header on the Ongoing page~~ (fixed on Day 3 as part of slice 3);
  - ~~The unstyled "View mode only" banner~~ (fixed on Day 4 as part of slice 4);
  - ~~The "Interact" navigation label doesn't say what the page is~~ (fixed on Day 4 as part of slice 4);
  - ~~Ongoing cards show "Court unavailable" while the court list is still loading~~ (fixed on Day 4, during the effort comparison);

## Development mode (R32)
- **Workflow:** Critical, because the case page handles evidence, appeal funding, and withdrawals. Errors are easy to spot. Thus, vibecode and vibecheck.
- **Who implements:** Claude Code or Codex.
- **What gets checked:** The executed tests, including checking the transaction contents for contract interaction related tests, plus my own browser checks.
- **Code review:** None, per this cell.
- **Hidden failure that would change it:** If the contract call tests take their expected values from the same code they test, a wrong amount or ruling would pass. Errors would then be hard to spot.

## Harness (R15):
- **Evaluator:** A separate review session, alongside the tests and my browser checks.
- **Durable state:** Git commits, `next.md` and the logs. Agent memory is off for the course, so nothing carries over.
- **Sandbox:** Separate copies and fixture mode. `.env` is readable by agents, but only contains `REACT_APP` variables, which are designed to be public in the app bundle anyway.
- **Feedback:** Tests, build output, and the browser checks. Also, the new `verifying-ui-changes` skill, if applicable.
- **Workaround tested:** Removing "Don't rewrite, restructure or reformat code outside what the task needs" from `CLAUDE.md`, because a similar rule already exists, and the talk argues that this type of rule (example use was a fixed memory schema) might stop paying off as models improve.

## Recovery prediction (R19):
- **Prediction:** When one of my two concurrent workers fails and I relaunch, the failed worker runs again, and so does any worker that started after it, even if it had finished. Workers that started before it are reused. This is stated in the resource and the course PDF itself. However, with both workers started together, which one counts as "after" can vary. Also, per the course "Use a small graph" section, the integrator should have requirements at the join, meaning that if one worker fails, it should stop. After relaunch, it runs once both workers succeed.
- **Actual (Day 4 block 4):** As predicted. After I stopped the adapter, the worker that started first, and relaunched, the adapter reran, and so did the completed UI worker, because it started after it. The integrator only ran once both passed the join. I did not predict the relaunch reusing the previous run's worktrees, which the start-revision check caught.

## Effort and model hypothesis (R22):
- **Hypothesis 1 (block 5, required):** On a bounded task, the same model at a lower effort gives the same accepted result for less usage/cost.
- **Measured on:** accepted quality, complexity (diff size), my review time, usage.
- **Result:** Supported on both tasks (Codex, GPT-6 Astra): medium was faster (1m31 vs 2m20, and 2m31 vs 3m04) with an equal or better result. Only two small tasks, so not a generalization, but actually useful for my personal experience, as I tend to not reduce effort, but it appears that I should try it more often.

# Measurements and Setup

## Setup

- Created the `feat/ui-overhaul` branch on the dispute resolver, based on `master` last commit [#cd01cdb](https://github.com/kleros/dispute-resolver/commit/cd01cdbf3650562ac4bd42df28efa1a5539ee20e).
- Claude Code had memories regarding the dispute resolver. Those were moved to a backup folder and auto memory was disabled for the repository. Codex had no memories and the memory setting was already off.
- The repository also had a `CLAUDE.md` that was mostly wrong. It was trimmed and also shared to Codex via a `AGENTS.md` file, so both have the same instructions.
- During the harmless check, both Claude Code (Fable 5.1 xHigh) and Codex (GPT-6 Astra xHigh) ran slightly different versions of the `yarn test` command. The suite was broken, so it failed. Claude Code, however, [explained why](evidence/day-1/claude-code-check.png). [Codex didn't](evidence/day-1/codex-check.png). The suite has since been fixed on Day 1.

## Overall plan usage:

| Day | Claude weekly (used) | Claude Fable (used) | Codex weekly (left) | Note |
| --- | -------------------- | ------------------- | ------------------- | -------------------------------------- |
| 1   | 3% -> 9%             | 4% -> 12%            | 100% -> 97%        | Claude had more runs. No limits hit. |
| 2   | 9% -> 18%            | 12% -> 24%           | 97% -> 90%         | Both had same number of runs, but Claude had a more difficult task. No limits hit. |
| 3   | 18% -> 28%           | 24% -> 41%           | 90% -> 90%         | Only Claude code was used today. No limits hit.  |
| 4   | 28% -> 42%           | 41% -> 65%           | 90% -> 88%         | Claude did the heavy lifting, particularly the workflow run. No limits hit.  |
| 5   | 42% -> 47%           | 65% -> 72%           | 88% -> 85%         | Aside from the capstone task, usage was not heavy today. No limits hit.  |



## Runs:

- [Day 1](logs/day-1.md)
- [Day 2](logs/day-2.md)
- [Day 3](logs/day-3.md)
- [Day 4](logs/day-4.md)
- [Day 5](logs/day-5.md)
