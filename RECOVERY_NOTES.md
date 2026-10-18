# QueuePact 0.2 recovery — 18 October 2026

## What the screenshots prove

Version 0.1 deployment was shown FINALIZED. Its Accepted-state board read had
ticket_count 0 and acknowledged_count 0, with the intended two team addresses.
The first mixed ticket subsequently showed UNDETERMINED, multiple leader rotations,
and validator disagreements. Execution SUCCESS and output 0 were proposed execution
data, not confirmation of a saved ticket.

The later screenshot shows a rotated leader raising:

`ValueError: EVALUATION_OUTPUT_INVALID: two invalid responses; no ticket saved`

This confirms two responses were rejected by v0.1's parser/report checks. It does
not reveal whether the failure was JSON, keys, route, reason, basis or a quotation.
It also does not explain every earlier Disagree vote. Raw responses and full RPC
receipts are unavailable; the read-only RPC request returned HTTP 403. The exact
response that failed cannot be reproduced from this screenshot alone.

## What changed

- The model returns only `route` and `reason`, with explicit character and key limits.
- Code attaches the original request and full applicable policy text, eliminating
  model transcription and excerpt selection as failure points. For REVIEW/OUTSIDE,
  both scopes are attached. The five stored report fields keep their names.
- Validators require those sources to match exactly, independently derive a route,
  compare it exactly, and audit the proposed reason against the full input/policy.
- Assessment and audit share routing rules but each has only its own output schema.
- At most two assessment attempts remain allowed. Failure categories are reported
  without printing raw model output. Provider failures still propagate. A negative
  audit remains a rejection, not a reason to retry for approval.

This addresses identifiable fragility in response handling. It is not proof that
quotation copying caused this specific failure, or that all hosted disagreements
will disappear. No failing response was reclassified into a successful result.

## What was checked

73 authored-fixture checks passed; SDK lint passed 3 checks and semantic validation
passed. Mixed requests still remain unassigned, and only the assigned team wallet
can acknowledge a queued ticket. Invalid outputs, source tampering and independent
disagreement still fail. These checks used no live models or hosted transactions.
Full test limitations are in VERIFICATION.md. The original source hash is
`0fbb1b71ce4ea94337dcf58f9ee674c4092c0d48ed32f9240994167816d07018`.

## Completed phone recovery

The old board was read at 21:33 Lagos time: zero tickets and acknowledgements.
Only its abbreviated address was supplied. A fresh v0.2 instance was deployed at
`0x143bFB5CBbCd63CFdE513Bdd330C22Ce9587e2F2` with the same two team wallets.
The initial board and four saved tickets were observed. An accidental extra mixed
ticket under key transport was kept; key taxi supplied the intended outside-scope
case. Ticket 3, notice, was assigned to PROGRAMME and acknowledged by that wallet.
The before/after report and hash match. The final result screenshot is at 21:49.

The initial v0.1 attempt remains failed-attempt evidence. No in-place migration was
performed. The v0.2 walkthrough is complete; no extra live tests are planned.
These are screenshot observations, not independently retrieved finalized receipts.
The GitHub repository exists at https://github.com/Thehaneul/queuepact.
Its first upload mixed archived v0.1 files into the root. This corrected flat
bundle keeps the archive in ARCHIVED_V01.zip to prevent duplicate-name collisions.
No submission has been reported.

## Technical reference

GenLayer's equivalence-principle documentation recommends independently deriving
decision fields and comparing those fields, with explicit error handling:
https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle

Transaction debugging distinguishes execution results from consensus outcomes:
https://docs.genlayer.com/developers/decentralized-applications/fee-outcomes-and-debugging

Checked 18 October 2026. These references support the approach; they do not verify
this contract's hosted behavior.
