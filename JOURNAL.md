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

**Setup confirmation:** [ x] App runs locally at localhost:5173 Setup incomplete: Docker Desktop could not be installed due to insufficient disk space on C: drive. Docker is required to run the PostgreSQL, Redis, and ChromaDB services the app depends on. Will complete setup once disk space issue is resolved 

**Cohort ledger:** [y] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:**[ [link to commit documenting the reproduced issue]](https://github.com/danielajaggan/pathreview/edit/fix/157-relevance-scorer-partial-overlap-fixture/JOURNAL.md)

**Reproduction summary:**
Ran `pytest tests/unit/test_relevance_scorer.py -q` and confirmed the test fails with `assert 1.0 < 0.9`. The chunk "Django is a Python web framework for rapid development" contains all four query terms (Python, Django, web, framework), so the scorer correctly returns 1.0. The fix is to change the chunk text so only some query terms are present.

**PLAN.md link:** [link to PLAN.md in your fork] https://github.com/danielajaggan/pathreview/blob/fix/157-relevance-scorer-partial-overlap-fixture/plan.md

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
The app setup is still incomplete and is expected to be completed once the disk issue is resolved

