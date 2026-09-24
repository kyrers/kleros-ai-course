# Brief

**Project:** Dispute resolver UI overhaul.

**Slices:**
1. Ongoing page redesign;
2. Details page redesign (the Interact page, opened from an ongoing card or dispute ID search);
3. Create page redesign;
4. Header, footer and wallet connection (navigation labels, view-only banner, network display, wallet UI/UX);

**Runs locally how:** `yarn install && yarn start` for live data. Fixture mode: `REACT_APP_USE_FIXTURES=true REACT_APP_FIXTURE_CHAIN_ID=100 yarn start` (Gnosis) or `…_CHAIN_ID=1` (Mainnet). See Dispute Resolver [README](https://github.com/kleros/dispute-resolver/blob/feat/ui-overhaul/README.md).

**Fixtures:** Dispute data captured from the chain and saved per chain (Gnosis: 8 disputes; Mainnet: none), plus a malformed dispute for error handling.

**Definition of done:** Each slice passes its acceptance check on fixture data.

**Held back for Day 5:** Chain switcher dropdown that updates the information without needing a reload.