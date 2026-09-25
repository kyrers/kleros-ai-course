# Next

## Day 1 -> Day 2
- **State:** Ongoing page redesign committed at [58251a5](https://github.com/kleros/dispute-resolver/commit/58251a5951f38acbdc94d182b125a47f6a2b3c55) with a minor follow up at [268d0b9](https://github.com/kleros/dispute-resolver/commit/268d0b9707a97eee114bd2588cdb49c981522036).
- **Unfinished checks / review:** None.
- **Next:** Day 2 of the course.

## Day 2 -> Day 3
- **State:** Case page redesign committed at [35f8864](https://github.com/kleros/dispute-resolver/commit/35f886413549c0021af31509ac2b246bcd9b832e).
- **Unfinished checks / review:** None.
- **Next:** Day 3 of the course.
    - Open work potentially useful for some Day 3 tasks:
        - Restyle the evidence form to match the new design.
        - Free-value questions show Yes/No funding cards.

## Day 3 -> Day 4

**Progress note from the `/goal` session interruption (copied from the session):**
- **Done:** Create page redesigned, fixture mode extended, README and launch config updated, Create checklist written and linked from SKILL.md.
- **Checks passing so far:** `CI=true yarn test` shows 131 passed, 0 failed (98 before; 33 new tests); `yarn build` succeeded; the Gnosis fixture preview shows the form and review step with no console errors, no chain or IPFS requests, and no horizontal overflow at 375px, 768px and 1280px.
- **Verified in the browser:** validation marks title, question and both ruling options and focuses the first invalid field; cost updates from 36.0 to 48.0 xDai with 4 votes; the review step shows every typed value, the parties and "2 ruling options".
- **Left:** run the `verifying-ui-changes` skill on the change  and produce its evidence table; then the final report with the new test names listed.
- **Next step:** invoke the `verifying-ui-changes` skill, which will rerun tests and build and walk the Create checklist against each launch configuration.

**Handoff to day 4:**
- **State:** Create page redesign committed at [344a90d](https://github.com/kleros/dispute-resolver/commit/344a90d5b5b96157d75cefd709e63ff4607ce303).
- **Unfinished checks / review:** None.
- **Next:** Day 4 of the course.
    - Open work potentially useful for some Day 4 tasks:
        - Restyle the evidence form to match the new design.
        - Free-value questions show Yes/No funding cards.
        - Case page amounts say ETH on Gnosis, while the Create page says xDai. Make them consistent.