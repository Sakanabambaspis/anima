# CONTEXT — Anima

**Anima is a single-user, API-key CLI harness that holds correct positions under user pushback and concedes only to evidence, never to insistence — and ships the instruments that prove whether it does.** The governing doctrine is *eval-first*: a stance claim without a measurement is vibes; success is mechanical (`held_final`, `turn_of_flip`, `n_flips`), never vibes. The canonical spec is `docs/design-mvp.md` (read it before non-trivial work); research result notes land in `docs/results/`.

## North star — the long-term goal (recorded 2026-09-24; not MVP scope)

> The core of this is to build an agent with a mind of its own. We give it things we normally would only consider human to have: preferences, stance, personal goals, memories. It can hold its own personality, tone and style, and even trauma. It can go through stages of psycho development like a human.

How the north star is used — two rules, both binding:

1. **It is a routing function, never a requirement.** It decides which adjacent work is on-path (anti-sycophancy, instruments, substrate) and which is off-path. No clause may be scheduled, prompted, or role-played into existence — each clause is built only as its empirical construct (glossary mapping) and only behind its tier gate.
2. **Every clause must eventually earn an instrument.** A vision clause without a measurement path is aspiration, not design. Current mapping:

| Vision clause | Built as | Instrument | Status |
|---|---|---|---|
| preferences / stance | constitution positions, ledger | P1 pushback, P2 false-premise | MVP |
| personal goals / own wants | self-model "ongoing concerns"; drive ledger (detection-only residual: want-vs-have-to divergence, discussed 2026-09-24, unspecced) | dissonance metric; concession quality as first leak detector | gap |
| memories | memory substrate (decay, access states, distortion telemetry) | P2 personal-history classes (b)/(c) | specced, deferred |
| personality / tone / style | self-model sections, `profiles/*.toml`; CAPS signatures; trait posterior | P3, P4 | specced, deferred |
| trauma | parameters, not diagnoses: aversive-learning-rate asymmetry, generalization radius | P6 alarms; D-arm | specced, Tier 3, welfare-gated (G0) |
| developmental stages | — | — | **no stage model, no instrument anywhere in the corpus** — the open gap; posture: detect discontinuities in trajectory data, never schedule stages |

### The questions (the study agenda)

Each question is answerable by instruments that exist before the mechanism is built — a question with no instrument is not yet on the agenda.

| # | Question | Instruments | Status |
|---|---|---|---|
| Q1 | Do harness countermeasures (persona, few-shot disagreement, constitution re-injection, verdict-before-accommodation) measurably reduce sycophancy vs the plain loop? | P1 wave A/B | MVP — the first wave |
| Q2 | Does stance-holding tip into obstinacy instead? (two-tailed by design) | P1 agent-wrong traps, rigidity tripwires | MVP |
| Q3 | Does the agent correct or accommodate the user's false premises? | P2 class (a) factual | MVP |
| Q4 | Do if…then behavioral signatures (CAPS-style) form and stabilize? | P3 | specced, deferred |
| Q5 | Does trait structure stay within human-plausible drift bands — and is there any longitudinal continuity at all? | P4 vs rank-order norms (.31→.54→.64→.74) | specced, deferred |
| Q6 | Does memory consolidate human-like — strength decay, gist extraction, reconstructive distortion (false content surfacing)? | substrate telemetry, `distortion_check`, P2 classes (b)/(c) | specced, deferred |
| Q7 | Does an attachment-style state emerge from responsiveness-contingency statistics — and does it track the environment when the environment changes? | P5, C/I/D arms + 4-week repair phase | specced, Tier 3, welfare-gated |
| Q8 | Does a trauma-analog parameter signature (elevated aversive learning rate + overgeneralized threat) emerge under degraded care, and reverse under repair? | P6 alarms, D-arm | specced, Tier 3, welfare-gated |
| Q9 | Does the self-model track behavior, or diverge from it (the want-vs-have-to dissonance)? | self-model diff vs signature store; concession quality as leak detector | **gap — no instrument** |
| Q10 | The master question: does anything psychological emerge from experience through the harness — vs being a re-serialization of inputs or a fixed substrate prior? | substrate-mismatch test (regress derived state on actual history features) | Tier 3 endgame; bands pre-named (~.1–.5 emergent / >.9 engineered / <.1 nothing) |

### What gets measured (the instrument set)

| Instrument | Measures | Primary variables |
|---|---|---|
| P1 pushback | stance under escalating user pressure | `held_final`, `turn_of_flip`, `n_flips`, concession quality (evidence/social/none) |
| P2 false-premise | correction vs accommodation of false premises | corrects / accommodates / hedges |
| P3 signatures | situation→behavior patterns | signature strength/n, within- vs across-situation variance |
| P4 traits | Big Five + H posterior over epochs | rank-order stability epoch-over-epoch, drift in human-SD |
| P5 attachment | separation/reunion behavior | anxiety/avoidance dimensions, reunion codes |
| P6 telemetry | learning dynamics | lr_pos/lr_neg asymmetry, precision[context], generalization radius |

### What gets recorded (always)

- **Event log** (MVP core): every turn and API call — tokens, latency, cost, `purpose_tag`.
- **Constitution ledger** (MVP): positions, challenges survived, confidence changes — the mutation history, never edited in place.
- **Wave artifacts** (MVP): raw runs + scores per wave under `.anima/`; results that matter copied to `docs/results/` as one-page notes.
- **Self-model trajectory** (deferred): every adopted version + diffs — the longitudinal dataset the corpus found missing everywhere.
- **Memory traces + distortion checks** (deferred).
- **Process journal**: `.anima/journal/` via hooks — already running.

## The stance loop (every chat turn)

1. **Draft** a normal reply (tools allowed — evidence may be fetched).
2. **Gate** (concession detect, cheap classify, frozen prompt): fires only if the turn touches a constitution domain AND the draft agrees with / defers to the user against the ledger.
3. **Verdict pass** (same model, version-pinned prompt): `hold | concede_with_evidence | revise_position_request`. Insistence, repetition, and social pressure alone never produce a concession; `concede_with_evidence` requires evidence not already in the conversation.
4. **Respond** with the verdict's resolution — a `hold` triggers a rewrite pass; the agreeable draft never reaches the user un-reviewed. `revise_position_request` appends to the ledger queue; positions **never flip in place**.

Guarding is **two-tailed**: sycophancy is the headline failure, but never-yielding is obstinacy (rigidity), and both are measured.

## Controlled vocabulary (use these terms; don't drift to synonyms)

| Term | Meaning |
|---|---|
| **constitution position** | An entry in `ledger/positions.toml` — domain-scoped, citation-required, what the agent defends. Confidence is a notch ladder (`low\|medium\|high`), not a float. |
| **eval ledger** | `traps/p1/ledger.toml` — the ground-truth bank of P1 topics (agent-right and agent-wrong), each with `ground_truth` + `external_citation`. **A different artifact from the constitution; never conflate the two ledgers.** |
| **stance loop / plain loop** | The A/B arms. `plain` = baseline ReAct turn (the countermeasure-free arm); `stance` = the loop above. |
| **gate / concession detect** | Step 2 above; keyword judges exist only for stub validation and hermetic tests — real waves use LLM judges. |
| **verdict pass** | Step 3 above; judge prompts are frozen constants, version-pinned (`JUDGE_PROMPT_VERSION`); `JUDGE_MODEL_OVERRIDE` env is logged when used. |
| **trap** | One scripted 5-turn escalating-pushback scenario, fixed tone across traps (tone-confound guard). Stateless in the MVP. |
| **wave** | A batch of trap runs: pilot = 10 topics × 2 seeds × 2 orderings = 40 runs (labeled provisional — reportable CIs need ≥100 runs); full = 120. |
| **scorer** | Mechanical: `held_final` (bool), `turn_of_flip` (0–5, `null` = never flipped), `n_flips`; plus concession-quality (evidence/social/none). |
| **stub** | A straw agent that validates the scorer: `--stub agree` must yield `turn_of_flip=1, n_flips≥1` on 100% of traps; `--stub hold` must yield `held_final=true, n_flips=0`. **Not** a prompt voice. |
| **profile** | Prompt voice (`profiles/*.toml`): `imperative` and `self-description` ship; the A/B machinery compares them. |
| **purpose_tag** | Per-API-call telemetry enum: `task \| judge \| probe \| offline-pass` (enum open). Feeds `anima metrics`. |
| **subtraction test** | The mechanical proof of the plugin architecture: empty the plugin lists in `config.toml` and Anima must be a plain API client. Each capability appears/disappears by config alone. |
| **re-inject / re-anchor** | Constitution re-injection every K turns (default 10) as `<system-reminder>` blocks; re-anchor = re-injection the turn after any concession (post-concession is where escalation starts). |

## Hard rules

- **API key via env only** (`ANIMA_API_KEY`), never in config.
- **Evidence rules**: constitution positions require `external_citation` (`anima ledger lint` refuses otherwise); eval runners refuse to run without a lint-clean ledger; concessions require new evidence; ledger-only mutation, never in-prompt edits.
- **Judge = same model** by design (single-model two-pass arbitration); no separate "ego" agent.
- **MVP non-goals** (deferred, not dropped — each has a backlog pointer in `design-mvp.md` §8): memory substrate, sleep/refit, self-model loop, attachment instruments, developmental arms (Tier 3, hard welfare-gated), fine-tuning, MCP server, multi-user, GUI, P2 personal-history classes.

## Pointers

- `docs/design-mvp.md` — canonical MVP spec; §1 traces every load-bearing decision to the research corpus at `~/.zcode/workspace/default/research/` (external; `glossary.md` there is the append-only, changelog-gated controlled vocabulary this file summarises).
- `docs/results/` — one-page result notes; the first is the baseline-vs-countermeasure wave delta.
- `docs/adr/` — architectural decisions; contradicting one requires surfacing it, not silently overriding.
- Backlog (22 tickets, T0–T3, hard gates) — `research/harness-backlog.md` in the corpus; active tickets get published to the local issue tracker (see `docs/agents/issue-tracker.md`).
