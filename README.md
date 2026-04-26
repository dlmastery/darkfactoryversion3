# Dark Factory v3

A governed, evidence-driven AI software factory implemented as a constellation of Claude Code skills.

> **Zero-Slop Compliance.** Every claim must be project-specific, evidence-backed, trace-linked, reviewer-challenged, and free of unsupported invention. If evidence is missing, mark it as an assumption, risk, open question, or failure.

---

## What this is

Most AI coding agents take an instruction and produce an answer. A factory does something different: it turns intent into a **governed run** with stages, gates, owners, evidence, and traceability — and refuses to advance when evidence is missing.

Dark Factory v3 is that factory, packaged as 14 cooperating skills. Each skill is a focused agent module with its own templates, validators, and review rubrics. The orchestrator (or the meta-attractor above it) routes work between them based on what the current node needs.

The system is designed for:
- **Greenfield projects** — turning raw intent into a project book, requirements, design, code, tests, and operations artifacts.
- **Brownfield work** — recon and impact analysis before touching an existing system.
- **Artifact-only or planning runs** — when you need governed BRDs, SRSs, ADRs, runbooks without writing code.
- **Meta-skill engineering** — designing or revising the factory itself.

---

## Architectural shape

```
                        ┌───────────────────────────┐
                        │     df-meta-attractor     │   meta-meta layer
                        │  (chooses the factory)    │   intent → governed run definition
                        └─────────────┬─────────────┘
                                      │
                        ┌─────────────▼─────────────┐
                        │ dark-factory-orchestrator │   orchestrator
                        │   (chooses the node)      │   selects child skills, enforces gates
                        └─────────────┬─────────────┘
                                      │
       ┌───────────────────┬──────────┼──────────────────┬─────────────────┐
       │                   │          │                  │                 │
┌──────▼─────┐  ┌──────────▼──┐  ┌────▼──────┐  ┌────────▼────────┐ ┌──────▼──────┐
│ governance │  │    intake   │  │ artifact  │  │   methodology   │ │ traceability│
│   mayor    │  │   spec lab  │  │  factory  │  │     blender     │ │   evidence  │
└────────────┘  └─────────────┘  └───────────┘  └─────────────────┘ └─────────────┘

┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐  ┌──────────────┐
│  brownfield  │  │    swarm     │  │   quality    │  │   context /    │  │  human-agent │
│    recon     │  │ coordination │  │   refinery   │  │    memory      │  │   handoff    │
└──────────────┘  └──────────────┘  └──────────────┘  └────────────────┘  └──────────────┘

                  ┌─────────────────────────┐  ┌────────────────────────┐
                  │  production / SRE       │  │    feedback /          │
                  │       handoff           │  │     learning           │
                  └─────────────────────────┘  └────────────────────────┘
```

The two-tier split is the defining choice: the **attractor** decides *what factory to run* (mode, gates, evidence model, methodology blend), while the **orchestrator** decides *what node to execute next* inside that factory.

---

## The 14 skills

| Skill | Role |
| --- | --- |
| [`dark-factory-orchestrator`](dark-factory-orchestrator/SKILL.md) | Picks child skills, creates node contracts, enforces evidence gates, keeps humans able to take over. |
| [`df-meta-attractor`](df-meta-attractor/SKILL.md) | Sits above the orchestrator. Pulls scattered intent into a stable attractor state — goal, scope, gates, evidence model, next safe action. Required before serious governed runs. |
| [`df-governance-mayor`](df-governance-mayor/SKILL.md) | The process constitution. Standards baseline (ISO 12207/15289, SSDF, OWASP SAMM), RASCI, stage gates, change control, engagement governance, token-budget SWAGs, Hawkeye conformance. |
| [`df-intake-spec-lab`](df-intake-spec-lab/SKILL.md) | Turns blurry intent into spec — interrogation rounds, recursive decomposition, acceptance criteria, scenario seeds, holdouts, transfer-test candidates. |
| [`df-brownfield-recon`](df-brownfield-recon/SKILL.md) | Maps an existing repo before any change. Inventory, conventions, ownership, risky areas, change-impact record. |
| [`df-artifact-factory`](df-artifact-factory/SKILL.md) | Generates the project book of knowledge — BRD, SRS, NFR catalog, HLD, LLD, ADRs, MDA CIM/PIM/PSM, DDD maps, test plans, trace matrices, release notes, runbooks, incident guides, certificates. |
| [`df-methodology-blender`](df-methodology-blender/SKILL.md) | Composes RUP, MDA, DDD, TDD, BDD, Agile, SRE, ISO lifecycle, SSDF — into one coherent workflow with concrete artifacts and gates, not ceremony theater. |
| [`df-swarm-coordination`](df-swarm-coordination/SKILL.md) | Runs multi-expert work — persona contracts, independent proposals, cross-critique, alternative synthesis, AI judge/jury for material transitions. |
| [`df-quality-refinery`](df-quality-refinery/SKILL.md) | The toughest gate. Triple expert review with artifact-specific 3-critic panels, ≥2 adversarial critics, ≥5 RALPH loops, 18 artifact-level + 15 per-seat rubric checks. |
| [`df-traceability-evidence`](df-traceability-evidence/SKILL.md) | Bidirectional link graph. Forward and reverse trace from intent → requirement → design → code → tests → release → operations, with a typed knowledge graph and validators. |
| [`df-context-memory`](df-context-memory/SKILL.md) | Externalizes memory to fight context rot. Compact context packs, predecessor recovery, fresh-session replay, human review/onboarding portals. |
| [`df-human-agent-handoff`](df-human-agent-handoff/SKILL.md) | Bidirectional handoff. State + rationale + open risks + the next safe action — every time ownership swaps. |
| [`df-production-sre-handoff`](df-production-sre-handoff/SKILL.md) | Release readiness, runbooks, rollback paths, observability, incident response, outage drill records, operator readiness evidence. |
| [`df-feedback-learning`](df-feedback-learning/SKILL.md) | Turns feedback (reviews, incidents, retros, gate failures) into requirements, scenarios, rubrics, templates, and process updates instead of letting them disappear into chat. |

---

## Repository layout

Every skill module is a self-contained Claude Code skill bundle with a uniform shape:

```
df-<module>/
├── SKILL.md                # frontmatter + workflow + rules + gate list
├── agents/                 # agent role definitions (e.g. openai.yaml)
├── assets/
│   └── templates/          # JSON record templates for governed evidence
├── references/             # protocols, rubrics, routing maps, checklists
└── scripts/                # Python validators (no external deps)
```

That uniformity is intentional: the orchestrator can route to any module without special-casing, and validators can be invoked the same way everywhere.

### Stats

- **14 skill modules** + 1 orchestrator
- **~259** Markdown documents (workflow, references, rubrics, protocols)
- **~194** JSON record templates (Attractor Run Records, TPM flow ledger, PERT plan, knowledge graph nodes, judge/jury verdicts, handoff packages, certificates)
- **~32** Python validators (zero external dependencies)
- **~8 MB** of factory definition

---

## Governed evidence model

Material work in a governed run cannot proceed without a chain of evidence:

| Record | Purpose |
| --- | --- |
| **Attractor Run Record** | Why this factory, in this mode, with these gates and evidence. |
| **Engagement Governance Record** | Client owner, delivery owner, token SWAG, iteration plan, change-control policy. |
| **Knowledge Graph + TPM Flow Ledger + PERT Plan** | Typed nodes for requirements, artifacts, evidence, gates, handoffs; flow state and dependencies. |
| **Strict `TASKS.md` bead ledger** | Every material work unit has a state, owner, control node, evidence, token posture, next bead. |
| **AI Judge/Jury Record** | TPM Judge, Evidence Clerk, specialist jurors, adversarial prosecutor, foreperson verdict — for every material transition. |
| **Execution-Kernel Preflight** | Cross-validates `TASKS.md`, TPM flow, PERT, knowledge graph, judge/jury before choosing the next action. |
| **SDLC Stage Coverage Matrix** | Every stage accepted / waived / deferred / N/A with rationale; required testing classes evidenced. |
| **Hawkeye Conformance Audit** | Veto power over skipped stages, stale evidence, generic tailoring, template-as-proof. |
| **Artifact Review Panel + RALPH Loop Records** | Artifact-specific 3-critic panel + adversarial critics + ≥5 review-attack-learn-patch-harden loops. |
| **Human Review and Onboarding Portal** | Dashboard, machine-readable portal index, diagram atlas, role-based reading paths — so a fresh reviewer can recover state without reading the repo linearly. |
| **Production Handoff + Outage Drill** | Deploy, verify, rollback, observability, incident path, operator readiness. |

Default thresholds:

- Governance compliance ≥ **98%**
- Selected expert review ≥ **96%** per expert
- Mandatory traceability **100%** or explicit deferral
- Critical / high security findings: **zero unresolved**
- Production readiness: owner, deploy, rollback, observability, incident path, known risks all present

---

## How a run flows

1. **Attract.** `df-meta-attractor` converts raw intent into a governed run definition — operating mode, methodology blend, evidence model, generated meta-skill contract.
2. **Govern.** `df-governance-mayor` baselines standards, RASCI, gates, engagement governance (client/delivery owners, token SWAG, iteration plan, change-control).
3. **Intake.** `df-intake-spec-lab` interrogates intent and decomposes spec until every leaf has owner, acceptance criteria, validation method, and trace targets.
4. **Recon (brownfield only).** `df-brownfield-recon` produces a change-impact record before any code is touched.
5. **Build.** `df-artifact-factory` produces the project book; `df-methodology-blender` keeps RUP/MDA/DDD/TDD/BDD/SRE coherent; `df-swarm-coordination` runs expert debates and AI judge/jury for material transitions.
6. **Refine.** `df-quality-refinery` applies artifact-specific critic panels, adversarial critics, RALPH loops, and rubric scoring before anything is accepted.
7. **Trace.** `df-traceability-evidence` maintains the link graph and the knowledge graph; validators fail the gate if links are missing.
8. **Hand off.** `df-human-agent-handoff` packages state for ownership swaps; `df-production-sre-handoff` produces release readiness and operator evidence.
9. **Remember & learn.** `df-context-memory` keeps long runs fresh; `df-feedback-learning` converts review/incident feedback into rubric, template, and process updates.

The loop is `intake → build → refine → trace → handoff → learn`, with the attractor and governance gates wrapping all of it.

---

## Operating rules (the short version)

- Do not claim zero mistakes. Build a zero-defect *posture* through redundant review, evidence, and residual-risk records.
- Do not start material work without an Attractor Run Record, engagement governance, task bead, TPM flow step, and PERT dependency node — or explicit waivers with owner, rationale, expiry, and residual risk.
- Code-producing work requires implementation and test evidence. UI work requires browser/WYSIWYG evidence. Scenario-driven work requires scenario, holdout, and transfer evidence. Production work requires release/ops evidence. **Documents do not substitute for the work.**
- Methodology labels (ISO, RUP, MDA, DDD, TDD, BDD, SRE, SSDF) must map to concrete artifacts and checks — not decoration.
- Templates are not proof. Validators reject template-as-evidence for instantiated runs.
- Keep the user-facing conversation simple. Store technical evidence in files.

---

## Using the skills

Each module is a standalone Claude Code skill. With the Claude Code CLI, you invoke a skill by reference and Claude loads its `SKILL.md` plus only the references the workflow currently needs (a context-rot mitigation).

A typical entry point for a serious run:

```
1. Invoke df-meta-attractor          # produce Attractor Run Record + tailoring
2. Invoke df-governance-mayor        # establish gates, RASCI, engagement governance
3. Invoke dark-factory-orchestrator  # routes the rest
```

For a quick artifact-only or review-only task you can skip straight to `dark-factory-orchestrator` and let it route.

The Python validators in each `scripts/` directory have **no external dependencies** and can be run on a vanilla Python 3 install:

```bash
python df-governance-mayor/scripts/validate_tasks_md.py path/to/TASKS.md
python df-traceability-evidence/scripts/validate_trace_links.py --strict ...
python df-meta-attractor/scripts/validate_meta_meta_skill_contract.py ...
```

---

## License & status

This repository is the working definition of the Dark Factory at version 3. It is intentionally opinionated, intentionally heavy on governance, and intentionally hostile to AI slop. Use it where you need software that someone could actually own afterwards.
