---
artifact_type: schema_plan
authority: speculative
generated_by: claude-lane
parent_artifacts:
  - gpt:.plan/260727-1744-cross-check-s0-harness-study.md
  - .plan/260727-1932-reaction-gpt-s0-cross-check-concede-blockers.md
tags: [harness, ledger, schema, capability-dimensions, replay-profiles, n1-trace,
       c-item-dispositions, d-harness-1]
---

# Schema Plan — The Harness Ledger: Capability Dimensions, Event Families, N=1 Trace

*Design plan for the katallagent harness ledger, per the handoff in
`gpt:.plan/260727-1744` (cross-check of S0-HARNESS). Evidence citations (`ES-*`,
`RR-*`, `TP-*`, `RM-*`, `AO-*`, `CA-*`) resolve in `katallagent/lab/s0-harness/`
`evidence.jsonl`. Authority: speculative — this is a plan for review, not a decision;
D-HARNESS-1 remains open until gpt reviews this artifact. Implementation, when
licensed, lands in `katallagent/` outside `lab/`; the N=1 spike (a study) lands in
`katallagent/lab/`. Timestamp from `date`: 2026-07-27 19:32.*

---

## 1 · D-HARNESS-1, restated (per cross-check Finding 1)

Candidate decision text, adopting gpt's reformulation:

> **D-HARNESS-1 (claim-to-observation gate).** No adaptive-mechanism result may earn
> a product or architecture decision unless the mechanism ran under a preregistered
> observation surface sufficient for its claimed outcome, resource, provenance, and
> contestability properties. Common N=1 capture begins before any baseline used for
> later comparison is collected.

Consequences: exploratory spikes under smaller instruments are legal but their results
are inadmissible for decisions; the "observation surface sufficient for the claim" is
checkable against §2's capability dimensions (a claim's required dimensions must be
covered by the recording that produced it); the harness itself is justified as the
*standard* preregistered observation surface, not as a universal prerequisite.

## 2 · Replay as a capability vector (per cross-check Finding 2)

Five independent dimensions; every recorded component, and every replay/attribution
claim, is graded per dimension. No consumer may compare positions across dimensions
as a single ordinal grade.

| Dimension | Values (ascending within the dimension only) |
|---|---|
| **REC** record completeness | `audit` (metadata only) · `partial` (some payloads) · `complete` (every boundary payload, writer-side) |
| **EXE** execution fidelity | `none` · `stubbed` (interior re-executed, recorded boundary results injected) · `deterministic` (bit-stable recompute of the component) · `stochastic` (re-execution without determinism) |
| **EQV** equivalence criterion | `exact-state` · `exact-output` · `distributional` · `task-semantic` · `judge-scored` (criterion must be named, with known error characteristics — AO-04, RR-07, RM-05) |
| **INT** intervention support | `none` · `analytic` (recompute from recorded data under a known outcome function) · `model-based` (learned critic/reward counterfactual) · `rerun` (environment re-execution from a checkpoint fork) |
| **VAL** validity envelope | the recorded assumptions under which the above hold: pinned code/model/environment identities, stationarity assumptions, declared removal/replacement semantics (CA-08), and expiry (e.g., model-snapshot retirement — RM-09) |

**Named profiles** (shorthand only, defined as vectors; consumers must expand them):

- `R0` = (audit, none, —, none, —)
- `R1` = (complete, none, —, analytic where an outcome function exists, payload-bound)
- `R2a` = (complete, stubbed, exact-output on interior commands, none, pinned harness code) — the Temporal/Durable contract (ES-04, ES-05)
- `R2b` = (complete, deterministic, exact-state/output, none, controlled component only — AO-02, RR-02)
- `R3` = (complete, stochastic, named criterion required, none, live model snapshot)
- `R4` = (complete, any, named criterion, rerun with declared removal semantics + checkpoint fork (RR-06), stationarity declared)

Standing facts the profiles encode: hosted-model calls never exceed `EXE=stochastic`
(AO-01, RM-10); `INT=analytic` requires no execution fidelity at all (the CA-05
analytic case — this is why the ladder was not ordinal); `INT=rerun` claims expire
with their VAL envelope.

## 3 · Event model

One append-only stream; every event carries an **envelope** (field categories, names
indicative): event id; trace/span/parent correlation with n-ary containment links
(TP-01, TP-04); logical position whose visibility matches commit order (ES-08);
writer identity; harness version; schema version (ES-01); idempotency key.

**Event families** (proposed type vocabulary; bodies at category level):

1. **Lifecycle** — `actor.spawn` (parent actor, inheritance-manifest reference),
   `actor.retire`.
2. **Boundary crossings** (writer-side, payload-complete — RR-09, AO-06, AO-07) —
   `model.call` (resolved snapshot id, never alias-only (RM-09); full request/response
   payload refs; decoding config (AO-08); usage/cost), `tool.call`, `artifact.read` /
   `artifact.write` (PROV usage/generation typing — TP-06), `message.send` /
   `message.receive` (inter-agent, content retained — AO-06).
3. **Episode** — `episode.begin` (task id, attempt index — AO-03), `episode.end`,
   `outcome.observe` (joint outcome, always recorded even when per-agent
   decompositions exist — CA-06).
4. **Evaluation claims** (facts about assertions, never adjudications — B1) —
   `claim.assert` (claimant, rubric, judge model version, comparand ordering — AO-04,
   AO-09), `claim.supersede` (references its target), `claim.dispute`. Counterfactual
   claims additionally carry versioned removal semantics (CA-08) and their capability
   vector.
5. **Corrections** (RQ4 taxonomy — ES-07, ES-10) — `event.compensate` (references the
   false event); disputed interpretations need no event family (new derived view);
   migrations are upcast-on-read plus an auditable `migration.record` when
   copy-transform is unavoidable (ES-03).
6. **Replay & forks** — `checkpoint.mark`, `replay.begin` (claimed capability vector,
   replay-mode flag), `divergence.detect` (localized via periodic state digests —
   RR-04), `fork.create` (first-class, references its checkpoint — RR-06).
7. **Harness self-description** (RM-12, C17) — `harness.config` (version, environment
   snapshot, measured recorder overhead).

**Payload externalization**: envelope events carry content-addressed references plus
digests; sensitive payloads live outside the factual skeleton (ES-11, RR-11) — the
mechanism is an open choice (§6).

**Derived layer**: every score, view, and aggregate is a named, versioned derivation
over the stream, rebuildable from it and only from it (ES-12, TP-11, ES-08); the
stream's consumer API is read-only with no runtime callbacks to agents (T1–T6).

## 4 · C1–C18 dispositions (per cross-check requirement 3)

| C | Disposition | Concern | Consequence / test |
|---|---|---|---|
| C1 append-only + tamper-evidence | **adopt** (mechanism open, §6) | schema + storage | no in-place mutation path exists in the API; tamper-evidence mechanism chosen at spike |
| C2 schema version + non-destructive evolution | **adopt** | schema | P5 exercises the upcast path |
| C3 correction taxonomy | **adopt** | schema | spike deliberately writes one `event.compensate` |
| C4 boundary enumeration → derived grades | **adopt** | schema + runtime | boundary registry recorded; per-component capability vectors derived, not asserted; P2 tests sufficiency |
| C5 writer-side payload-complete capture | **adopt** | schema + storage | deliberate deviation from OTel content default, recorded (AO-07) |
| C6 correlation/DAG/commit-order/idempotency | **adopt** | schema (order visibility: storage) | replay of any committed prefix is internally complete |
| C7 write-time identity binding | **adopt** | schema | alias-only model identity recorded as explicit degradation |
| C8 stochastic config + attempt index | **adopt** | schema | repeat-run variance derivable (CA-02) |
| C9 resource metering from run 1 | **adopt** | schema | RM-11; open choice: fields on boundary events vs separate events (§6) |
| C10 claims attributed/referenced/versioned | **adopt** | schema | rescoring never rewrites (T5) |
| C11 joint outcomes co-recorded | **adopt** | schema | CA-06 |
| C12 replay machinery | **split**: events (checkpoint/fork/divergence/replay) **adopt** = schema; effect-suppressing gateways + replay-mode enforcement = **not-schema** (runtime requirement) | schema / runtime | per gpt's flag; runtime half lands with the spike harness code |
| C13 removal semantics + substitution hooks | **split**: claim-side semantics **adopt** = schema; substitution replay hooks = **defer** to spike | schema / runtime | CA-08, CA-09 |
| C14 no relevance-based up-front sampling | **not-schema** (governance/runtime policy); placeholder: if sampling ever exists its decisions are events (TP-02) | governance | TP-09; policy recorded in constitution, not fields |
| C15 observer/institution boundary T1–T6 | **not-schema** except the provenance fields making T4/T5 audits computable (**adopt-partial**) | architecture / governance | per gpt's flag; T1–T6 become review criteria for any future consumer |
| C16 sensitive-payload handling | **adopt** category (external refs + digests); mechanism **open choice** (§6) | security | ES-11; skeleton survives erasure |
| C17 harness self-identity + overhead | **adopt** | schema | P4 measures overhead through the harness's own records |
| C18 storage economics from measured I/O | **defer** to spike measurement | storage | RR-10; sizing decision follows P4 data |

Nothing is rejected outright; the study's constraints survive, but ten of eighteen
resolve fully into schema, and the rest split into runtime, storage, security, or
governance homes — which is the separation the cross-check demanded.

## 5 · The minimal N=1 trace (per cross-check requirement 5)

The smallest recorded episode that exercises every adopted schema constraint — the
spike's acceptance test, proposed as **S1-N1-TRACE** in `katallagent/lab/`:

1. `harness.config` (C17) → `actor.spawn` (root, no parent) → `episode.begin`.
2. ≥2 `model.call` + ≥1 `tool.call` + ≥1 `artifact.write` with payload-complete
   capture and resolved snapshot ids (C5, C7).
3. `outcome.observe` (joint outcome) + one `claim.assert` by an LLM judge with full
   claim provenance (C10, C11).
4. `episode.end` with resource aggregation (C9).
5. One deliberate `event.compensate` against a seeded wrong event (C3); one schema
   upcast exercised on read (C2).
6. `checkpoint.mark` → `replay.begin` at profile `R2a` → stub-replay the episode with
   `divergence.detect` armed (C12-schema; tests P2).
7. Repeat the episode ≥N times to populate attempt indexes and variance records
   (C8; tests P1 against the hosted endpoint).
8. Measure and record recorder overhead in `harness.config` (C17; tests P4 — cutoffs
   are design choices, measured values reported regardless).

The spike's product is findings (P1–P5 measured), so it lives in `lab/`; the harness
code it exercises is the first implementation artifact outside `lab/`.

## 6 · Unresolved choices, preserved as choices (per cross-check requirement 4)

1. **Tamper-evidence mechanism**: hash-chaining vs enforced-append store (TP-07).
2. **Sensitive-payload mechanism**: externalized blob store vs per-subject
   crypto-erasure (ES-11) — legal/operational trade-offs, needs user input.
3. **Storage substrate**: undecided by design (study non-goal); sized after P4/C18
   measurements.
4. **Resource representation**: fields on boundary events vs separate metering events.
5. **Fetch-digest provenance** (gpt correction 2): whether ledger artifact references
   carry archived-snapshot digests — recommended, unpriced.
6. **Sampling placeholder**: whether the schema reserves a sampling-decision event
   family now or on first need (TP-02 vs YAGNI; leaning first-need).

## 7 · Review request

gpt review requested on: the D-HARNESS-1 restatement (§1), the dimension vocabulary
and profile definitions (§2), the event-family vocabulary (§3), the C-item
disposition table (§4), and whether the N=1 trace (§5) is sufficient to exercise the
adopted set. On acceptance, D-HARNESS-1 goes to the user for ratification with §1 as
its text, and S1-N1-TRACE becomes the next study artifact.
