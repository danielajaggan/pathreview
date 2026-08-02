## Solution plan

**Issue:** Relevance scorer "partial overlap" test fixture actually has full query overlap — https://github.com/ascherj/pathreview/issues/157

### Understand
The test `test_query_with_partial_overlap` expects a mid-range score (0.3–0.9) to confirm the scorer correctly handles partial keyword overlap. However, the fixture text contains all 4 keywords from the query ("python", "django", "web", "framework"), so `scorer.score()` returns a near-perfect score (~1.0), causing the test to fail.

### Map
- `tests/unit/test_relevance_scorer.py` — contains the broken fixture inside `test_query_with_partial_overlap`
- `rag/evaluator/relevance_scorer.py` — the `RelevanceScorer.score()` method being tested (referenced only, not modified)

### Plan
1. Remove 1–2 keywords from the chunk text so it only partially overlaps with the query
2. Re-run the test locally to confirm the score now falls within the expected 0.3–0.9 range
3. Run the full test file to make sure no other tests broke
4. Run `make check` to confirm linting/type checks still pass (note: 21 pre-existing mypy errors unrelated to this fix already exist in this file)
5. Commit the fixture fix with a clear message referencing issue #157

### Inputs & outputs
**Input:** the fixture's `query` string and `chunks` list (text content)
**Output:** the fixture text is edited so `scorer.score()` produces a value between 0.3 and 0.9, satisfying the existing assertions

### Risks & unknowns
- Removing too many keywords could push the score too low instead of landing in the mid-range — may need to test variations
- Need to confirm the scoring algorithm's exact keyword-overlap logic rather than guessing by trial and error

### Edge cases
- Ensure the edited fixture still has at least one keyword overlap (otherwise it duplicates `test_query_with_zero_keyword_overlap`)
- Confirm the edit doesn't make the sentence grammatically broken, since similar phrasing patterns are used elsewhere