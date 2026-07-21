# AKM Index v1.1 — Agentic Knowledge Management Evaluation Framework

> **One-line Summary** — A universal evaluation scale that measures **how well an individual's or organization's knowledge management system is designed and operated to run together with AI agents** — 5 pillars × 25 criteria, scored out of 100. Every score requires artifact evidence (files, configurations, logs).

---

## 1. Purpose and Scope

### 1.1 What It Measures

The AKM Index does not measure "how well you write notes." It measures **how mature the system is by which AI agents read, write, and maintain that knowledge base**. A fine Zettelkasten scores low on AKM if agents cannot access it; rough notes can score high if the agent pipeline is robust.

The unit of measurement is the **system** (person + knowledge repository + agent runtime + operating loops) — not individual tools and not individual notes.

### 1.2 Minimum Requirements for Evaluation

All three of the following must exist for a system to be evaluable. If any one is missing, record the system as "Not Evaluable (Pre-Level)."

1. **A knowledge repository** — a persistent place where files, notes, and documents accumulate (Obsidian vault, Notion, code repository, wiki, etc. — format-agnostic)
2. **At least one agent runtime** — an LLM agent environment with access to that repository (Claude Code, Codex, Gemini CLI/Antigravity, Cursor, OpenClaw, Hermes, self-built, etc.)
3. **Intent of repeated use** — sustained operation rather than a one-off experiment (traces of use within the last 30 days)

### 1.3 Design Principles

- **Behavioral anchors (BARS)**: every criterion distinguishes levels by observable behaviors and artifacts — a behaviorally anchored rating scale, never a "vibe" rating.
- **Evidence required**: to award a score of 1 or higher, at least one artifact of evidence (file path, configuration content, log, dated deliverable) must be cited per criterion. Self-report alone ("I review every week") caps at 1 point.
- **Tool-neutral**: no specific product (Obsidian, Claude, etc.) is presupposed. The examples in the anchors are examples only.
- **Reproducible**: a different rater, given the same evidence, must arrive at the same score within ±1 level (inter-rater reliability).

---

## 2. Framework Structure

### 2.1 The Five Pillars and Their Weights

| Pillar | Name | Core question | Weight |
|---|---|---|---|
| **P** | Prompt Engineering (the prompt layer) | Are the instructions given to agents managed as assets? | 20% |
| **C** | Context Engineering (the context layer) | Can agents find "the right knowledge" in "the right amount"? | 25% |
| **H** | Harness Engineering (the harness layer) | Are runtime tools, permissions, memory, and orchestration in order? | 20% |
| **L** | Loop Engineering (the loop layer) | Do the human+agent cycles actually run, and compound? | 20% |
| **X** | Interop & Governance (portability & governance) | Does the system port across runtimes and devices, with recovery and security assured? | 15% |

> **Rationale for the weights** — C (Context) is highest at 25% because supplying context is the very reason a knowledge management system exists; the other pillars only carry meaning if C is present. X is lowest at 15% so the scale stays fair to individuals who use a single environment — yet because scoring X at 0 caps the total at 85 points, "multi-environment operation works as loss prevention, not as a bonus." These weights are the v1.0 defaults; they may be adjusted for a given evaluation purpose, but any adjustment must be disclosed in the report.

### 2.2 The Common Maturity Ladder (applied identically to every criterion)

| Level | Name | General definition |
|---|---|---|
| **0** | Absent | The concept does not exist or is not recognized |
| **1** | Ad-hoc | Exists but undocumented. Done by hand, case by case. Not reproducible |
| **2** | Documented | Rules and structures exist as documents, but compliance depends on human willpower. Partially applied |
| **3** | Systematized | Tools, automation, and hooks **enforce the rules or make them the default**. Breaking the rules is harder than following them |
| **4** | Self-improving | The system itself is measured, and there is a **recorded case** of that feedback leading to updated rules, prompts, or structures |

> **Evidence standard for Level 4** — Level 4 does not mean "lots of automation." It requires **"traces of the system fixing the system"** — e.g., a commit where a retrospective led to a CLAUDE.md revision, a record of an agent failure log leading to a new hook, a quarterly prompt audit document. Without such traces, the ceiling is 3 no matter how sophisticated the setup.
>
> **Two forms of observation (v1.1)**: even where an individual anchor exemplifies "measurement, ratios, or frequencies," the essence of Level 4 is a **recorded closed feedback loop**. Quantitative metrics (write-back rate, hit rate, etc.) and **dated qualitative records** (incident→cause→fix→prevention chains, recorded retrospectives leading to revisions) are accepted as equal evidence. For single-person systems in particular, N incident-based improvement records are equivalent to periodic instrumentation — building dashboards nobody will read is not what this rubric asks for.

### 2.3 Score Calculation

- Per-criterion score: 0–4
- Per-criterion allocation: pillar weight ÷ 5 (each P·H·L criterion at full marks = 4.0%, each C criterion = 5.0%, each X criterion = 3.0%)
- Criterion converted score = (level ÷ 4) × criterion allocation
- **Total = Σ criterion converted scores (0–100)**

| Total | Maturity band | Characteristics |
|---|---|---|
| 0–20 | **M0 Manual** | The agent is used like a search box. No system |
| 21–40 | **M1 Tooling** | Individual automations exist but are not connected to each other |
| 41–60 | **M2 Structured** | A documented schema + agent access paths exist |
| 61–80 | **M3 Orchestrated** | Multi-agent, hooks, and loops actually run; enforcement mechanisms exist |
| 81–100 | **M4 Compounding** | The system measures and improves itself. The baseline rises with every session |

---

## 3. Pillar P — Prompt Engineering (20%)

**Definition**: the degree to which the instructions given to agents (system directives, skills, commands, personas) are designed and operated as **version-controlled assets** rather than one-off chat messages.

### P1. Instruction Layering & Scoping

Core question: Are instructions divided into global/project/directory scopes so that the right instruction loads in the right place?

| Level | Anchor |
|---|---|
| 0 | Instructions are re-typed into the chat every session |
| 1 | A collection of reusable prompts exists (notes app, snippets) but is manually copy-pasted |
| 2 | One or more instruction files (CLAUDE.md/AGENTS.md/GEMINI.md, etc.) exist, but as a single monolithic file or without scope separation |
| 3 | Global ↔ project ↔ directory scopes are separated with clear precedence. Domain-specific instructions are split into skills/commands and loaded only when needed |
| 4 | A procedure for auditing duplication and conflicts across the instruction hierarchy (audit skill, periodic review) exists, with execution records |

Evidence examples: a list of instruction files with each file's scope, the skill directory structure, instruction audit records.

### P2. Task Contracts

Core question: Do the instructions for recurring tasks specify inputs, outputs, save locations, format, and tone at contract level?

| Level | Anchor |
|---|---|
| 0 | Only "just tidy this up for me"-level instructions are used |
| 1 | Some instructions mention output format, but only sporadically |
| 2 | For key recurring tasks, output format and save paths are documented (e.g., "save results to folder X with frontmatter included") |
| 3 | Skills/commands specify input conditions, an output schema (frontmatter fields, sentence endings and tone, filename rules), save paths, and failure handling. Template files are bundled |
| 4 | A validation step that detects contract violations (a format linter, a reviewer agent) is built into the pipeline, and there are cases of violation → contract revision |

Evidence examples: the output-spec section of a SKILL.md, template files, format-validation hooks.

### P3. Trigger & Routing Design

Core question: Among dozens of instruction assets, are triggers designed so the right one is selected automatically at the right moment?

| Level | Anchor |
|---|---|
| 0 | Only 1–2 instruction assets exist, so the notion of routing is itself unnecessary |
| 1 | Many assets exist, but the user must remember names and invoke them manually every time |
| 2 | Asset descriptions state their purpose, but without trigger phrases or counter-examples. Frequent misfires and non-fires |
| 3 | Descriptions are written for machine routing: trigger phrases (Korean/English), negative triggers ("when NOT to use this"), and differentiators from similar assets are explicit |
| 4 | Routing failures (wrong skill fired, skill not fired) are logged and collected, with cases of description revisions that followed |

Evidence examples: trigger phrases in skill descriptions; differentiator phrasing of the "For X use Y instead" kind.

> **Equivalent path (v1.1)**: systems whose governance forbids editing descriptions directly (e.g., an immutable-originals policy for imported assets) may present a **central routing registry** — triggers, negative triggers, and differentiators managed outside the originals and consulted by the agent — as equal evidence. Anchor examples are examples only (§1.3 tool neutrality).

### P4. Exemplars & Counter-examples

Core question: Are instructions backed not only by abstract rules but by actual examples (a corpus of one's own writing style, good/bad output pairs)?

| Level | Anchor |
|---|---|
| 0 | No examples |
| 1 | Examples are given ad hoc in chat ("like this") but never saved |
| 2 | One or two representative examples are included in the instruction files |
| 3 | A style corpus (collection of published writing, a voice guide), good-output/bad-output pairs, and edge-case examples are managed as assets and referenced from instructions |
| 4 | A procedure exists by which new deliverables join the corpus (e.g., published articles are automatically added to style references), plus records comparing deliverable quality against the corpus |

Evidence examples: the publication corpus referenced by a style skill, few-shot example files.

### P5. Prompt Asset Lifecycle

Core question: Are instruction assets managed like code — with versioning, change history, deduplication, and retirement?

| Level | Anchor |
|---|---|
| 0 | No lifecycle. Assets only accumulate |
| 1 | Old prompts are mixed in, and which one is current is unclear |
| 2 | Versions are distinguished by filename or date, but the reasons for changes are not recorded |
| 3 | History is managed in a VCS such as git; change rationale is traceable via a CHANGELOG or commit messages; a single-source-of-truth (SSOT) principle and synchronization procedures exist |
| 4 | Periodic audits (retiring unused assets, consolidating duplicates) run on a schedule and leave audit reports |

Evidence examples: git history of instruction files, a CHANGELOG, a system-docs synchronization skill.

---

## 4. Pillar C — Context Engineering (25%)

**Definition**: the degree to which the knowledge base is designed so agents can **find the knowledge they need (retrieval), understand it (structure), and receive it in an amount they can handle (budget)**.

### C1. Taxonomy & Addressability

Core question: Is classification and naming stable and predictable enough that an agent can "guess" a path?

| Level | Anchor |
|---|---|
| 0 | No folders, or a meaningless jumble |
| 1 | Folders exist, but naming is inconsistent and the same topic is scattered across multiple places |
| 2 | Numbering/naming conventions are documented (Johnny Decimal, PARA, a home-grown scheme, etc.). Exceptions occur frequently |
| 3 | The conventions are stably followed, and the agent instruction files include a "what lives where" map so agents can pinpoint paths without exploration |
| 4 | A procedure exists so that structural changes update the map and the instruction files together (sync skills/hooks), and checks that detect structure violations run |

### C2. Machine-readable Metadata

Core question: Are frontmatter, links, and types defined as a schema and actually applied consistently?

| Level | Anchor |
|---|---|
| 0 | No metadata |
| 1 | Tags/properties are used, but notation varies (three different tags for the same concept) |
| 2 | A frontmatter schema and tag dictionary are documented. Applied to new notes, while the legacy corpus is neglected |
| 3 | Templates/skills auto-inject the schema; required fields per note type are defined; link rules (wikilink target priority) are explicit |
| 4 | Schema-violation detection (a linter, an audit agent) runs periodically, and there is a record of the schema itself being revised based on usage data |

### C3. Retrieval Infrastructure

Core question: Can agents query the entire knowledge base through lexical + semantic search?

| Level | Anchor |
|---|---|
| 0 | Agents have no way to search the knowledge base (they can only open files directly) |
| 1 | Only filesystem-level grep/glob search is possible |
| 2 | In-app search (Obsidian search, etc.) is strong but not exposed to agents, or agent-facing search covers only some folders |
| 3 | Hybrid lexical + semantic (vector) search is exposed to agents via MCP/CLI, covers most of the knowledge base (80%+), with automatic index refresh |
| 4 | Search quality is measured (failed-query collection, recall checks), with a record of the indexing strategy and collection design being revised accordingly |

### C4. Progressive Disclosure & Context Budget

Core question: Is knowledge layered so agents read "only as much as needed" rather than "everything"?

| Level | Anchor |
|---|---|
| 0 | Agents are fed monolithic documents or full dumps |
| 1 | Large documents are manually chopped up and provided |
| 2 | Index/summary documents exist but are not wired into agent workflows |
| 3 | An index → core summary → detailed source hierarchy is designed, and instructions/skills explicitly prescribe reading in that order (e.g., maintaining LLM-facing distilled context separate from human-facing documents) |
| 4 | A record of the hierarchy being redesigned on the basis of context consumption and accuracy (summary refresh cadence, documented token budgets) |

### C5. Provenance & Freshness

Core question: Are originals and derivatives distinguished, and can the agent tell which version is the current canonical one?

| Level | Anchor |
|---|---|
| 0 | Originals, summaries, and copies are jumbled together; not even the human knows which is canonical |
| 1 | Canonical-version knowledge exists only in the human's head |
| 2 | The separation between an immutable source layer and a derivative layer is documented. Date fields exist |
| 3 | Derivatives back-reference their originals via `source:` or similar; priority rules for multi-copy conflicts (which side is the winner) are codified; update dates are tracked in metadata |
| 4 | A record of automated staleness detection (flagging outdated derivatives, alerts to refresh derivatives when a source changes) |

---

## 5. Pillar H — Harness Engineering (20%)

**Definition**: the degree to which the agent runtime (tools, permissions, memory, orchestration) is engineered to operate **safely and powerfully** in combination with the knowledge base.

### H1. Tool Surface

Core question: Are tools (MCP/CLI/API) in place so agents can read + write + manipulate the knowledge base?

| Level | Anchor |
|---|---|
| 0 | Agent and knowledge base are separate (connected only by copy-paste) |
| 1 | Filesystem access only |
| 2 | Generic tools are used (file access + some app CLI), but knowledge-base-specific operations (property edits, link queries) are impossible |
| 3 | Knowledge-base-specific tools (a dedicated MCP server, the full app CLI) enable reads, writes, metadata manipulation, and link-graph queries. Where gaps exist, custom tools are built to fill them |
| 4 | Tool usage statistics and failures are observed, with a record of the tool surface being expanded or retired accordingly |

### H2. Skill & Automation Library

Core question: Do recurring workflows accumulate as reusable skills/commands that write their results back into the knowledge base?

| Level | Anchor |
|---|---|
| 0 | No skills |
| 1 | 1–5 simple scripts/commands |
| 2 | Multiple skills exist, but mostly generic utilities unrelated to the knowledge base; outputs get saved anywhere |
| 3 | A domain skill library (10+) knows the knowledge base's rules, paths, and schema, and writes results back to their proper locations. A meta-skill for creating skills exists |
| 4 | Skill usage frequency and success rates are observed, with records of consolidation, retirement, and revision. When a new workflow emerges, turning it into a skill is the default reflex |

### H3. Guardrails & Permissions

Core question: Are destructive agent actions defended by hooks, permissions, and sandboxing?

| Level | Anchor |
|---|---|
| 0 | Full permissions granted (permanent YOLO mode) + no backups |
| 1 | Relies solely on manual approval of every action (mindless rubber-stamping induced by approval fatigue) |
| 2 | Allow/deny lists are configured. A list of dangerous operations is documented |
| 3 | Lifecycle hooks (blocking destructive commands, protected paths, format enforcement) are actually configured, enforcing the rules as code. The attack surface is documented |
| 4 | Incidents and near-misses are recorded and have led to guardrail updates. Periodic security reviews are scheduled |

### H4. Persistent Memory & State

Core question: Do learnings and context survive the end of a session, so the next session starts on top of them?

| Level | Anchor |
|---|---|
| 0 | Every session starts from a blank slate |
| 1 | The user manually summarizes and pastes in prior context every time |
| 2 | Memory files exist but pile up without structure (stale memories left unattended) |
| 3 | Structured memory (an index + per-item files, type distinctions), session handoff procedures, and rules for correcting or deleting bad memories exist |
| 4 | Memory hit rates and stale entries are checked, with a record of cleanup being executed periodically |

### H5. Orchestration & Observability

Core question: Are subagents, background jobs, and schedules put to use — and is it possible to trace what happened?

| Level | Anchor |
|---|---|
| 0 | A single chat thread only |
| 1 | Occasionally opens parallel chat windows, nothing more |
| 2 | Uses subagents/background runs, but arbitrarily; logs are not checked |
| 3 | Role-differentiated agent definitions (researcher, reviewer, etc.), workflow/pipeline composition, scheduled runs, and traceable paths for session logs and outputs exist |
| 4 | Orchestration costs and outcomes are recorded and compared, with cases of the configuration being revised (e.g., a record of a pipeline being retired or replaced) |

---

## 6. Pillar L — Loop Engineering (20%)

**Definition**: the degree to which the capture → process → develop → publish → review cycle **actually repeats** as a human+agent division of labor, with deliverables feeding back to strengthen the system (compounding).

> **Important: this pillar alone weighs "evidence of operation" over "design"** — Level 3 or higher on the L pillar strictly requires **recurring artifacts dated within the last 30 days** (daily notes, weekly reviews, processing logs). A beautiful pipeline document with no recent traces of operation caps at 2.
>
> **Planned pause (v1.1)**: if a cause record **dated before the interruption began** exists (vacation note, outage log, etc.) and artifacts prove the loop resumed with zero manual repair, that period is excluded from the execution-rate denominator (up to 14 days within the 30-day window). Retroactive claims are not accepted. This clause evaluates **resilience**, not raw uptime.

### L1. Defined Pipeline

Core question: Are the stages by which knowledge enters and exits made explicit, with an owner (human/agent) assigned to each stage?

| Level | Anchor |
|---|---|
| 0 | Knowledge only comes in; there is no path out |
| 1 | An implicit flow exists but is undocumented |
| 2 | Stages (capture → organize → publish, etc.) are defined in documents |
| 3 | Per-stage folders and status fields are reflected in the structure; skills/commands that perform stage transitions exist; the human/agent division of roles is explicit |
| 4 | Per-stage dwell time and conversion rates are measured, with a record of bottleneck improvements |

### L2. Cadenced Reviews

Core question: Do daily/weekly/monthly reviews actually recur, with the agent assisting?

| Level | Anchor |
|---|---|
| 0 | No reviews |
| 1 | Done occasionally, irregularly |
| 2 | Templates and cadence are defined, but the execution rate over the last 30 days is below 50% |
| 3 | Execution rate of 50%+ over the last 30 days, and the agent generates review drafts and aggregations (confirmed by artifacts) |
| 4 | Multiple traceable cases of review results leading to system changes (revised rules, skills, structures) |

### L3. Backlog & Flow Control

Core question: Is the inbox under control — do you know and manage the capture-to-processing ratio?

| Level | Anchor |
|---|---|
| 0 | No inbox concept; everything is jumbled together |
| 1 | An inbox exists but the backlog grows without bound; its size is unknown |
| 2 | Backlog size is known, with intermittent cleanups. Processing criteria (what to keep, what to discard) are documented |
| 3 | Triage automation exists (agent classification and routing), and backlog volume is tracked in the cadenced reviews |
| 4 | The capture:processing ratio is measured and managed, with a record of regulating the inflow itself (revised capture rules) |

### L4. Compounding Write-back

Core question: Are agent-session deliverables promoted into first-class knowledge that raises the baseline for the next session?

| Level | Anchor |
|---|---|
| 0 | Agent conversations evaporate |
| 1 | Good results are occasionally copied and saved by hand |
| 2 | A dedicated location for agent outputs exists, but deliverables pile up as dumps with no structure for re-reference |
| 3 | Skills/procedures exist that promote deliverables into typed, first-class notes (query results → wiki documentation, etc.), and later sessions actually search for and reuse them |
| 4 | The write-back rate (share of session deliverables promoted) or reuse frequency is observed, with a record of the write-back pipeline being revised |

### L5. Self-correction Loop

Core question: Is there a closed circuit in which failures and feedback lead to updates of instructions, structure, and guardrails?

| Level | Anchor |
|---|---|
| 0 | The agent repeats the same mistake and it is left alone |
| 1 | Corrections happen in chat as they arise, but evaporate when the session ends |
| 2 | A habit of recording corrections into instruction files/memory exists (irregular) |
| 3 | Feedback → memory/instruction updates is proceduralized (a rule of recording corrections immediately), and audits of the system itself (decision audits, maturity assessments) have been executed |
| 4 | A record of the audit → backlog → execution → re-audit cycle completing at least twice (including tracking of recommendation follow-through rates) |

---

## 7. Pillar X — Interop & Governance (15%)

**Definition**: the degree to which the system is not locked into a particular runtime or device but ports across them, is defended against loss and leakage, and can be transferred to other people.

### X1. Cross-runtime Portability

Core question: Is a single canonical context reused across multiple agent runtimes (Claude Code, Codex, Gemini, Cursor, OpenClaw, etc.)?

| Level | Anchor |
|---|---|
| 0 | Locked to a single runtime; no concept of portability |
| 1 | Instructions are written separately per runtime, and their contents have drifted apart |
| 2 | A shared standard file such as AGENTS.md exists, but some runtimes are unconnected (empty file, unreferenced) |
| 3 | A shared canonical source + per-runtime adapters (symlinks, references, conversion scripts) so that all major runtimes see the same context. Per-runtime capability differences are documented |
| 4 | The onboarding procedure for adding a new runtime is documented and validated, and cross-runtime drift checks run periodically |

> **Primary-runtime-focused users (v1.1)**: even if a secondary runtime's actual usage share is low, an adapter that is documented and has **at least one verified execution record** (a session log or output showing that runtime reading the canonical context) counts as "connected." What this criterion measures is **verified portability**, not constant multi-runtime operation.

### X2. Device & Agent Topology

Core question: Are the roles and output paths of multiple devices and agents defined, with sync conflicts managed?

| Level | Anchor |
|---|---|
| 0 | A single device — or multiple devices editing the same files chaotically |
| 1 | Multiple devices in use, but with no role separation or conflict countermeasures (frequent sync conflicts) |
| 2 | Per-device/per-agent roles are documented. Output paths are separated per agent |
| 3 | Role and path separation is enforced structurally (dedicated per-agent output folders); the sync mechanism, conflict history, and countermeasures are documented |
| 4 | A record of topology changes (devices added/removed) applied per procedure, with the effectiveness of conflict-recurrence prevention verified |

### X3. Recoverability

Core question: Are the knowledge and configuration version-controlled and backed up — and is recovery actually possible?

| Level | Anchor |
|---|---|
| 0 | No backups |
| 1 | Mistaking cloud sync (Dropbox, etc.) for backup (deletions and corruption propagate as-is) |
| 2 | Only some core assets are covered by git/backups. Coverage is not documented |
| 3 | The knowledge base, agent configurations, and skills are all covered by VCS or snapshot backups, with the coverage list documented |
| 4 | Records of recovery rehearsals (actual restore tests) exist, plus a history of backup gaps discovered → closed |

### X4. Security & Privacy

Core question: Are secrets and PII controlled within the knowledge base and in public deliverables?

| Level | Anchor |
|---|---|
| 0 | Passwords and keys scattered in plain text across notes; no awareness |
| 1 | The risk is recognized, but nothing is inventoried or remediated |
| 2 | The attack surface and locations of sensitive data are documented (an inventory exists). Some unremediated items remain |
| 3 | A secrets-management tool is in use; PII-stripping procedures for public releases exist; agent access restrictions on sensitive paths are actually configured |
| 4 | Periodic security audits run on a schedule, with finding → remediation completion rates tracked |

### X5. Shareability & Standardization

Core question: Does the system exist as documents and kits — not just inside the owner's head — so that others can adopt it?

| Level | Anchor |
|---|---|
| 0 | The system exists only as tacit knowledge |
| 1 | It can be explained to others, but no documents exist |
| 2 | System documents exist, but they assume the owner's environment so heavily that others cannot apply them |
| 3 | Distributed as starter kits, public documents, and templates, with version and release management. Actual external adoption exists, or use in lectures/consulting |
| 4 | Records of adopter feedback leading to kit revisions; accepted improvement contributions from a community or students |

---

## 8. Scoring Protocol (Evaluation Procedure)

### 8.1 Evidence Collection — 10–60 minutes

Collect the following from the evaluee, or inspect it directly. For remote evaluations, screenshots and file copies may substitute.

**System boundary declaration (v1.1)**: before scoring begins, declare the boundary of the evaluated system in the report — the vaults, repositories, devices, and agent hosts included. This prevents evaluator discretion over boundaries from driving scores in distributed systems (publishing corpus on a separate drive, agent host on a separate PC).

**Unreached-node clause (v1.1)**: enforcement mechanisms on nodes unreachable during the evaluation session (another PC's config, an offline host) may be substantiated by **behavioral traces** — N or more instances of consistently pathed/permissioned outputs. In that case, list the unreached nodes and the substitution method in the report. A score should be a property of the system, not a function of where the evaluation session happened to run.

**Mandatory artifact checklist**

| # | Artifact | Related criteria |
|---|---|---|
| 1 | All agent instruction files (CLAUDE.md/AGENTS.md/GEMINI.md/.cursorrules, etc.) + their scope placement | P1, P2, X1 |
| 2 | Directory listing of skills/commands/hooks + the full text of 3 representative ones | P2, P3, H2, H3 |
| 3 | Top-level structure of the knowledge repository + classification rule documents | C1 |
| 4 | Per-note-type templates and the frontmatter schema | C2 |
| 5 | Search tool configuration + index status (document count, last refresh date) | C3 |
| 6 | Three actual daily/weekly reviews from within the last 30 days | L2 |
| 7 | Inbox size (file count) + its proportion of the whole | L3 |
| 8 | Agent output locations + two cases of write-back (formal promotion) | L4 |
| 9 | Memory files and configuration files (permissions, hooks, MCP list) | H1, H3, H4 |
| 10 | List of runtimes in use + each runtime's context connection status | X1, X2 |
| 11 | git/backup coverage (what is covered and what is not) | X3 |
| 12 | Security inventory and secret-handling method | X4 |
| 13 | Existence of public distributions (starter kits, documentation sites) | X5 |
| 14 | Cases of "the system fixing the system" — audit/retrospective → revision records | all Level-4 judgments |

### 8.2 Scoring Rules

1. **Score each criterion independently**: to prevent halo effects, criteria may be scored in shuffled pillar order.
2. **No evidence caps the score at 1**: criteria supported only by self-report score at most 1.
3. **Recency rule**: 30 days for the L pillar; for the rest, if there are no traces of use or updates within 90 days, deduct 1 level as "exists but dormant."
4. **Conservative judgment**: when torn between two levels, take the lower. Never award Level 4 without a "record of improvement."
5. **N/A handling**: for a justified N/A (e.g., X2 for a single-person, single-device user), exclude that criterion and redistribute the pillar's allocation across the remaining criteria. To prevent N/A abuse, allow at most 3 across the entire system.

### 8.3 Deliverables

- Per criterion: level (0–4) + one line quoting the evidence + one line stating the deduction reason
- Per-pillar subtotals and the total score, with the maturity band
- Top 3 strengths / top 3 priority improvements (with estimated score gains)
- (For multi-environment users) a per-runtime current-status and direction matrix
- **(Recommended, v1.1) Root-cause deduction trace**: a table tracing major deductions back to root design choices. It does not affect scoring; it is a diagnostic deliverable that aims the improvement roadmap at causes rather than symptoms

### 8.4 Anti-Gaming Cautions

- **Does merely writing a document earn 3 points?** No. Level 3 requires "tools enforcing the rules or making them the default" — documents alone are worth 2.
- **Last-minute cramming before the evaluation**: check artifact dates. Reviews and logs batch-created within 7 days of the evaluation are not accepted as L-pillar evidence.
- **Tool collectors**: 20 installed MCP servers are not evidence for H1. Evidence is traces of the tools "being used" in combination with the knowledge base.
- **Evaluator conflict of interest**: when self-evaluating one's own system, require at least one deduction reason to be recorded per criterion (a forced flaw search).

---

## 9. Supplementary Assessment — Runtime Matrix (a profile outside the score)

Multi-runtime users complete the matrix below in addition to the total score. It serves as the evidence base for scoring X1 and as input for roadmap derivation.

| Runtime | Context connection | Skills/automation | Role | Operating status | Direction |
|---|---|---|---|---|---|
| (e.g., Claude Code) | Canonical direct / adapter / separate copy / unconnected | Yes (n) / none | Primary / specialized / experimental / dormant | Daily / intermittent / stopped | Maintain / expand / consolidate / retire |

---

## 10. Versioning & License

- **v1.1.0** (2026-07-21): revision incorporating field feedback — 6 items adopted from the adversarial-verification feedback by Changhyun Ahn (Achmage OS field assessment, 2026-07-20). ① "Two forms of observation" note for Level 4 (quantitative metrics and dated qualitative incident records accepted equally) ② P3 central routing registry as an equivalent path ③ X1 verified-portability note ④ L-pillar planned-pause clause ⑤ §8.1 system boundary declaration and unreached-node clause ⑥ §8.3 root-cause deduction trace (recommended deliverable). **The 5 pillars × 25 criteria, weights, level ladder, and scoring formula are identical to v1.0** — scores remain comparable.
- **v1.0.0** (2026-07-16): first public release. 5 pillars × 25 criteria. Reference case: published together with the measured evaluation of the CMDS (Yohan Koo) system.
- Revision policy: adding or removing criteria is a minor version; editing anchor wording is a patch. Weights to be revisited once 5 measured cases have accumulated.
- Use: free to use and adapt. Attribution "based on AKM Index v1.1" is recommended in evaluation reports.
