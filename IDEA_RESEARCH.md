# Next-project research — 18 October 2026

Recommendation: **QueuePact**, a shared request queue with team-specific acknowledgement.
These are prototype ideas, not validated businesses or claims of original invention.

| Idea | Practical purpose | Distinct workflow | Main drawback |
| --- | --- | --- | --- |
| QueuePact | Send a community-hall request to the team whose agreed duties cover it. | AI selects the team; deterministic code grants only that team's wallet permission to acknowledge. | Useful as shared coordination only when participating teams actually want a common decision record. |
| Report grouping | Link differently worded reports of the same incident. | Maintains incident groups and avoids duplicate work. | Close GenLayer prior art; false merges and recurring incidents complicate evaluation. |
| Lost-item matching | Compare a lost-item description with a found-item record. | Creates a possible match for a property office to investigate. | Public descriptions cannot prove ownership; private distinguishing details should not be exposed on-chain. |

## Research findings and choice

FixMyStreet demonstrates that routing reports to responsible bodies is an
established operational problem. Its conventional system uses location and
category. QueuePact instead tests interpreting a short natural-language request
against two frozen duty descriptions and binding the result to a wallet's next
permitted action. We are not claiming to replace or integrate with FixMyStreet.

GenLayer's fit guide explicitly warns that ordinary routing without a meaningful
shared state change is often better handled by a conventional backend. QueuePact's
narrow GenLayer role is to decide who may acknowledge a particular immutable
ticket in a shared record. It is a conditional fit for independently operated
teams that agree to the policy; one organisation with a trusted dispatcher can
use a simpler database and rules/AI service. Customer demand remains untested.

The existing GenLayer-Dedup-Registry evaluates semantic duplicates, so incident
grouping is a less distinct next entry. Its README was retrieved for prior-art
review; its claimed deployment or test results were not independently verified.

MissingX asks users for descriptive details to support matching, while a property
office confirms the result. That reinforces the difference between text similarity
and ownership proof. A useful lost-property system would need an off-chain private
verification process, adding scope beyond our small phone walkthrough.

QueuePact is the strongest of these three for our current constraints: two fixed
teams, four routing outcomes, one permissioned follow-up action and no private
evidence, payments, web fetching or large candidate search.

## Difference from all six previous projects

| Previous project | What it handles | QueuePact's difference |
| --- | --- | --- |
| SourceRoots | Evidence origins and documented separate evidence-producing events | Allocates a task to a team; does not trace evidence provenance. |
| HoldoutLedger | Documented separation of experimental groups | Has no datasets, experiment admission or leakage classification. |
| ScopeHandshake | Whether new client work requires both parties' change approval | Chooses between teams and limits acknowledgement to one wallet; no price, brief amendment or two-party change authorization. |
| NoticeParity | Fidelity of translated notices and permission to reuse them | May route a request about a notice, but does not translate, compare or authorize reuse of its content. |
| ExplainGate | Learning checkpoints and learner advancement | Contains no grading, lessons or progression. |
| SurplusHold | Quantity reservations and stock release | Contains no inventory, reservations or scarcity allocation. |

Source-grounded judgments and strict validation are reusable engineering patterns.
The new application state is task assignment and acknowledgement, not a rename
of the earlier workflows. Support routing was an unbuilt alternative in the
SurplusHold research; this implementation develops that separate idea.

## Scope and planned effort

- Implement one English-language community-hall policy with VENUE and PROGRAMME.
- Separate meaningful REVIEW_REQUIRED and OUT_OF_SCOPE results from execution errors.
- Cover both teams, permission failures, replay, malformed responses, bounded retries,
  model disagreement and record preservation locally using authored fixtures.
- Phone scope: one deployment, three ticket submissions, one acknowledgement,
  five essential reads, two wallets and one wallet switch.
- No emergency triage, work-completion verification, identity checks, payment,
  customer outreach, independent demand validation or reward guarantees.

## Sources checked

1. [GenLayer: When to Use GenLayer](https://docs.genlayer.com/developers/intelligent-contracts/when-to-use-genlayer)
2. [GenLayer: Equivalence Principle](https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle)
3. [GenLayer: Your First Contract](https://docs.genlayer.com/developers/intelligent-contracts/first-contract)
4. [FixMyStreet: How it works](https://fixmystreet.org/how-it-works/)
5. [GenLayer-Dedup-Registry README](https://github.com/Ritapossible/GenLayer-Dedup-Registry)
6. [MissingX: Registering a lost item](https://www.missingx.com/en/help/how-to-register-a-lost-item)
7. [MissingX: Waiting for a response](https://www.missingx.com/en/help/waiting-for-a-response)

Names are working labels. DeskRelay and DutyRoute had existing product hits, so
they were not selected. The targeted QueuePact search returned no results; this
is not a trademark, comprehensive prior-art or originality clearance.
