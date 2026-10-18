# QueuePact verification — 18 October 2026

**65 authored-fixture tests passed. SDK lint passed (3 checks), and semantic
validation passed. No hosted deployment or live model evaluation has occurred.**

The ABI has one constructor parameter and five methods: three reads and two writes.
The saved logs are local_test_results.txt and sdk_check_results.txt.

## Covered locally

- All four routes: VENUE, PROGRAMME, REVIEW and OUTSIDE.
- The complete planned phone sequence: mixed request, excluded request,
  programme assignment and acknowledgement by the programme wallet.
- Both teams' acknowledgement permissions; requester, other team and outsider
  rejection; unassigned-ticket rejection; acknowledgement exactly once.
- Key replay rejection, wallet-isolated keys and direct ID recovery.
- Exact policy hash, original ticket hash and preservation after acknowledgement.
- Input/address/ID validation, maximum text lengths and the 1,000-ticket boundary.
  Intake stops at the limit but existing tickets can still be acknowledged.
- Invalid model routes, empty/oversized reasons, invented or wrong-team quotes,
  raw duplicate JSON keys, malformed JSON and extra fields such as a model-selected wallet.
- One fresh format retry; a two-call assessment limit; provider errors without
  automatic retry; valid negative audit without retry.
- Independent route disagreement and evidence-audit rejection.
- Serialization of both nondeterministic closures with cloudpickle.

The injection, ambiguity and conflicting-instruction examples use authored REVIEW
responses. They verify safe handling of that result, not live attack detection.
Duplicate keys are detectable in raw JSON text; duplicates already collapsed by
a provider into a decoded object cannot be recovered by this contract.

## What consensus checks mean here

The SDK direct runner normally executes the leader result without a validator
network. Running the validator callback separately tests agreement logic; it
does not undo a direct-mode write that already occurred.

A specifically labelled local gate harness invokes the callback before returning
the proposed assessment to the contract, and raises on rejection. It checks that
the contract then creates no ticket, assignment, consumed key or counter change.
This is not evidence of real network rollback, finality or Byzantine robustness.

No test measures model accuracy, response latency or network cost. The official
SDK's configuration output mentions default RPC settings, but this fixture suite
does not submit transactions or make live model calls.

## Tooling and corrections retained

Python 3.12.14; genlayer-test 0.29.2; genvm-linter 0.11.0;
pytest 9.1.1; cloudpickle 3.1.2; direct SDK v0.3.0-rc7.
The dependency header matches the official first-contract example checked today.
The linter reports a newer runner; this is a deliberate reproduction pin.

The first fixture run was interrupted during SDK setup after setup errors.
Its partial output is retained in local_initial_setup_results.txt; a complete
traceback was not captured, so no more specific cause is asserted. The pinned
official archive was then prepared with prepare_test_sdk.py.

The next run reached 31 passes and one fixture expectation failure: a float ID
was rejected by calldata encoding before the contract's integer check. The test
was corrected to expect that earlier SDK rejection and still check unchanged
state. Its original output is in local_fixture_correction_results.txt.

Two constructor exceptions were changed to the SDK's UserError type after linter
advice. The final source passed lint and validation with no such warnings, and
the final complete fixture run passed all 65 cases in 3.09 seconds.

Previous project code and deployments were not changed or retested. Reused local
tooling and SDK preparation code are infrastructure, not new evidence about those
projects. ExplainGate remains finished and submitted.

## Hosted evidence still to collect

One new contract address, the initial board read, three saved ticket results and
one acknowledgement preserving the original ticket hash/report. Record actual IDs,
wallets, errors and transaction outcomes. An Accepted-state read is not a full
finalized receipt audit; keep those evidence types separate.

The bounded phone walkthrough is in PHONE_WALKTHROUGH.txt. No optional live tests
are planned after it completes. Production suitability, private identity,
physical work completion, contest eligibility and rewards are not established.
