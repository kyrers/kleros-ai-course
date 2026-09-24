# Rediscovery

**Three postponed ideas and what stopped each:**
1. Full dispute resolver refactor, mostly focusing on UI:
    - Postponed because the code is a mess and impossible to dive into without AI. Earlier models did not seem capable to handle this big of a task either. Haven't tried in a while.
2. Court V1 tech stack modernization:
    - Postponed because it's a core product and earlier models seemed to struggle, frequently breaking the project after the first few changes while claiming success in refactoring a small part.
3. Personal reading queue:
    - Postponed mostly because it was never a priority for me, just a fun side project. AI should be able to do this easily at this point.

**Chosen idea:** Dispute resolver UI overhaul.

**Smallest useful demo:** Ongoing page redesigned. 

**Acceptance check:** With hardcoded data, a new user can see relevant data for existing disputes, filter by state, and the page shows sensible empty, loading and error states.

**Stretch goal:** Modernize the entire tech stack.

**Old obstacle:** Poor and complex code written on an old tech stack.

**Current hypothesis:** Current LLMs can work through the DR code and deliver a noticeably better UI while I define and judge the result, without needing to read most of the code.

**Experiment to run on Day 1:** Adding a search bar to the ongoing page, done twice from the same commit, but one ending with my usual *"double check your work"* vs another with an outcome brief with checks the agent must run.
