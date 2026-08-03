## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/146

**Issue title:** PII scrubber fails to redact parenthesized US phone numbers

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The PII scrubber's phone number regex in `safety/pii_scrubber.py` handled
separators inconsistently across the pattern — the gap after the optional
parenthesis allowed a space, dash, or dot, but the gap before the final four
digits only allowed a dash or dot, not a space. This meant formats like
`(555) 123-4567` and `+1 555 123 4567` passed through both `scrub()` and
`detect()` completely unredacted, since the regex simply didn't match them.
The fix updates all three separator groups in the `phone_us` pattern to
consistently allow space, dash, dot, or no separator at all, so every common
US phone format is caught. All four tests named in the issue now pass:
test_us_phone_number_redaction, test_us_phone_formats, test_detect_phone_pii,
and test_phone_at_start_of_text.

**Branch name:** fix/146-parenthesized-phone-redaction

**Setup confirmation:** [ ] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Akhrorxuja/pathreview/commit/7922612

**Reproduction summary:**
Ran the existing unit test suite before fixing the code and confirmed
`test_us_phone_formats` failed on parenthesized and space-separated phone
numbers (e.g. `(555) 123-4567`, `+1 555 123 4567`), which passed through
`scrub()` and `detect()` completely unredacted due to inconsistent
separator handling in the `phone_us` regex.

**PLAN.md link:** https://github.com/Akhrorxuja/pathreview/blob/fix/146-parenthesized-phone-redaction/PLAN.md

**Walkthrough video (recommended):**

**Blockers or open questions:**
Still need to confirm the app runs locally at localhost:5173 — Docker
Desktop install has been delayed by a slow internet connection. No other
open questions on the fix itself; all four named tests pass.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the core fix to the `phone_us` regex in `safety/pii_scrubber.py`,
correcting the inconsistent separator groups so parenthesized and
space-separated phone formats are properly redacted. All four tests named
in issue #146 pass.

**Next steps:**
Run `make check` and `make test-unit` to compare against the pre-existing
baseline on `main`, clean up any lint issues introduced by my own change,
and open the PR for review.

**Blockers:**
Local environment setup took longer than expected (Python version mismatch,
missing Rust toolchain for compiling `cryptography`), but resolved by
installing Python 3.11 and Rust via rustup.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/623

**Branch:** fix/146-parenthesized-phone-redaction

**What you built:**
Fixed the `phone_us` regex in the PII scrubber so it consistently redacts
US phone numbers across all common separator formats (dashes, dots, spaces,
and parentheses), resolving issue #146.

**Tests added or updated:**
No new tests were added — the existing tests in
`tests/unit/test_pii_scrubber.py` already covered the required scenarios.
All four tests named in the issue now pass.

**Self-review confirmation:** [x] make check passes  [x] make test-unit passes
(No new lint errors or test failures introduced compared to `main`; see
PR description for the full before/after comparison.)

**Draft PR feedback received from:** none
