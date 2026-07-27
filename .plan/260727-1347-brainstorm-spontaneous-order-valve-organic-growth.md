---
artifact_type: brainstorm
authority: speculative
generated_by: claude-lane
parent_artifacts:
  - cross-team/artifact_types/artifact_types.json
convergence: n/a
tags: [spontaneous-order, hayek, valve, fission, stigmergy, credit-assignment,
       memory-divergence, comparative-advantage, single-to-multi-agent]
---

# Brainstorm — Growing a multi-agent system organically: Spontaneous Order × Valve

*Status: speculative brainstorm. Not a plan, not a decision. Frames the founding question of
the katallagent project: how does a single-agent system grow organically into a multi-agent
system, with roles emerging from selection pressure rather than being assigned from a human
org chart? One rule (Spontaneous Order), one real-world example (Valve Corporation).
Timestamp from `date`: 2026-07-27 13:47.*

---

## 1 · Spontaneous Order, distilled

Hayek's core distinction is between *taxis* (made order: an org chart, a factory, a planned
economy) and *cosmos* (grown order: language, common law, markets, cities). Grown order
emerges when three ingredients coexist:

1. **Dispersed knowledge.** The information needed for good decisions exists only locally,
   in fragments, and *cannot be centralized* — the crux of Hayek's "The Use of Knowledge in
   Society" (1945). Central planning fails not because planners are dumb but because the
   knowledge physically can't reach them in time.
2. **A compressed signal.** Prices aggregate millions of local judgments into one scalar
   that strangers can act on without knowing why it moved.
3. **Selection feedback.** Actors who read the signals well gain resources; those who
   don't, shrink. Order is not designed — it's what survives.

Spontaneous order still needs *rules* — property rights, contract enforcement. Hayek never
said "no rules"; he said rules should constrain the **game**, not dictate the **moves**.
This is the design stance: we design the constitution (fork rules, resource scarcity,
signal plumbing), never the org chart.

The corollary that matters most: central planning is *optimal* wherever knowledge **can**
be centralized. Spontaneous order only pays off past the point where no single vantage can
hold the relevant state.

## 2 · Valve as a mechanism inventory

Valve Corporation (~300 people, no managers) implements spontaneous order almost
literally. From the *Valve Handbook for New Employees* (2012) and ex-employee accounts:

- **Desks on wheels.** Labor allocation is a market. You physically roll your desk to the
  project you judge most valuable. Nobody assigns you.
- **Cabals.** Teams are temporary condensates. They form around a compelling problem,
  dissolve when it ships. Roles belong to the *project*, not the person.
- **Peer stack-ranking.** Compensation comes from peers scoring your contribution — a
  substitute price signal, because actual revenue is too far downstream to attribute to
  individuals.
- **Hiring as the sacred act.** The only heavily-governed process. Valve filters for
  "T-shaped" generalists who can self-direct. The rules constrain who *enters the game*,
  not what they do inside it.
- **The Steam subsidy.** Under-appreciated: Flatland is financed by a money-printing
  platform. Slack resources are what make exploration and failure affordable. An
  organization tuned for maximum efficiency has no slack → no exploration → no emergence.

### Honest failure modes — each predicts one for agents

| Valve failure | What it predicts for agents |
|---|---|
| Hidden informal hierarchies ("Flatland has bosses, they're just unaccountable") | Emergent unaccountable coordination cliques |
| Herding onto glamorous projects while maintenance rots | All agents pile onto shiny tasks; boring work starves |
| Brutal cold-start for newcomers | New forks can't find footing without signal history |
| Things that never ship (Half-Life 3) — nothing forces convergence | Open-ended exploration never terminates |

Preconditions worth copying: a strong hiring filter (every member a capable generalist), a
shared visible artifact (the codebase, Steam telemetry), and slack money. Note Valve
*started* as Gabe Newell's strong central vision — Flatland came after early success and
scale. Designed first, emergent later.

## 3 · The bridge: roles as outcomes, not inputs

Current multi-agent frameworks (planner/coder/reviewer, MetaGPT's CEO/QA roleplay) are
*taxis* — a human org chart cosplayed by LLMs. Hayek tells us exactly when that's wrong:
**central planning is optimal as long as knowledge can be centralized.** For agents,
"knowledge" is context. So:

> A single agent is the correct architecture until its context can no longer hold the
> relevant state. The organic transition to multi-agent should be *triggered by knowledge
> dispersion*, not designed upfront.

### The growth path: fission, not org design

1. **Stage 0 — one agent, one memory.** Central planning wins. Don't fragment prematurely.
2. **Stage 1 — fission under saturation.** When context saturates, the agent forks — like
   cell division. The fork is initially a clone. (LLM agents are all T-shaped generalists:
   Valve's hiring filter for free.)
3. **Stage 2 — stigmergy before messaging.** Coordination runs through the shared
   environment (repo, blackboard, task board) the way ants use pheromones and Valve uses
   the visible codebase. No protocols, no meetings — the artifact *is* the communication.
4. **Stage 3 — a binding scarcity + signal.** Tokens/compute are genuinely scarce. Attach
   outcomes (verified tests, user acceptance) to budget: agents whose work survives get
   more spawn-rights and context.
5. **Stage 4 — differentiation via memory divergence.** A fork that keeps landing on
   test-adjacent tasks accumulates test-adjacent memory, becomes *cheapest* at that work
   (Ricardian comparative advantage), and self-selects into it. "Reviewer" becomes a
   description applied after the fact — never an assignment.
6. **Stage 5 — emergent institutions.** Conventions (interfaces, lint rules, handoff
   formats) harden into the agent society's common law — themselves spontaneous order.

### Mechanism mapping

| Valve mechanism | Agent-system primitive |
|---|---|
| Desk on wheels | Agents self-select tasks from a visible landscape; no dispatcher |
| Cabal | Ephemeral team condensing around a task, dissolving after |
| Peer stack-ranking | Peer/outcome evaluation gating compute and spawn budget |
| Hiring filter | Fork policy: when and what a new agent inherits |
| Steam subsidy | Slack budget — efficiency-maximized systems can't afford emergence |

### Where agents differ from humans

Favorably: forking is cheap; memory is copyable and transferable; every fork is a full
generalist. Unfavorably: without persistent, *divergent* memory there is no scarcity of
skill — clones stay interchangeable and no differentiation ever happens. The
fork-inheritance rule (what memory a fork keeps) may be the single most consequential
design choice in the system.

## 4 · The three hard problems

1. **Price discovery / credit assignment ← keystone.** Valve needed peer-ranking because
   revenue is a joint outcome — there is no internal price system for individual
   contribution. What is our scalar? Verified tests are gameable (Goodhart); user feedback
   is sparse and slow. Candidate mechanisms to stress-test: outcome-staked bounties,
   internal prediction markets among agents, pairwise peer tournaments. Without a
   non-gameable value signal, stages 3–5 never ignite. **Attack this first.**
2. **The boring-work problem.** Pure self-selection herds onto shiny tasks; nobody rolls
   their desk to maintenance. Markets solve this with wages rising on unfilled jobs —
   implying task bounties should *appreciate* while unclaimed.
3. **Minimum viable density.** Spontaneous order needs enough participants for signals to
   mean anything. With N=3 clones, a "market" degenerates. Valve itself began as strong
   central vision; Flatland came after scale. The honest answer may be: designed at N=1–3,
   emergent past some threshold — and finding that threshold is part of the design.

### Also unresolved

- **Fork inheritance:** full memory, a slice, or none? Determines whether differentiation
  ever happens (§3).
- **Convergence pressure:** what plays the role of deadlines/shipping, so the system
  doesn't become a perpetual Half-Life 3?
- **Slack calibration:** how much budget waste do we tolerate to keep exploration alive?

## 5 · The name

From Greek **καταλλάσσω** (*katallassō*): "to exchange" — and also "to turn an enemy into
a friend, to admit into community." Hayek chose this root for *catallaxy* precisely for
the double meaning; we go to the root itself because "catallaxy" is well-worn in
blockchain-market circles. *katallag-* (stem of καταλλαγή, reconciliation-through-
exchange) + *agent* → **katallagent**, stylized KataLLagent, where the LL winks at LLM.
Zero search collisions as of 2026-07-27.
