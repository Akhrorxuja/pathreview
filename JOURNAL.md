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
