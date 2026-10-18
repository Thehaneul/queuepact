# QueuePact

A GenLayer Intelligent Contract for assigning community-hall requests to one of
two teams and recording that team's acknowledgement.

**Status: v0.2 passed 73 local fixture checks and completed the bounded Studio
walkthrough on 18 October 2026, evidenced by user-provided Accepted-state reads.**
Contract: `0x143bFB5CBbCd63CFdE513Bdd330C22Ce9587e2F2`.

| Ticket | Key | Observed result |
| --- | --- | --- |
| 0 | mixed | REVIEW_REQUIRED; both teams needed |
| 1 | transport | REVIEW_REQUIRED; old mixed text was submitted accidentally |
| 2 | taxi | OUT_OF_SCOPE; personal transport excluded |
| 3 | notice | QUEUED for PROGRAMME, then ACKNOWLEDGED by its assigned wallet |

Ticket 3's original report and hash remained unchanged. The deployment row showed
FINALIZED; complete finalized method-call receipts were not independently audited.
The walkthrough is complete; no extra live tests are planned. See HOSTED_RESULTS.json.

Version 0.1 deployed successfully, but its first mixed ticket ended UNDETERMINED.
One rotated leader rejected two model responses. The exact rejected fields were
not logged; this is not a confirmed diagnosis of every validator disagreement.
The subsequent v0.1 board read showed zero saved tickets. The current v0.2 contract
is a fresh instance. Repository: https://github.com/Thehaneul/queuepact. No submission has been reported.
See RECOVERY_NOTES.md. Original source and failure evidence are preserved inside
ARCHIVED_V01.zip, with their history/ paths retained within that archive.

## In plain English

The venue team handles tables, chairs and room layout. The programme team handles
event timetable notices, programme listings and invitation wording. Someone submits
a short request. GenLayer interprets the request against these fixed duties.
Code selects the permitted team wallet from the accepted route. Only that wallet
can acknowledge the ticket, once. Acknowledgement means receipt and acceptance
of responsibility in this record; it does not prove any work was performed.

| AI route | Initial status | Who can acknowledge? |
| --- | --- | --- |
| VENUE | QUEUED | Venue wallet only |
| PROGRAMME | QUEUED | Programme wallet only |
| REVIEW | REVIEW_REQUIRED | Nobody |
| OUTSIDE | OUT_OF_SCOPE | Nobody |

Mixed requests, unclear duties, conflicting instructions and evaluator manipulation
go to review. Clearly excluded work goes outside scope. Neither outcome assigns a
wallet. The prototype keeps these records unchanged; clarifications require a
new ticket with a new key. A new ticket is not automatically linked to the old one.

## Deployment and public API

Constructor: `QueuePact(programme_wallet: str)`.
The deploying wallet becomes VENUE. Supply a different nonzero `0x` address for
PROGRAMME. The wallet mapping and written policy are frozen at deployment.
The deployer designates the roles; this does not prove the other wallet agreed
to the whole policy or that either wallet represents a real organisation.

| Method | Type | Purpose |
| --- | --- | --- |
| get_board() | Read | Policy, team addresses, policy hash, ticket and acknowledgement counts |
| get_ticket(ticket_id) | Read | Original decision and hash, assigned wallet and current acknowledgement |
| get_ticket_id(requester, request_key) | Read | Recover a saved ID; -1 means not found in the read state |
| submit_ticket(request_key, request_text) | Write | Assess and save one request; return its ID |
| acknowledge_ticket(ticket_id) | Write | Assigned team acknowledges once; no AI call |

Use `get_ticket` to read a numbered result. `get_ticket_id` is only a recovery
lookup requiring a public wallet address and the original key.

IDs begin at 0 and follow successful saves, including review and outside-scope
tickets. Duplicate keys are rejected for the same submitting wallet. Different
wallets have separate key namespaces. Failed operations consume neither a key
nor an ID. Any wallet may submit, including a team wallet.

## What GenLayer decides

The leader evaluates the request. Each validator independently evaluates the same
policy and full request and compares the exact route, then audits the proposed
route and reason against the full inputs. The model cannot select a wallet, change a permission,
set acknowledgement flags, edit the policy or claim a completed task.

The model returns only route and reason. Code attaches the full original request,
the complete selected team's scope (both scopes for REVIEW/OUTSIDE), and the fixed
basis label. The stored report still has five fields. Deterministic checks require
an allowed route, a nonempty explanation of at most 500 characters, and exact
equality of these attached sources. The sources are context, not a model-selected
supporting excerpt. Independent route agreement and semantic auditing remain
mandatory. Invalid model output never becomes an automatic REVIEW result.

The assessment and audit now have separate output instructions. Assessment format
errors name a bounded failure category, without echoing raw model content.

Each assessment allows at most one fresh format retry (two calls total). Each
validator audit allows at most one format retry. A valid negative audit is never
retried to seek a favourable answer. Provider errors propagate; there is no
automatic whole-transaction retry or fabricated fallback decision.

Normal path: one leader prompt and two prompts per validator. Contract-code
maximum: two leader prompts and four prompts per validator. Consensus retries,
node/provider behaviour and leader rotation may add work outside these bounds.
All ticket data is prepared and validated before storage writes begin.

## Record integrity

The policy hash covers the version, policy and both team wallets. The ticket hash
covers the original request, submitter, key, ID, policy hash, assessment, assigned
team/wallet and initial status. Later read fields `acknowledged`, `acknowledged_by`
and `current_status` are excluded. Acknowledgement preserves the original hash and
report. Hashes detect record changes; they do not authenticate physical events.

## Deliberate limits

- English text only; 600 characters per request and 40 per key.
- Keys contain lowercase ASCII letters, digits, hyphens or underscores.
- At most 1,000 tickets; acknowledgements still work when intake reaches the cap.
- No payments, personal-data vault, identity checks, spam prevention or fairness claim.
- Public ticket text must not contain secrets or private personal information.
- No external web retrieval, notifications, deadlines, reassignment, cancellation,
  task-completion proof or appeal workflow inside this prototype.
- Fixed role wallets cannot rotate; losing one can leave its tickets unacknowledged.
- Not an emergency service. These fixed teams exclude repairs and personal transport.
- Consensus can make mistakes. No measured model reliability or production claim.
- A normal backend is simpler for teams that already trust a single dispatcher.

## Reproduce local checks

Use Python 3.12 in a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-dev.txt
python prepare_test_sdk.py
python -m pytest -q test_queuepact.py
genvm-lint check queuepact.py
```

SDK preparation may need network access to download an official GenVM archive.
Direct tests use controlled responses and do not submit transactions or call live
models. Loader messages about default RPC settings are configuration output,
not evidence of network transactions.

The dependency comment matches the official first-contract documentation checked
on 18 October 2026. Direct tests use v0.3.0-rc7; the linter's newer-runner notice
is retained. These are reproduction pins, not a latest-version claim.

See IDEA_RESEARCH.md, VERIFICATION.md and PHONE_WALKTHROUGH.txt.
