## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/158

**Issue title:** review_service unit tests misconfigure async mocks — 13 of 19 tests fail

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The unit tests for `review_service` configure the mocked database session incorrectly. The service correctly awaits `db.execute()`, but the mock result returns a coroutine from `scalars()` instead of a normal result object. This causes calls such as `scalars().first()` and `scalars().all()` to fail even though the service implementation is correct. A successful fix will update the test mocks so that `execute` is asynchronous while the returned result and scalar methods behave like normal synchronous SQLAlchemy result objects.

**Selection notes — “Is this right for me?” checklist reasoning:**
This issue has a limited scope because it primarily affects one unit test file and does not require changes to the application’s production behavior. The failure is reproducible with a single pytest command, and the expected result is clearly defined: all existing `review_service` tests should execute correctly. The issue involves Python unit testing, `AsyncMock`, and `MagicMock`, which are skills I can investigate and test locally. Because it is labeled Tier 1 and does not appear to require major architectural changes, I believe it is a realistic issue for me to complete within the module timeline.

**Branch name:** test/158-review-service-async-mocks

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Ariunlag/pathreview/commit/FULL_COMMIT_SHA

**Reproduction summary:**
I reproduced issue #158 by running:

`pytest tests/unit/test_review_service.py -q`

The test suite produced 13 failed tests and 6 passed tests. The `get_review()` tests fail with `AttributeError: 'coroutine' object has no attribute 'first'`, while the `list_reviews()` tests fail with `AttributeError: 'coroutine' object has no attribute 'all'`.

The failures occur because the tests configure the result returned by `db.execute()` as an `AsyncMock`. The service correctly awaits `db.execute()`, but then uses `scalars().first()` and `scalars().all()` synchronously.

**PLAN.md link:** [PLAN.md](https://github.com/Ariunlag/pathreview/blob/test/158-review-service-async-mocks/PLAN.md)

**Walkthrough video (recommended):**
Not recorded

**Blockers or open questions:**
I need to verify how the mocked SQLAlchemy Result object should behave and how to handle the two `db.execute()` calls made by `list_reviews()`.