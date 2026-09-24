## Runs
### Fix test runner

- **Task and start commit:** Make `CI=true yarn test` run. Commit [2d09463](https://github.com/kleros/dispute-resolver/commit/2d094637f595b2c5892f6124228797fa475b6501).
- **Done when:** `CI=true yarn test` runs at least one test and passes; `yarn start` and `yarn build` still work.
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 13 minutes total, 4 minutes mine. $4.18 API equivalent cost.
- **Result and Evidence:** Pass. Application starts and build produces the same result as before. [Test suite runs](/evidence/day-1/fix-test-runner-proof.png).
- **Learnings/Notes:** A second blocker appeared after the first fix and the agent fixed it too.



### Fixture mode

- **Task and start commit:** Fixture mode for the Ongoing page. Commit [77fa0a1](https://github.com/kleros/dispute-resolver/commit/77fa0a17c73acc9612f98704389b7a15781eb5bc).
- **Done when:** with fixture mode on, the "Ongoing" page shows the same Gnosis disputes on every load and Mainnet shows the empty state; the browser's Network tab shows no requests to the chain or APIs; with the delay on, a loading state is visible; with forced failure on, the page shows an error instead of crashing; with fixture mode off, the app works as before; `CI=true yarn test` still passes.
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 31 minutes total, 6 minutes mine. $9.84 API equivalent cost.
- **Result and Evidence:** Pass. `yarn start`, `yarn build`, and `CI=true yarn test` work as before. Limitations exist, as the fixture  mode only covers the Ongoing page, and countdowns use the current time, so they change between loads. [Gnosis with disputes](/evidence/day-1/fixture-mode-gnosis.png). [Mainnet empty](/evidence/day-1/fixture-mode-mainnet.png). [Loading](/evidence/day-1/fixture-mode-gnosis-loading.png). [Error](/evidence/day-1/fixture-mode-gnosis-fail.png).
- **Learnings/Notes:** The agent flagged a pre-existing crash on malformed disputes and reported as instructed. This caused me to make one intervention to change the fixture design, so not an agent error. Another limitation is that the switch to fixture mode puts test code into production code. Kept for the course, but it must be replaced with network-level mocking, or removed, after finishing the course and before any PR reaches production.



### Search bar (Rediscovery experiment)

- **Task and start commit:** Add a search bar to the Ongoing page. Commit [ad18b70](https://github.com/kleros/dispute-resolver/commit/ad18b70c4261efc2a2bd300595ac4d235bada139).
- **Done when:** Typing a full dispute number shows only that dispute, but partial matches still work, be it by ID, title, or court. Text search ignores case. Clearing restores the full list without refetching and typing doesn't refetch disputes. No match shows a clear message. If no ongoing disputes exist, search does nothing. It still works after changing the status filter and nothing crashes.



#### Attempt A: my usual prompt (ending "double check your work")

- **Configuration:** Claude Code, Fable 5.1, extra-high. Prompt with my own wording.
- **Cost and time:**  18 minutes total, 5 minutes mine. $6.70 API equivalent cost.
- **Result and evidence:** Meets all criteria in my own check: [9 tests pass](/evidence/day-1/rediscovery-experiment-A-tests.png), [search works](/evidence/day-1/rediscovery-experiment-A-search.png), and [there's a no results message](/evidence/day-1/rediscovery-experiment-A-no-results.png).
- **Interventions:** None.



#### Attempt B: outcome brief with checks it must run

- **Configuration:** Claude Code, Fable 5.1, extra-high. Prompt based on page 19 outcome brief with the "Done-when" as acceptance.
- **Cost and time:** 16 minutes total, 5 minutes mine. $5.69 API equivalent cost.
- **Result and evidence:** Meets all criteria in my own check. [16 tests pass](/evidence/day-1/rediscovery-experiment-B-tests.png), [search works](/evidence/day-1/rediscovery-experiment-B-search.png), and [there's a no results message](/evidence/day-1/rediscovery-experiment-B-no-results.png).
- **Interventions:** None.



#### Comparison

- Both met every criterion and the code is similar. B's no results message is clearer as it names the active filter.
- A wrote tests and checked the browser without being asked, so my "double check" habit didn't produce a sloppy result.
- B mapped its evidence to each criterion, added more tests, and flagged risks A didn't (e.g 1005 would also match 11005) or a blank page when only the status filter matches nothing.
- The conclusion is that for this simple task the outcome brief didn't change the code much, but it produced better evidence and surfaced more risks. It also produced a slightly better UI, was slightly cheaper, and faster. Still, if improvements can be seen in this small task, I'll be adopting it in the future and checking if it improves my results. The winner is B, merged into `feat/ui-overhaul` at [9cda8af](https://github.com/kleros/dispute-resolver/commit/9cda8af9f8c334ae487ce285a037f7bf689c1b9e).

#### Follow-up: handoff to Codex

- **Start commit:** [9cda8af](https://github.com/kleros/dispute-resolver/commit/9cda8af9f8c334ae487ce285a037f7bf689c1b9e).
- **Handoff note:** With a status filter that matches no disputes and an empty search, the page shows a clear message instead of a blank area. The existing no match message, with a search, and no data message, are unchanged. The malformed fixture's note is up to date. A test covers the empty filter case and all tests pass.
- **Configuration:** Codex, GPT-6 Astra, extra-high.
- **Cost and time:** 4 minutes total, 1 minute mine. Weekly usage left moved from 100% to 99%
- **Result and evidence:** Pass. [The end goal was met](/evidence/day-1/search-bar-codex-handoff-result.png), one test was added, and all commands work properly.
- **Learnings/Notes:** Handoff worked from the note alone, no intervention was needed. Codex provided a short report of what was done, but didn't check the browser. It was not asked to do so, but on earlier runs, Claude seems to do this unprompted.

### Slice 1: Ongoing page redesign (Claude vs Codex)
- **Start commit:** [3bcbb44](https://github.com/kleros/dispute-resolver/commit/3bcbb441bface83e38410d1d6d8dc5c22465f91a). Same prompt for both.
- **Done when:** Following Kleros branding, the page must look better and more modern, without losing any relevant information already displayed. The page must also be responsive, particularly at phone, tablet, and desktop widths. A dispute without meta-evidence shows a clear placeholder and a malformed dispute is handled gracefully while rendering the other disputes. Search, status filter, empty/no-match, loading, and error states must keep working. Tests should be added and passing.

#### Claude
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 25 minutes total, 5 minutes mine. $10.68 API equivalent cost.
- **Result and evidence:** Meets all criteria. 46 tests pass. [The updated UI.](/evidence/day-1/slice-1-ongoing-page-after-claude.png)
- **Learnings/Notes:** No intervention needed. Flagged that the README and fixture note were now outdated, but left them, as out of scope.

#### Codex
- **Configuration:** Codex, GPT-6 Astra, extra-high.
- **Cost and time:** 21 minutes total, 5 minutes mine. Weekly usage left moved from 99% to 98%.
- **Result and evidence:** Meets all criteria. 32 tests pass. [The updated UI.](/evidence/day-1/slice-1-ongoing-page-after-codex.png)
- **Learnings/Notes:** No intervention needed. Didn't flag the outdated files, but the UI is better.

#### Comparison
- **Mistakes Claude:** None, but the UI was considerably worse, in my opinion. Particularly in desktop width.
- **Mistakes Codex:** A very heavy blue status dropdown on focus that differs from Kleros branding.
- **Shared misses:** None.
- **Test changes or contamination:** None, both used separate copies from the same tags and memory is off for both.
- **Conclusion:** Both met the criteria. Claude was more thorough (more tests, flagged outdated files), but Codex design was better. The winner was Codex, merged at [58251a5](https://github.com/kleros/dispute-resolver/commit/58251a5951f38acbdc94d182b125a47f6a2b3c55). A small correction will follow for Codex, but an even bigger correction would be needed for Claude.

#### Small correction (Codex)
- **Done when:** "Newest first" tag is gone, as it is unnecessary. The search field and status dropdown share one subtle focus style (a thin Kleros-purple border or soft glow) instead of the current search field's thick double outline and the dropdown's blue fill. The dropdown keeps its white idle look when open. Focus stays visible for keyboard users. Tests pass.
- **Configuration:** Codex, GPT-6 Astra, extra-high.
- **Cost and time:** 5 minutes total, 1 minute mine. Weekly usage left moved from 98% to 97%.
- **Result and evidence:** Pass. [Criteria met](/evidence/day-1/slice-1-codex-small-correction.png), and tests pass.

#### Conclusion

**By hand, in this codebase, I'd estimate at least 5 hours for this slice 1 work. The agents did it in 20 minutes (Claude) and 16 minutes (Codex).** This is not counting fixing the test runner, only the ongoing page redesign.

I needed about 5 minutes to check everything was working, although I did not perform a line by line code review. I only needed to make one correction to the winning solution from Codex, which took 4 minutes from the agent and 1 from me. So Codex was able to reach a satisfactory solution in approximately 20 minutes.