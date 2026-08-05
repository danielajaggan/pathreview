## Solution plan

**Issue:** Relevance scorer "partial overlap" test fixture actually has full query overlap — https://github.com/ascherj/pathreview/issues/157

### Understand
The test `test_query_with_partial_overlap` expects a mid-range score (0.3–0.9) to confirm the scorer correctly handles partial keyword overlap. However, the fixture's chunk text contained all 4 keywords from the query ("python", "django", "web", "framework"), so `RelevanceScorer.score()` returned a near-perfect score (~1.0) instead of a genuine partial match, causing the assertion `0.3 < score < 0.9` to fail.

### Map
Files touched:
- `tests/unit/test_relevance_scorer.py` — the fixture inside `test_query_with_partial_overlap` (query string + chunk text)

Files referenced but not modified:
- `rag/evaluator/relevance_scorer.py` — the `RelevanceScorer.score()` and `_tokenize()` methods being exercised by the test, reviewed to understand exactly how keyword overlap is calculated before editing the fixture

### Plan
1. Read `RelevanceScorer.score()` and `_tokenize()` to understand exactly how keywords are matched (case handling, tokenization) so fixture edits are evidence-based, not guesswork
2. Remove 2 keywords ("web", "framework") from the chunk text so only "python" and "django" genuinely overlap with the query
3. Re-run `test_query_with_partial_overlap` locally to confirm the score now falls within 0.3–0.9
4. Run the full `test_relevance_scorer.py` file to confirm no other tests in the same file broke
5. Run `make test-unit` and `make check` across the whole repo to confirm no unrelated regressions, and document any pre-existing failures observed

### Inputs & outputs
**Input:** the fixture's `query` string and `chunks` list (specifically the `text` field of each chunk)
**Output:** `scorer.score()` returns a float between 0.3 and 0.9, satisfying the existing assertions without changing any production code

### Risks & unknowns
- Other tests in this file (e.g. `test_multiple_keyword_matches`, `test_score_ranges_from_zero_to_one`) could implicitly depend on similar fixture phrasing patterns — need to confirm none of them break when this fixture changes
- Removing too many/too few keywords could push the score outside the 0.3–0.9 band entirely (either back to near-1.0 or down near 0.0), so the exact choice of which keywords to remove matters and was validated empirically by running the test after each edit
- Unsure whether `_tokenize()` treats hyphenated compound words (e.g. "front-end") as one token or two — if a future fixture used hyphenated terms, the overlap calculation could behave unexpectedly. Not directly relevant to this fix, but worth flagging since it affects how "keyword overlap" is defined in general

### Edge cases
- The edited fixture must retain at least one keyword overlap so it doesn't collapse into the same case as `test_query_with_zero_keyword_overlap` (which expects `score < 0.5`)
- The rewritten chunk sentence must stay grammatically coherent so it still resembles the realistic parsed text the scorer would see in production (not an artificially constructed edge-case string)
- Case sensitivity: confirmed via `test_case_insensitive_matching` elsewhere in the file that matching is case-insensitive, so capitalization differences in the fixture aren't a risk here