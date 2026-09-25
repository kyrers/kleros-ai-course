## Runs
### Grilling: case details page (slice 2)
- **Start commit:** [268d0b9](https://github.com/kleros/dispute-resolver/commit/268d0b9707a97eee114bd2588cdb49c981522036).
- **Request given (deliberately vague):** Redesign the case details page (opened from an Ongoing card or the Interact page). Style must be consistent with the Ongoing page and Kleros branding. This page crashes in fixture mode today.

##### (a) Interview
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 17 minutes total, 2 minutes mine. $4.14 API equivalent cost.
- **Questions and decisions:**
1. How should the case page be laid out? This decides the whole structure of the redesign.
    - I accepted the recommended option with open sections and sidebar.
2. Where should the case-page fixture data come from, and which dispute states must it cover? This sets the acceptance criteria for fixture mode.
    - I accepted the recommended option of live capturing and adding what's required to the existing ongoing fixture.
3. In fixture mode there is no wallet, so the page would always show the signed-out, view-only state. How should the write flows (Submit evidence, Fund appeal, Withdraw, Sign in) behave in fixture mode?
    - I accepted the recommended option of a simulated account and in-memory writes.
4. The case page currently has its own dispute-ID search box that navigates on every keystroke, and /cases/ without an ID shows only that box. Now that the Ongoing page has search, what should happen to it?
    - I accepted the recommended option of searching only on enter or button press and a landing state.
5. Last one: how deep should the redesign go on the interaction-heavy pieces, the crowdfunding cards and the evidence-submission modal? They hold most of the page's logic and are the biggest chunk of work.
    - I accepted the recommended option of full redesign with logic untouched.
- **Explicitly not needed:** Filler copy, changing the header's view-only banner, the Create page, or any change to contract related logic.
- **Remaining assumptions / deferred:**
    - Reuses the Ongoing page's visual language, no new dependencies.
    - Keeps class components and the existing data flow. Rewrites render methods and styles, and new presentational pieces may be function components.
    - Fixture branches follow the existing one-check-per-read pattern in app.js. The case page's meta-evidence read becomes fixture-aware.
    - Fixtures move to a per-dispute layout, with the loader, README and launch.json updated and the capture script in the scripts folder.
    - Edge states (loading, not found, meta-evidence failed, view-only, non-standard) become Ongoing-style cards. Every field is read defensively.
    - The disabled "View Voting Options" dropdown becomes a plain list of ruling options. The note that jurors vote on Court stays.
    - The evidence iframe stays inline with the same sandbox, gains an open-in-new-tab link, and still needs network access in fixture mode.
    - The ruling alert becomes a "Jury decision" or "Winner" card in the sidebar, with the same wording.
    - Navigation pushes explicitly on submit instead of redirecting on every render. Route paths unchanged.
    - A case-page test file renders against fixtures, covering the synthetic states and malformed data. `CI=true yarn test` passes.

##### (b) Challenge
- **Configuration:** Codex, GPT-6 Astra, extra-high. Note that I chose Codex for this as an opportunity to experiment with it, since I have much more experience with Claude Code.
- **Cost and time:** 3 total minutes. Weekly usage left unchanged.
- **Findings:**
    - The sidebar assumes a small set of data and might break with real data, particularly for multi-select questions.
        - Cheapest experiment: a disposable layout mock with a four-outcome appeal, comparing the full sticky sidebar against compact sticky facts.
    - Simulated writes are a much bigger job than the original plan acknowledges.
    - Fixtures need a precise format.
    - Graceful degradation cannot be achieved in render methods only.
    - Replacing redirect with push is only half of the navigation fix.

##### (c) Interrogation
- **Configuration**: Done in the same session used in b.
- **Cost and time:** 10 total minutes, 4 mine. Weekly usage left unchanged.
- **Asked:** Evidence for each concern (files/lines read, commands run), what's untested, what would prove it wrong, and at least one assumption I should verify myself.
- **What held up / what didn't:** Static review only, nothing run. Navigation, degraded states and simulated writes work have direct code evidence. The sidebar concern is an untested prediction, as is the iframe inconsistency. It retracted one claim (round transitions). Assumption to verify: multi-select and free-value disputes must stay supported, because it saw all 8 captured disputes are binary. Verified they're possible, so must stay supported.

##### Result
- **Plan changes (after b and c):**
    - Layout undecided: the full sidebar might have problems with some data, so it is decided in the interface-alternatives block, with a multi-select case.
    - No simulated writes: a fixture flag shows the signed-in UI, with write actions stubbed to success or failure, mirroring production's per-action error behaviour. The real contract calls are tested separately with mocked contracts.
    - Fixtures: extend the existing ones with the case data, types preserved, one block, frozen time. Hand-made states: a four-outcome multi-select appeal, a free-value question, missing meta-evidence, malformed, and a non-existent ID.
    - Fix four existing bugs: a failed load is shown as "not found", missing data blanks the page, unavailable data defaults to valid values such as 0, and the page can keep showing the old case after the URL changes. Chain changes via the URL stay out of scope.
    - The implementation run does the fixtures and fixes with minimal visual change. The new look comes in the interface block.
- **Learnings/Notes:** The interview produced 5 decisions but over-scoped the plan. The challenge cut it and found real problems with it. I accepted every default in the interview, the challenge in a fresh session is what exposed the problems. The interrogation showed the challenge was from a static review and got one claim retracted, but it also helped clarify acceptance criteria.

### Case details page: fixtures and fixes
- **Task and start commit:** Make the case page work in fixture mode and fix the four bugs. Commit [268d0b9](https://github.com/kleros/dispute-resolver/commit/268d0b9707a97eee114bd2588cdb49c981522036).
- **Done when:** Edge case disputes are added to fixtures and all are handled correctly. All four bugs identified are fixed. Tests check what the user sees and are added for all scenarios the grilling identified. `CI=true yarn test` and `yarn build` pass.
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 1h35 total, 30 minutes mine. $43.18 API equivalent cost.
- **Result and evidence:** Pass after three reworks. [89 tests pass](/evidence/day-2/fixtures-and-fixes-tests.png), [multi-select case](/evidence/day-2/fixtures-and-fixes-multi-select.png), [failed load](/evidence/day-2/fixtures-and-fixes-failed-load.png).
- **Learnings/Notes:** Fixture tests passed every time, but my browser checks found three problems: sections popping in (a regression), a pre-existing double load on the Ongoing page, and the search replacing history on every keystroke. The agent also proposed two behaviour changes that I accepted: failures shown as failures, and exact free-value rulings. The clarified brief helped, this was a somewhat complex task and the acceptance criteria passed on the first attempt. The reworks came from real browser checks, which the brief didn't include, but perhaps should've.

### Failure injection
- **Task:** Break the case page on purpose and confirm an unchanged test catches it.
- **Done when:** The chosen test fails on the break, then passes after restoring. The test expected values don't come from the code it tests.
- **Check used:** `interact.test.js`, the test "keeps the timeline, votes, court and evidence when the meta-evidence is missing".
- **Break made:** Changed "Question unavailable." to "Question missing." in the component. Test untouched.
- **Result and evidence:** Pass. [2 tests failed on the break](/evidence/day-2/failure-injection-failed-tests.png) (the chosen one and the malformed dispute test), [89 pass after restoring](/evidence/day-2/failure-injection-tests-passing.png). Expected text is written literally in the test.
- **Web3 scenario:** If the page showed an appeal deadline later than the real one, or a side as more funded than it is, other options might not get funded in time, or a win might be obtained by default.

### Interface alternatives: case details page (Claude vs Codex)
- **Task and start commit:** Two competing redesigns of the case page from the same brief, in separate copies. Commit [8491ca5](https://github.com/kleros/dispute-resolver/commit/8491ca502fa56e5f5eaef9f246949165dc32dfbb).
- **Done when:** All information and behaviour kept. Responsive, and follows the Ongoing page style and Kleros branding. Design must be better, handle loading and error states properly, and unexpected scenarios (e.g. multiple cards to be funded in appeal) properly. User must be able to navigate easily, and no unnecessary labels or text should exist. Test and build should still be successful.

#### Claude
- **Configuration:** Claude Code, Fable 5.1, extra-high.
- **Cost and time:** 51 minutes total, 11 minutes mine. $27.33 API equivalent cost.
- **Result and evidence:** Met the acceptance criteria and added 8 tests. [The updated UI](/evidence/day-2/case-page-redesign-claude-code.png).
- **Learnings/Notes:** Slower, but followed the brief better. Required one follow-up to improve the design further.

#### Codex
- **Configuration:** Codex, GPT-6 Astra, extra-high.
- **Cost and time:** 35 minutes total, 6 minutes mine. Weekly usage left moved from 97% to 91%.
- **Result and evidence:** Met the acceptance criteria and added 6 tests. [The updated UI](/evidence/day-2/case-page-redesign-codex.png).
- **Learnings/Notes:** Finished faster and produced far more evidence, including unnecessary files. However, design was worse.

#### Comparison
- **Mistakes Claude:** Changed the withdraw logic without asking and included some filler text.
- **Mistakes Codex:** Made decisions it shouldn't have, like moving evidence display to a new tab or replacing the evidence modal with an inline form. Also had more filler text and worse design.
- **Shared misses:** Typing in the search changed the page's dispute ID fields before pressing enter.
- **Test changes or contamination:** None, both used separate copies from the same commit and memory is off for both.
- **Conclusion:** Claude won on design and on respecting the brief. Its issues were fixed in one follow-up round in the same session. Merged at [35f8864](https://github.com/kleros/dispute-resolver/commit/35f886413549c0021af31509ac2b246bcd9b832e).

#### Review
- **Configuration:** Codex, GPT-6 Astra, extra-high. Read-only review of the diff against the requirements. 
- **Cost and time:** 8 minutes total, 2 minutes mine. Weekly usage left moved from 91% to 90%.
- **Findings:** Five correctness gaps my browser checks and the tests missed. Misdiagnosed the cause of one of them, which the build session disputed and the reviewer later accepted.
- **What I changed:** Sent them to the Claude session to reproduce each with a failing test and fix. Asked the reviewer to check after the changes were implemented and it confirmed all were fixed, while still flagging a very edge case scenario that did break one of the original rules, but was an acceptable design choice by Claude Code.
- **Learnings/Notes:** Four of the gaps were edge case scenarios, but one of them was a money-flow regression introduced by my own follow-up request. The error was in my prompt, but the review definitely earned its place.
