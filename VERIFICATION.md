# QueuePact verification — 18 October 2026

**Version 0.2: 73 authored-fixture tests passed in 3.38 seconds. SDK lint passed
(3 checks), and semantic validation passed. The bounded v0.2 Studio walkthrough
completed on 18 October 2026, supported by user-provided Accepted-state reads.**

Version 0.1 deployed and returned the expected initial board. Its first mixed
ticket ended UNDETERMINED after leader rotations and validator disagreement.
An additional screenshot shows one leader exhausting two invalid model responses.
Its post-failure board showed zero saved tickets. The new v0.2 instance subsequently
saved four tickets and recorded the assigned programme wallet's acknowledgement.
See HOSTED_RESULTS.json, RECOVERY_NOTES.md and history for the separate evidence.

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
- Invalid model routes, empty/oversized reasons, raw duplicate JSON keys,
  malformed JSON and extra fields such as a model-selected wallet or quotation.
- The stored request is the entire original input. REVIEW/OUTSIDE carry both
  complete policy scopes; single-team routes carry the selected complete scope.
  Validators reject shortened, invented or wrong-team sources and altered basis.
- The mixed-request regression needs no model-generated quotation. Assessment
  and audit prompts have separate schemas. Error categories are bounded and do
  not echo raw model text. Two invalid independent responses still reject a vote.
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

Version 0.1 had two constructor exceptions changed to the SDK's UserError type
after linter advice; it passed 65 fixtures in 3.09 seconds. Those exact files and
logs are archived in ARCHIVED_V01.zip under history/queuepact-0.1. Its later live failure demonstrates
that fixture success does not establish model compatibility or consensus.

Version 0.2 preserves the permission and storage paths, simplifies the model
schema, attaches sources in code, separates audit instructions, and reports
bounded format-error categories. Its first fixture run passed 73 cases in 3.38
seconds. Lint and semantic validation passed. There were no live model calls or
new transactions during these local checks.

Previous project code and deployments were not changed or retested. Reused local
tooling and SDK preparation code are infrastructure, not new evidence about those
projects. ExplainGate remains finished and submitted.

## Completed bounded hosted walkthrough for v0.2

Contract: `0x143bFB5CBbCd63CFdE513Bdd330C22Ce9587e2F2`.
The initial v0.2 board showed the correct version, two wallets, zero tickets and
zero acknowledgements. The deployment transaction row showed FINALIZED.

Accepted-state get_ticket reads showed IDs 0 and 1 as REVIEW_REQUIRED; ID 2 as
OUT_OF_SCOPE; ID 3 as QUEUED for PROGRAMME and then ACKNOWLEDGED by the programme
wallet. Ticket 1 was an operator input mistake: key transport with the old mixed
text. It was preserved; key taxi produced the intended excluded-transport result.
The walkthrough therefore used four ticket submissions and one acknowledgement.

The full original report and hash of ticket 3 matched before/after acknowledgement.
All four displayed ticket hashes and the policy hash were reproduced from the
normalized screenshot transcription. This verifies transcription consistency,
not an independent retrieval of on-chain state. HOSTED_RESULTS.json contains the
transcribed fields and screenshot filenames; screenshot images are not bundled.

No full finalized method-call receipts or independent RPC state were retrieved.
No final board-totals read was requested. This is a completed small demonstration,
not a model-accuracy benchmark or general consensus-reliability claim. VENUE-only
routing and rejection/security/capacity cases remain local-fixture coverage only.

The completed phone record is in PHONE_WALKTHROUGH.txt. No optional live tests
are planned. Production suitability, private identity,
physical work completion, contest eligibility and rewards are not established.
