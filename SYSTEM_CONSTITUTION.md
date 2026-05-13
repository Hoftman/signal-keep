# Signal Keep — System Constitution

**Version:** 2.5
**Status:** Ratified
**Supersedes:** v2.4

---

## Scope

This Constitution governs **Signal Keep** only. **Signal Lab** and **Signal Care** are bound by it only at the boundary where they interact with Signal Keep — namely, the consent and contribution interface for Sharing-tier data (Lab) and the handoff and booking records for Supported-tier subscribers (Care). Internal operations of Lab and Care are outside the scope of this document.

---

## Brand

**Signal Keep** — the app. A faithful record of what you tell your body and its feedback. Kept by you. Read by you.

**Signal Lab** — the research arm. Studies patterns across anonymized contributed records and publishes findings as open research. Operates outside the deterministic execution path of Signal Keep (§I.3). Not governed by this Constitution except at the contribution boundary.

**Signal Care** — the clinical handoff. Managed booking and payment for appointments with a licensed internist partner. The partner is not employed by Signal Keep; Signal Keep does not direct the care. Not governed by this Constitution except at the handoff boundary.

---

## Changelog (v2.4 → v2.5)

- **Third signal kind — Plan** — introduced. Plans are subscriber-stated intentions for Action signals. Distinct from Action (what was logged) and Reaction (what the body reported).
- **Glossary** updated: Signal kinds are now Plan, Action, Reaction.
- **Appendix B — Plan Registry** added. Defines schema, vessel concept, supersession rules, target kinds, applicability matrix, and gap semantics.
- **Plan scope:** optional per (subscriber, signal). Logging Actions works without a Plan. Plans apply only to Action signals; Reactions are involuntary by definition and have no Plan.
- **Plan cardinality:** exactly one active Plan per (subscriber, action_signal). A new Plan is a new append-only row that supersedes the previous (§I.1).
- **Vessel concept:** Plans carry an optional `vessel` field — the subscriber's natural unit of accounting (e.g., 250 ml glass, 600 ml bottle). Storage remains metric (§I.8); vessels are a presentation-layer aid.
- **Vessel freedom:** any positive integer size in the signal's Capture unit. Curated picker in UI, free-form in schema.
- **Gap semantics:** the Plan–Action gap is **computed on read, never stored.** No Action record references a Plan id.

## Changelog (v2.3 → v2.4)

- **Scope section** added at top. Constitution governs Signal Keep only; Lab and Care bound only at their interaction boundaries with Keep.
- **`stillness` recategorized** from Core Metabolic Drivers → Support Regulators. Category counts: Core 6, Support 7, Operational 4. Total Action signals: 17.
- **Stillness definition** rewritten to ground the signal explicitly in the Judeo-Christian spiritual tradition.
- **Determinism Law (§I.3)** strengthened with explicit firewall: Signal Keep never consumes Signal Lab outputs as inputs.
- **Appendix A.2** tables extended with a **Capture** column declaring the unit/structure used to record each Action signal.
- **Registry version** bumped to 1.2.

## Changelog (v2.2 → v2.3)

- **Appendix A.2 — Action Registry** extended with seventeenth signal: `stillness`. Captures intentional wakeful rest.
- **Brand section** added. Names Signal Keep (app), Signal Lab (research), Signal Care (clinical handoff) as the canonical product trinity.
- **Glossary** Sharing-tier definition cross-references Signal Lab.
- **Glossary** Supported-tier definition cross-references Signal Care.
- **Registry version** bumped to 1.1.

## Changelog (v2.1 → v2.2)

- **Appendix A.3 — Reaction Registry** populated. Two kinds defined: **Fast** (perceived, app-open cadence, 0–10 scale) and **Slow** (measured, monthly cadence, metric units).
- **Naming choice:** Fast/Slow chosen over Immediate/Eventual to avoid collision with §V.5 eventual consistency.
- **Period data** gated on biological-sex profile field; added as conditional Slow reaction.
- **Cadence policy:** soft — encouraged but not rate-limited.

## Changelog (v2.0 → v2.1)

- **Appendix A: Signal Registry** added — canonical list of Action signals grouped into 3 categories. Versioned separately; amendable as Policy.
- **Category** defined in Glossary — categories carry both UX-grouping and priority-ordering semantics.
- **Signal** glossary entry extended with slug/id convention and cross-reference to Appendix A.
- **Reactions** noted as logged directly by subscriber (registry TBD in a future revision).
- **Emoji convention** declared as reference-only; each signal has a stable snake_case slug as canonical identifier.

## Changelog (v1 → v2.0)

- **Numbering:** dotted section IDs (I.1, V.2, etc.) for unambiguous cross-references.
- **Glossary:** added at top.
- **Tiers:** three tiers defined (Sharing, Private, Supported) with pricing and data semantics.
- **Signals:** defined (Actions vs. Reactions).
- **Clock skew:** single hybrid rule with soft (90s) and hard (300s) thresholds — resolves v1 A/B ambiguity against the Determinism Law.
- **Identity uniqueness:** match on *either* email or phone triggers duplicate detection.
- **Phone verification:** email mandatory, phone optional.
- **Phone change handling:** new `identity_profile` record; `subscriber_id` unchanged.
- **Identity merges:** clarified as append-only merge events, never mutations.
- **Evolution Law:** migrations can add/deprecate; they cannot mutate existing rows.
- **Referral:** one-time benefit per referred user, lifetime, across any referrer.
- **RPO/RTO:** externalized to Operations Policy doc.
- **Rate limiting:** stated as principle — concrete limits defined per endpoint in API spec.
- **Amendment process:** Core Laws amendable only by super-majority of technical committee + 30-day public review.
- **Acknowledgement mechanism:** design doc signed by eng lead + domain owner, archived in `/governance/acks/`.
- **Copy:** markdown escape artifacts cleaned; "24-hour rule" defined at first use; Effect Law wording normalized.

---

## Purpose

This document defines the non-negotiable structure of Signal Keep. Nothing here is optional. Where flexibility is required, it is explicitly defined as policy.

---

## Glossary

- **Subscriber** — An identified, verified individual with a `subscriber_id`. All records are associated with a subscriber.
- **Signal** — A chemical messenger (hormones, metabolites) or physical input that instructs cells and organs to alter energy-producing or energy-storing behavior. Each signal has a stable, immutable snake_case slug (e.g. `sleep_recovery`) that serves as its canonical identifier across storage, APIs, and i18n keys. Emojis and display names are presentation concerns and may change; slugs may not (per §I.7). The canonical list lives in **Appendix A**. Signals split into three kinds:
  - **Plan** — subscriber-stated intention for an Action signal (e.g., "3,500 ml of water per day"). Optional; not all subscribers have a Plan for every signal. One active Plan per (subscriber, action signal) at a time; new Plans supersede previous ones append-only. Plans may carry a **vessel** — the subscriber's natural unit of accounting (e.g., 250 ml glass) for presentation purposes only. Plans apply only to Action signals. See **Appendix B**.
  - **Action** — voluntary, subscriber-originated input (food choice, exercise, sleep timing, stillness). Logged directly by the subscriber. Action records never reference a Plan id; the Plan–Action gap is computed on read.
  - **Reaction** — involuntary body-originated message (hunger, satiety, energy level). Logged directly by the subscriber. Reactions have no Plan — they are involuntary by definition.
- **Category** — A grouping of signals that serves two purposes: (a) UX organization in the PWA navigation, and (b) priority ordering reflecting biological weight. The three categories, in priority order, are **Core Metabolic Drivers** > **Support Regulators** > **Operational Signals**. See Appendix A.
- **Tier** — A subscription class (Sharing, Private, Supported) that determines pricing and data-handling semantics. Each tier has its own isolated database.
- **Sharing Tier** — $70/year. Anonymized signal data is contributed to **Signal Lab** for training research models under explicit signup consent. Membership in this tier is permanent with respect to training: data contributed under Sharing cannot be retroactively withdrawn from trained models.
- **Private Tier** — $70/month or $700/year. Full data sovereignty; no Signal Lab contribution; data is only presented back to the subscriber.
- **Supported Tier** — $700/month or $7,000/year. Private-tier data sovereignty plus access to **Signal Care**: managed handoff to a licensed internist partner.
- **Adapter** — An implementation of an external dependency (database driver, billing provider, email service). Adapters live outside Core.
- **Port** — An interface declared in Core that an adapter implements. Core depends only on ports.
- **Core** — Business logic and ports. Compiles and tests without any adapter.
- **Event** — An append-only record of something that occurred.
- **Effect** — An outbound interaction with an external system (billing charge, email send, webhook call).
- **Audit log** — Append-only, hash-chained record of state changes and access decisions.
- **correlation_id** — UUID linking a request, its effects, and their responses across services.
- **idempotency_key** — Client-supplied key guaranteeing a write produces exactly one record regardless of retries.
- **event_time_local** — Clock time as the subscriber experienced it, with timezone offset.
- **event_time_utc** — Server-derived UTC equivalent of event_time_local.
- **log_time_utc** — Server-generated timestamp at write time.
- **24-hour rule** — Default window within which events may be logged retroactively; see §II.4.
- **RPO / RTO** — Recovery Point Objective (max tolerable data loss) / Recovery Time Objective (max tolerable downtime). Values live in the Operations Policy document.

---

# I. Core Laws (Immutable)

These laws define the system. They are amendable only under §VI.

## I.1 Data Integrity Law

All persisted data is append-only. No updates. No deletes. Every state change creates a new record. Audit logging is first-class and mandatory.

### I.1.a Tamper Evidence

Audit logs must be tamper-evident:

- Each record includes a cryptographic hash of the previous record (hash chaining).
- Periodic snapshot hashes are stored externally.

### I.1.b Identity Merges and Append-Only

Identity merges (§I.9.c) and phone-number changes (§I.9.d) do not mutate existing records. They create new records (a merge event, a new `identity_profile` row) that reference prior state. History is never rewritten.

---

## I.2 Time Law

Every record includes three timestamps:

- `event_time_local` (client-provided, with timezone offset)
- `event_time_utc` (derived server-side)
- `log_time_utc` (server-generated at write time)

### I.2.a Rules

- Local time is used for user-facing views.
- UTC is used for ordering and system logic.
- All timestamps are required.

### I.2.b Ordering

1. Primary: `event_time_utc`
2. Tie-breaker: `log_time_utc`
3. Final tie-breaker: monotonic record ID

### I.2.c Clock Integrity

The server validates client-supplied time against its own clock using a hybrid two-threshold rule:

- **Soft threshold (default 90s):** within this skew, the record is accepted normally.
- **Hard threshold (default 300s):** between soft and hard, the record is accepted with `time_untrusted = true` and flagged in the audit log. Beyond hard, the record is rejected with correction guidance.

Thresholds are configurable (§III.1). The rule itself is not — the system always applies exactly this hybrid, preserving determinism (§I.3).

### I.2.d Idempotency

- Every write must include an `idempotency_key`.
- Duplicate submissions must not create duplicate records. See §V.1.

### I.2.e Local Time Semantics

`event_time_local` records the clock time as the subscriber experienced it, regardless of timezone. For events with duration, `event_time_local` is when the event began. For instantaneous events, it is when the event occurred. Charts and user-facing visualizations use `event_time_local`. Ordering and system logic use `event_time_utc`.

---

## I.3 Determinism Law

Signal Keep is fully deterministic. Outputs are strictly derived from inputs and persisted state. No AI, no LLMs, no probabilistic behavior in the execution path.

### I.3.a Signal Lab Firewall

Signal Lab models are trained on anonymized Sharing-tier contributions but operate entirely outside the deterministic execution path of Signal Keep. **Signal Keep never consumes Signal Lab outputs as inputs.** No model output — including population baselines, reference ranges, recommendations, predictions, classifications, or derived statistics — flows back into Signal Keep's computation, storage, or presentation layers. The contribution channel from Keep to Lab is one-way and read-only from Lab's perspective.

This firewall is structural: Signal Lab is reached only through outbound effects (§I.4) at the contribution boundary. There is no inbound port from Lab to Keep.

---

## I.4 Effect Law

All outbound interactions with external systems must be reproducible.

### I.4.a Requirements

Each external effect must record:

- request payload
- response payload
- status
- `event_time_utc` and `log_time_utc`
- `correlation_id`

### I.4.b Replay

- External effects must support deterministic replay.
- Idempotent behavior is required; non-idempotent providers must be wrapped with deduplication (§V.1).

---

## I.5 Architecture Law

Hexagonal architecture is mandatory.

### I.5.a Rules

- Core contains business logic and ports.
- Adapters implement all external dependencies.
- No provider-specific logic outside adapters.
- Core must compile and test without adapters.

### I.5.b Billing Architecture

Billing is isolated via a dedicated port:

- All billing state is derived from events.
- No provider-specific assumptions leak into Core.

---

## I.6 Server Authority Law

All business logic, validation, and state live on the server. The client is a presentation layer only.

---

## I.7 Evolution Law

- All schema changes occur via migrations.
- Field meaning never changes.
- Migrations may: add fields, add tables, add indexes, create new versioned read paths, deprecate reads.
- Migrations may not: mutate existing rows, backfill derived values into historical records, or rewrite history.
- Versioning is mandatory for changes.

---

## I.8 Units Law

All stored data uses metric units only. Conversion to imperial occurs only at the presentation layer, driven by subscriber locale preference.

---

## I.9 Identity Law

Every record is associated with a subscriber identity.

### I.9.a Uniqueness

Identity is globally unique on **either** email or phone: a match on either field against any existing subscriber triggers duplicate-detection (§V.7). Neither field may be reused across distinct subscribers.

### I.9.b Verification

- Email verification is **mandatory** for all subscribers.
- Phone is **optional**. When provided, it participates in uniqueness and duplicate detection per §I.9.a regardless of whether it has been verified.
- Phone verification is required only when phone is used for authentication, account recovery, or Signal Care handoff (Supported tier).

### I.9.c Identity Merging

- Merging is explicit and auditable.
- Merges require operator action (§V.7) — no automatic merges.
- A merge creates a new merge event referencing both identity records; original records are retained unchanged.

### I.9.d Phone Number Changes

A subscriber may change their phone number post-signup. The change creates a new `identity_profile` record. The previous phone is retained in history (and remains blocked from reuse by other subscribers under §I.9.a). `subscriber_id` does not change.

---

## I.10 API Contract Law

All APIs are versioned from day one. Contracts do not break. Breaking changes require a new version.

---

## I.11 Access Control Law

- No access without verification (§I.9.b).
- RBAC enforced server-side.

### I.11.a Secrets & Keys

- Environment variables only.
- Key rotation required.
- Encryption key lifecycle defined in Operations Policy.

### I.11.b Access Audit

- All access decisions are logged, including denials.

---

## I.12 Operations Law

All operational concerns are accessed through ports.

### I.12.a Requirements

- Monitoring, failover, rate limiting required.
- SLOs must be defined.
- Backups must be immutable.
- Disaster recovery targets (RPO/RTO) defined in the Operations Policy document.

---

## I.13 Internationalization Law

All user-facing text must be externalized. No hard-coded strings. Multi-locale support is structurally enabled from day one, including:

- RTL layout support.
- Locale-specific pluralization rules.
- Locale-specific number, date, and time formatting.
- Imperial-unit display per §I.8 driven by locale preference.

---

# II. System Rules (Enforced Implementation)

## II.1 Architecture & Adapters

- Every dependency has a primary plus fallback adapter.
- Circuit breakers, retries, and timeouts are mandatory (§V.8).
- Failover is automatic.

## II.2 Reliability

- No data loss.
- Real-time failover.
- Backup and restore are tested regularly (Operations Policy).

## II.3 Data

- Append-only enforced at the DB level.
- Immutable audit log (§I.1.a).
- No deletion under any circumstance.

## II.4 Time Enforcement

- **24-hour rule:** events may be logged retroactively only within 24 hours of `event_time_local`. Configurable per §III.1.
- Enforced via `event_time_utc` against server clock.
- Exception workflow: admin override, recorded with audit flag and justification (§V.2.b).

## II.5 Tiers & Data Isolation

- Each tier (Sharing, Private, Supported) has its own database.
- Data never moves between tier databases.
- Sharing-tier membership is permanent with respect to Signal Lab training: already-contributed data cannot be retroactively withdrawn from trained models. Subscribers may stop new contributions at any time by tier change or cancellation.

### II.5.a Federated Queries

- Must aggregate across all tier databases the caller is authorized to read.
- Default consistency model: eventual (§V.5).

## II.6 Referral Integrity

- Cross-check email and phone across all tier databases before granting referral benefit.
- Referral benefit is **one-time per referred user, lifetime, across any referrer.** A subscriber can only ever be "the referred" once.
- All referral attempts (valid and invalid) are logged.

## II.7 Security

- Encryption at rest and in transit.
- Rate limiting enforced (§II.7.a).
- Input validation at every boundary.

### II.7.a Rate Limiting

Principle: every public endpoint must enforce rate limits. Concrete limits are defined per endpoint in the API specification, not here.

## II.8 CI/CD

- Tests required: unit, integration, end-to-end.
- Migration validation required (§I.7).
- API backward-compatibility tests mandatory (§V.9).

## II.9 Documentation

- OpenAPI specs required.
- Docs updated with every change.

## II.10 Accessibility & UX

- WCAG 2.1 AA.
- Graceful degradation.

---

# III. Operational Policies (Configurable)

All values live in the Operations Policy document. This section names them.

## III.1 Time Policy

- 24-hour rule threshold (default 24h).
- Clock skew: soft threshold (default 90s), hard threshold (default 300s) — see §I.2.c.

## III.2 Tier Policy

- Pricing, billing cycles, and plan definitions (current values: see Glossary).

## III.3 Billing Policy

Must support:

- subscription creation
- renewals
- delays
- failures

## III.4 Referral Policy

- Reward amounts and eligibility window.

## III.5 Operations Policy

- Monitoring depth.
- Failover strategies.
- RPO and RTO targets.
- Backup cadence and retention.
- Restore-test cadence.

---

# IV. Development Process

## IV.1 Acknowledgement

Before any implementation PR is merged:

- A design doc must reference every Law, Rule, and Policy section it interacts with by ID (e.g. §I.1, §V.3).
- The design doc must be signed by the engineering lead and the relevant domain owner.
- The signed doc is archived in `/governance/acks/` and linked from the implementation PR.

## IV.2 Reviews

- Schema changes require design review (§I.7).
- New adapters require security review (§I.11).

---

# V. Failure-Safe Specification

Every critical path must specify success, failure, and recovery.

## V.1 Idempotency & Exactly-Once Effects

- Every write requires `idempotency_key`.
- Uniqueness scope: `(subscriber_id, idempotency_key)`.
- Server stores idempotency records with request hash and response.
- Retries return the original response.
- External effects (billing, email) must be idempotent or wrapped with deduplication.

## V.2 Time Failure Handling

### V.2.a Clock Skew

See §I.2.c for the hybrid rule. The rule itself is immutable; only its thresholds are configurable.

### V.2.b Late Events

- Default: reject beyond the 24-hour rule.
- Exception path: admin override or explicit "late entry" mode, both recorded with audit flag and justification.

## V.3 Write & Failover Semantics

- Writes must be atomic per storage system.
- On failure or timeout, the client retries with the same `idempotency_key`.
- After failover, no duplicate writes — guaranteed by §V.1.

## V.4 External Systems (Billing) Reconciliation

- All providers must support webhook ingestion.
- System records inbound provider events and reconciles daily against provider state.
- Discrepancies create audit events and require a resolution workflow.

## V.5 Federated Query Consistency

- Default model: eventual consistency.
- Query layer must merge by `(event_time_utc, log_time_utc, id)` and deduplicate via record IDs.
- Partial outages return degraded results with a completeness flag.

## V.6 Audit Log Integrity Failures

- On hash chain break, the system raises a critical alert and isolates the affected segment.
- Recovery: rebuild chain from the last valid checkpoint, record the repair event immutably.

## V.7 Identity Conflict Resolution

- Duplicate detection (§I.9.a) triggers a resolution workflow.
- Merge requires explicit operator action and a full audit trail.
- No automatic merges.

## V.8 Adapter Failure Handling

All adapters must implement:

- timeout
- retry with backoff
- circuit breaker

Fallback adapter must be functionally compatible with the primary.

## V.9 API Contract Enforcement

- Backward-compatibility tests mandatory.
- OpenAPI schemas validated in CI.
- Breaking changes require a new version.

## V.10 Disaster Recovery

- Backups are periodic and immutable.
- RPO and RTO defined in the Operations Policy document.
- Regular restore tests required.

---

# VI. Amendment Process

Core Laws (§I) are immutable except through this process:

1. A written amendment proposal referencing the Law(s) to be changed.
2. Super-majority approval (two-thirds) of the technical committee.
3. A 30-day public review period during which the proposal is open for comment.
4. After the review period, a second super-majority vote ratifies the amendment.
5. The amendment is merged as a new version of this Constitution, with the changelog recording the rationale and vote.

System Rules (§II) and Operational Policies (§III) may be amended by simple technical-committee majority, recorded in the changelog.

---

# VII. Cross-Signal Dependencies

Signals (see Glossary) share data where biological reality requires it. A value that moves — weight, meals per day, protein multiplier — affects every signal that depends on it.

- The system always uses the most recent source of truth at the time of computation.
- Subscribers are informed that changing a setting in one signal may affect derived fields in another.
- Historical logs are never recomputed. Flags are frozen at the time of logging (§I.7).

---

# VIII. Final Principle

Signal Keep is:

- Deterministic
- Append-only
- Server-authoritative
- Fully auditable
- Tamper-evident
- Failure-safe
- Operationally resilient
- Provider-independent

Nothing is built that violates these principles.

---

# Appendix A: Signal Registry

**Registry version:** 1.2
**Status:** Canonical
**Amendment:** Simple-majority of the technical committee, recorded in the changelog. Amendments may add signals, deprecate signals, or rename display labels. Slugs, once assigned, are immutable (§I.7).

## A.1 Conventions

- **Slug** is the canonical, immutable identifier. Snake_case. Used in storage, APIs, and i18n keys. Never reused.
- **Display name** is the human-readable label. Localized per §I.13.
- **Emoji** is reference-only shorthand used in this document and early product surfaces. Not canonical — product surfaces may substitute custom iconography without a registry change.
- **Category** determines UX grouping and priority ordering. Core Metabolic Drivers > Support Regulators > Operational Signals.
- **Kind** distinguishes Action (subscriber-logged intentional behavior) from Reaction (subscriber-logged body response).
- **Capture** declares the unit or structure used to record an instance of the signal. Capture types are: `duration_min` (integer minutes), `count` (integer occurrences), `volume_ml` (integer milliliters), `composite` (a structured record defined per signal). Storage is metric per §I.8.

## A.2 Action Registry (v1.2)

Seventeen Action signals across three categories.

### Core Metabolic Drivers

| Slug | Display Name | Emoji | Capture |
| --- | --- | --- | --- |
| `sleep_recovery` | Sleep and Recovery | 🛌 | `duration_min` |
| `morning_light` | Morning Light Exposure | 🌅 | `duration_min` |
| `resistance_training` | Resistance Training | 💪 | `duration_min` |
| `intermittent_fasting` | Intermittent Fasting | ⏳ | `duration_min` |
| `food_nutrition` | Food and Nutrition | 🍽️ | `composite` (meal record: time, type, macros) |
| `post_meal_walking` | Post-Meal Walking | 🚶 | `duration_min` |

### Support Regulators

| Slug | Display Name | Emoji | Capture |
| --- | --- | --- | --- |
| `daily_hydration` | Daily Hydration | 💧 | `volume_ml` |
| `controlled_breathing` | Controlled Breathing | 🫁 | `duration_min` |
| `stillness` | Stillness | 🕯️ | `duration_min` |
| `cold_exposure` | Cold Exposure | 🧊 | `duration_min` |
| `heat_exposure` | Heat Exposure | 🔥 | `duration_min` |
| `social_connection` | Social Connection | 🤝 | `duration_min` |
| `noon_sunlight` | Noon Sunlight Exposure | ☀️ | `duration_min` |

**`stillness` definition.** Intentional wakeful rest grounded in the Judeo-Christian spiritual tradition: contemplative prayer, Scripture reading, silent reflection, and quiet sitting in the presence of God. The signal is defined by this posture and lineage; it is not a generic mindfulness or meditation field repurposed under a different name.

Practiced as a deliberate pause, stillness activates parasympathetic recovery, lowers cortisol, and restores the nervous system. It is distinct from:

- `sleep_recovery` (unconscious rest)
- `unplug` (screen-absence as such; a subscriber may unplug without being still, and may be still without unplugging)
- `focus_blocks_90` (productive attention; stillness is non-productive by intent)
- sedentary inactivity (involuntary or passive stillness such as desk-sitting, which is not a metabolic positive and is not captured by this signal)

The system does not enforce the distinction between intentional stillness and sedentary inactivity — that discernment belongs to the subscriber, per §I.6 and the product stance that the subscriber is the authority on their own body.

### Operational Signals

| Slug | Display Name | Emoji | Capture |
| --- | --- | --- | --- |
| `joint_mobility` | Joint Mobility | 🧍 | `duration_min` |
| `focus_blocks_90` | 90-Minute Focus Blocks | 🎯 | `count` |
| `unplug` | Unplug | 📵 | `duration_min` |
| `play` | Play | ⚽ | `duration_min` |

## A.3 Reaction Registry (v1.0)

Reactions are subscriber-logged responses of the body. They split into two **kinds** by cadence and data character:

- **Fast reactions** — perceived, subjective. Logged as often as the subscriber wishes, typically on app-open. Captured on a **0–10 integer scale**.
- **Slow reactions** — measured, objective. Logged approximately monthly. Captured in metric units per §I.8; presentation layer converts to imperial per locale.

Cadence is a soft nudge, not an enforced rule: the system may encourage logging via UI prompts but must accept submissions at any frequency (subject to §II.7.a rate limits defined in the API spec).

### Fast Reactions

| Slug | Display Name | Scale | Notes |
| --- | --- | --- | --- |
| `energy_level` | Energy Level | 0–10 | 0 = depleted, 10 = peak |
| `mental_clarity` | Mental Clarity | 0–10 | 0 = foggy, 10 = sharp |
| `sweet_cravings` | Sweet Cravings | 0–10 | 0 = none, 10 = intense |

### Slow Reactions

| Slug | Display Name | Unit | Gating | Notes |
| --- | --- | --- | --- | --- |
| `weight` | Weight | kg | All subscribers | Stored metric; displayed per locale preference |
| `waist` | Waist Circumference | cm | All subscribers | Stored metric; displayed per locale preference |
| `period` | Period | structured | Female biological sex only | Structure TBD: at minimum start date, end date, and flow indicator |

### Gating

- `period` is shown only when the subscriber profile indicates female biological sex. The gating field lives on `identity_profile` and is evaluated server-side (§I.6).
- Subscribers who change the biological-sex profile field create a new `identity_profile` record per §I.9.d; historical `period` logs are retained unchanged per §I.1.

### Extensibility

Additional Fast and Slow reactions may be added by simple-majority amendment (§A). Slugs are immutable once assigned (§I.7). New reactions must declare kind (Fast/Slow), scale or unit, and gating rules at time of addition.

## A.4 Category Ordering

When surfaces need to rank signals by biological weight (dashboards, onboarding, default sort orders), the canonical order is:

1. Core Metabolic Drivers
2. Support Regulators
3. Operational Signals

Within a category, order is the order of rows in A.2 unless a surface explicitly specifies otherwise.

---

# Appendix B: Plan Registry

**Registry version:** 1.0
**Status:** Canonical
**Amendment:** Simple-majority of the technical committee, recorded in the changelog.

## B.1 Definition

A **Plan** is a subscriber-stated intention for an Action signal: what they aim to do, in what natural unit they think about it. Plans are optional per signal — a subscriber may log an Action signal without ever declaring a Plan for it.

Plans are not analytic outputs. They are first-class records, stored append-only, written by the subscriber, governed by §I.1 and §I.7. A new Plan is a new row that supersedes the previous one for the same `(subscriber_id, action_signal_slug)` pair. Old rows are retained.

Plans apply only to **Action signals**. Reactions are involuntary by definition (§Glossary) and have no Plan.

## B.2 Schema

Every Plan record carries:

- `plan_id` — monotonic identifier
- `subscriber_id`
- `action_signal_slug` — must match an entry in Appendix A.2
- `target` — the metric value the subscriber aims for, in the signal's Capture unit (e.g., `3500` for `daily_hydration` whose Capture is `volume_ml`; `1080` for an 18-hour `intermittent_fasting` window when Captured as `duration_min`)
- `target_kind` — declares how `target` is interpreted: see §B.4
- `vessel` — nullable; see §B.3
- `window` — nullable structured field for clock-time windows (e.g., fasting eating window 12:00–18:00 local); see §B.5
- `notes` — nullable free-text subscriber note (i18n-neutral; stored as-entered)
- `supersedes_plan_id` — nullable pointer to the previous active Plan for the same `(subscriber_id, action_signal_slug)` pair
- `event_time_local`, `event_time_utc`, `log_time_utc`, `idempotency_key`, `correlation_id` — per §I.2 and §I.2.d

At any moment, the **active Plan** for `(subscriber_id, action_signal_slug)` is the most recent row (by `event_time_utc`, then `log_time_utc`, then `plan_id`). Older rows remain queryable as history (§I.1).

## B.3 Vessel

The `vessel` field records the subscriber's natural unit of accounting. It is metadata for presentation only; canonical storage of Action records remains in the signal's Capture unit per §I.8.

`vessel` is a structured value:

- `vessel.size` — integer, in the same metric unit as the signal's Capture (e.g., `250` for a 250 ml glass when the signal Captures `volume_ml`)
- `vessel.label_key` — i18n key for the vessel's display name (e.g., `vessels.glass`, `vessels.bottle`, `vessels.cup`)

Vessel size is **free-form**: the subscriber may declare any positive integer size. The product surfaces a curated picker for common sizes, but the field accepts any value.

The UI uses `vessel` to count down in the subscriber's natural unit:

- target 3,500 ml, vessel 250 ml → "14 glasses"
- target 3,000 ml, vessel 600 ml → "5 bottles"
- target 1,080 min (18-hour fast), vessel null → "18 hours"

Vessel is meaningless for some signals (e.g., `morning_light`, `stillness`) and may be omitted. When omitted, presentation falls back to the Capture unit.

## B.4 Target kinds

`target_kind` declares how `target` is read. The enumerated kinds are:

- `daily_total` — cumulative metric value to reach within a day (e.g., 3,500 ml hydration, 45 min walking)
- `daily_count` — number of discrete events to reach within a day (e.g., 3 focus blocks)
- `weekly_count` — number of discrete events to reach within a week (e.g., 4 resistance-training sessions)
- `duration_threshold` — a single event must meet or exceed `target` minutes (e.g., 480 min sleep, 1,080 min fast)
- `time_window` — the event must occur within a clock-time window declared in `window` (e.g., morning light within 60 min of waking, fasting window 12:00–18:00). `target` is the maximum permitted offset in minutes, or `0` for events that must fall fully inside `window`.

The target_kinds applicable to each Action signal are declared in §B.7.

## B.5 Window

The `window` field is structured:

- `window.start_local` — nullable; clock time in local timezone (e.g., `12:00`) or a reference like `wake_time`
- `window.end_local` — nullable; clock time or reference like `wake_time + 60min`
- `window.days_of_week` — nullable; subset of `[mon, tue, wed, thu, fri, sat, sun]`; defaults to all days

References such as `wake_time` are evaluated against the subscriber's logged `sleep_recovery` Action record for the relevant day.

## B.6 Plan–Action gap (computed on read)

The gap between Plan and Action is **never stored**. It is derived at query time:

1. Resolve the active Plan for `(subscriber_id, action_signal_slug)` at the query timestamp.
2. Read Action records in the query range.
3. Compute the gap per `target_kind` (e.g., for `daily_total`: `sum(action.value) − plan.target`).

Because Plans evolve over time but Action records are frozen (§I.7), the gap as displayed today is computed against today's active Plan. Historical "what was the gap on April 22?" queries must resolve the Plan active **on April 22** — a lookup in the append-only Plan history. The system never rewrites historical Action records to reflect new Plans.

No Action record carries a `plan_id` reference. Linking Action to Plan is a query-layer concern, not a schema concern.

## B.7 Applicability matrix

Not every Action signal supports every `target_kind`. The applicable kinds per signal:

| Action Signal | Applicable target_kind(s) | Vessel meaningful? |
|---|---|---|
| `sleep_recovery` | `duration_threshold`, `time_window` | no |
| `morning_light` | `time_window` | no |
| `resistance_training` | `weekly_count`, `duration_threshold` | no |
| `intermittent_fasting` | `duration_threshold`, `time_window` | no |
| `food_nutrition` | `daily_count` (meals) plus composite-specific targets defined when `food_nutrition`'s composite schema is finalized | no |
| `post_meal_walking` | `daily_count`, `duration_threshold` | no |
| `daily_hydration` | `daily_total` | **yes** (glass, bottle, cup) |
| `controlled_breathing` | `daily_count`, `duration_threshold` | no |
| `stillness` | `daily_total`, `duration_threshold` | no |
| `cold_exposure` | `weekly_count`, `duration_threshold` | no |
| `heat_exposure` | `weekly_count`, `duration_threshold` | no |
| `social_connection` | `weekly_count`, `daily_count` | no |
| `noon_sunlight` | `time_window`, `duration_threshold` | no |
| `joint_mobility` | `daily_count`, `daily_total` | no |
| `focus_blocks_90` | `daily_count`, `weekly_count` | no |
| `unplug` | `daily_total`, `time_window` | no |
| `play` | `weekly_count`, `daily_count` | no |

This matrix is part of the Plan Registry and amendable per §VI policy rules.

## B.8 Extensibility

New `target_kind` values may be added by simple-majority amendment. Existing `target_kind` values are immutable (§I.7). Vessel `label_key` values follow the i18n key discipline of Appendix-level keys: snake_case under the `vessels` namespace, immutable once shipped.
