# Log

## Habit to retest (R02):

- Asking the model to double check its work at the end of its run.

## AA table version + date (R04):

- AA Coding Agent Index v1.5, checked on September 22nd 2026. Same as the PDF.

## Plans / limits (R05):

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



## Setup

- Created the `feat/ui-overhaul` branch on the dispute resolver, based on `master` last commit [#cd01cdb](https://github.com/kleros/dispute-resolver/commit/cd01cdbf3650562ac4bd42df28efa1a5539ee20e).
- Claude Code had memories regarding the dispute resolver. Those were moved to a backup folder and auto memory was disabled for the repository. Codex had no memories and the memory setting was already off.
- The repository also had a `CLAUDE.md` that was mostly wrong. It was trimmed and also shared to Codex via a `AGENTS.md` file, so both have the same instructions.
- During the harmless check, both Claude Code (Fable 5.1 xHigh) and Codex (GPT-6 Astra xHigh) ran slightly different versions of the `yarn test` command. The suite is broken, so it failed. Claude Code, however, [explained why](evidence/day-1/claude-code-check.png). [Codex didn't.](evidence/day-1/codex-check.png)



## Overall plan usage:


| Day | Claude weekly (used) | Claude Fable (used) | Codex weekly (left) | Note |
| --- | -------------------- | ------------------- | ------------------- | ---- |
| 1   | 3% → 9%              | 4% → 12%             | 100% → 97%         |   Claude used more. No limits hit.   |




## Runs:
- [Day 1](logs/day-1.md)
