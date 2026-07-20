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
