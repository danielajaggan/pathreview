## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/157

**Issue title:** Relevance scorer “partial overlap” test fixture actually has full query overlap

**Tier:** [y] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
[In 3–5 sentences, in your own words: what the issue is (not a copy-paste of
the title), what is currently broken or missing, and what a successful fix
would accomplish. Naming the part of the codebase it affects is helpful context.]
The function test_query_with_partial_overlap is currently broken as there are issues in the test data, this includes all 4 keywords from the search query inside the test text. Seeing that all words are there, the system scores it as a perfect 1.0 which makes the test fail since it expects a score below 0,9. In order to fix this,some words from tests/unit/test_relevance_scorer.py need to be removed from the test text so it only counts as a partial match

**Branch name:** [paste branch name here] fix/157-relevance-scorer-partial-overlap-fixture

**Setup confirmation:** [ ] App runs locally at localhost:5173

**Cohort ledger:** [y] Issue added to cohort ledger