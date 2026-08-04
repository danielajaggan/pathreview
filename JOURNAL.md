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

**Setup confirmation:** [y] App runs locally at localhost:5173
**Cohort ledger:** [y] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** (https://github.com/danielajaggan/pathreview/blob/fix/157-relevance-scorer-partial-overlap-fixture/tests/unit/test_relevance_scorer.py)

**Reproduction summary:**
[1–2 sentences: How did you reproduce the issue? What did you observe?]
an `pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v` and confirmed the test fails: the fixture text contains all 4 query keywords (python, django, web, framework), so `scorer.score()` returns a near-perfect score instead of the expected partial-overlap range (0.3–0.9).

**PLAN.md link:** [link to PLAN.md in your fork] https://github.com/danielajaggan/pathreview/blob/fix/157-relevance-scorer-partial-overlap-fixture/plan.md

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]


## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the fix for issue #157 — edited the fixture text in `test_query_with_partial_overlap` to remove 2 keywords ("web", "framework") so it only partially overlaps with the query. Test now passes locally. Ran full test suite (`make test-unit`) and lint/type checks (`make check`); confirmed 52 test failures and 182 lint errors are pre-existing and unrelated to my change (all in different files). Opened a draft PR.

**Next steps:**
Get peer/mentor feedback on the draft PR in Slack, address any feedback, then finalize and mark the PR ready for review before Sunday's deadline.

**Blockers:**
[leave blank, or note anything real you're facing]

### Check-in 2 (end of week)

**PR link:** https://github.com/danielajaggan/pathreview/pull/1

**Branch:** fix/157-relevance-scorer-partial-overlap-fixture

**What you built:**
Fixed the `test_query_with_partial_overlap` fixture in the relevance scorer test suite — the original fixture text contained all 4 query keywords, causing a perfect score instead of a genuine partial-overlap score. Removed 2 keywords so the test now correctly validates partial overlap.

**Tests added or updated:**
`tests/unit/test_relevance_scorer.py` — updated the fixture text in `test_query_with_partial_overlap`. Confirmed via `make test-unit` that no other tests were affected (52 pre-existing failures unrelated to this change, documented in the PR description).

**Self-review confirmation:** [x] make check passes  [x] make test-unit passes
*(both confirmed to introduce no new failures beyond documented pre-existing ones)*

**Draft PR feedback received from:** Pending — posted in Slack, awaiting peer/mentor review. Will update this entry once feedback is received and the PR is marked ready.





