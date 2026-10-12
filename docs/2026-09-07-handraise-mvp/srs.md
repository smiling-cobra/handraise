# SRS: Handraise MVP — Mobile Help Coordination Network

**Status:** Draft for engineering review
**Date:** 2026-10-13
**Product source of truth:** `docs/2026-09-07-handraise-mvp/prd.md` (FR-1 – FR-20, AC-1 – AC-26)
**Background:** `docs/2026-09-07-handraise-mvp/handraise-concept-brief.md`, `competitive-analysis-point.md`

Decisions taken with the product owner before drafting:

- **Mobile:** React Native + Expo (TypeScript), one codebase for iOS and Android.
- **Backend:** TypeScript modular monolith on Node.js, PostgreSQL + PostGIS.
- **Hosting:** AWS `il-central-1` (Tel Aviv), funded by AWS new-account credits (plus AWS Activate if eligible) for the pilot.
- **LLM:** Claude via the Anthropic API as the default, behind a provider-agnostic port. **Replacing the provider must be a configuration change plus a new adapter, never a change to domain code.**
- **Deletion (AC-13):** pseudonymization plus crypto-shredding. Engagement facts survive, and personal data is destroyed.

---

## 1. Current Context (apps, db, infra)

**This is a greenfield build. There is no code, infrastructure, schema, or test suite to inherit.**

- **Repository (`task-link`, branch `prd-refine`)** contains only product documents:
  - `docs/2026-09-07-handraise-mvp/prd.md`: the MVP PRD, which is this plan's contract.
  - `docs/2026-09-07-handraise-mvp/handraise-concept-brief.md`: the original concept. FR-5 of the PRD deliberately amends it: there is no human pre-review queue, and requests are published or blocked with an appeal path.
  - `competitive-analysis-point.md`: an analysis of POINT-class volunteer platforms. It explains why Handraise cannot be built on one (PRD §9).
- **Apps / services:** none. There is no mobile app, API, operator console, or background worker.
- **Database / storage:** none.
- **Infrastructure / config:** none. No AWS account setup, IaC, CI, or environments exist. `.claude/` holds only local tooling settings.
- **Auth / permissions / audit:** none exist. The PRD requires all three from day one (FR-1, FR-2, §5.3).
- **External dependencies still undecided (PRD open questions):**
  - identity vendor (Q3);
  - registry data source (Q12);
  - pilot city (Q1);
  - prohibited / safety-sensitive taxonomy (Q6);
  - retention periods (Q8);
  - pattern thresholds (Q9);
  - moderation staffing (Q10).
- **Constraints carried from the PRD:**
  - native iOS and Android only, with an internal operator console allowed (§6.1 item 19);
  - Hebrew RTL and English;
  - WCAG 2.2 AA equivalent;
  - one jurisdiction (Israel) shipped, but held as configuration (§5.2);
  - no money movement;
  - a 3-month closed pilot in one city.

Because nothing exists yet, this SRS defines the target system rather than changes to one. Section 11 sequences the build from an empty repo.

---

## 2. Goals

### 2.1 Must

- **One-tap bilateral confirmation, proven server-side.**
  - An `Engagement` row can only be inserted when two distinct parties have confirmed the same `EngagementTerms` version with the same content hash (AC-1, AC-26). This is enforced in the database, not only in application code.
  - A material change keeps the prior terms in force until both parties re-confirm (AC-3).
- **Purpose-bound disclosure enforced at the data-access layer.**
  - Address and phone live in an encrypted vault, readable only through a disclosure policy (AC-11).
  - For Organizations, only the handling operator and named attendees can read them.
  - Private matching preferences are never serialized to any other user (AC-12).
- **Append-only history.**
  - Engagement events, terms versions, attestations, screening decisions, moderation actions, and disclosure events cannot be updated or deleted by the application's database role (FR-11, §5.2).
- **Account model:**
  - Individual and Organization account types, required at sign-up and changeable only by support (AC-21).
  - Organizations are operated by named Individual operators, with every action attributed to that operator (AC-22).
  - When an Organization helps, at least one attendee must be named before confirmation (AC-23).
- **Screening pipeline:**
  - Decides within 10 s at p95 (AC-25).
  - Fails closed (AC-6).
  - Detects emergency intent with precedence over every other flow (AC-8).
  - Reaches ≥95% recall on the prohibited set in Hebrew and English (AC-7).
  - Records model, prompt, ruleset, and provider versions on every decision.
- **Replaceable LLM provider.**
  - All LLM use goes through `packages/llm`. Domain code never imports a vendor SDK.
  - Provider and model are chosen per task by configuration.
  - A provider swap is gated by the same evaluation harness.
- **Discovery:**
  - Opt-in matching notifications are delivered within 1 minute at p95 (AC-25).
  - Urgency-invariant ranking (AC-17).
  - Ineligibility reasons are shown, not hidden (AC-18).
- **Group requests:** roles, shifts, and capacity, with an independent Engagement per Helper (AC-16).
- **Trust record:**
  - Derived and never editable.
  - No composite score anywhere (AC-14).
  - Dimensions appear only at ≥3 ratings, and free text is never public (AC-15).
- **Safety-sensitive flow:**
  - Per-engagement, versioned attestations by both parties (AC-9).
  - Evidence labelled as unverified on every surface (AC-10).
- **Deletion model:**
  - Removes personal data without erasing the counterparty's factual history (AC-13).
  - Tested before the pilot launches.
- **Hebrew RTL and English** selectable independently of the device locale. VoiceOver and TalkBack work on all core flows (AC-20).
- **Operator console** for:
  - appeals, reports, Organization validation, and pattern review items;
  - SLA timers and recorded rationale.
- **Jurisdiction-as-configuration:** emergency numbers, taxonomy, attestation texts, retention, thresholds, registry and identity adapters, and the pilot-city area model.
- **Observability** for every §8 metric, including client-measured time to publish.

### 2.2 Should

- A second LLM adapter (OpenAI-compatible Chat Completions) implemented and passing the eval harness. This proves the provider seam is real rather than theoretical.
- Automated Organization validation against registry data, with manual console review as the fallback (Q12).
- Tier 1 identity verification behind a feature flag, ready to switch on once a vendor is chosen (Q3).
- AI-suggested profile additions (FR-3), shown as unconfirmed until the user accepts them (AC-5).
- Starting a new request from a previous request (FR-4).

### 2.3 Nice

- Realtime thread updates over WebSocket. The baseline is push plus polling while a thread is open.
- A self-serve analytics dashboard beyond the console's SQL-backed metric views.
- Automatic image re-encoding and malware scanning for evidence uploads, beyond the MIME and size checks.

---

## 3. Proposed Architecture

### 3.1 Approach and rationale

The system is a **single TypeScript monorepo** containing:

- an Expo mobile app;
- one Node.js **modular monolith** (HTTP API plus a background worker, from the same codebase);
- a small internal web console;
- shared packages for contracts, policy, and the LLM port.

Postgres is the system of record. It also hosts the job queue, so the pilot needs no message broker.

Why this shape:

- **Correctness lives in one transactional database.** The riskiest guarantees — bilateral confirmation, append-only history, capacity limits, and disclosure — are transactional invariants. They are easiest to enforce in one Postgres database, using constraints, triggers, and role grants, inside one deployable.
- **One language end to end.** Zod contracts are shared between mobile, API, and console, so request and response shapes cannot drift between clients.
- **Cheap to run.** The pilot fits within AWS credits: one small compute host, one small RDS instance, S3, and KMS. Section 7 has the cost guard.
- **The split points are modules, not services.** If the pilot succeeds, a module such as screening or notifications can be extracted without redesign.

### 3.2 Repository layout

```
apps/
  mobile/        Expo (React Native, TypeScript, expo-router); EAS Build + EAS Update
  api/           Fastify HTTP API; same image runs `worker` entrypoint (pg-boss jobs)
  console/       Internal operator console (React + Vite SPA), served by api under /console
packages/
  contracts/     Zod request/response schemas, enums, error codes (shared by all apps)
  policy/        Jurisdiction config loader + `jurisdictions/il/*` (taxonomy, emergency numbers,
                 attestation texts, retention, thresholds, area model, adapter selection)
  llm/           Provider-agnostic LLM port, adapters (anthropic, openai-compatible, fake), prompts
  db/            Kysely schema types, SQL migrations, DB roles/grants, invariant triggers
infra/           AWS CDK (TypeScript) stacks: network, data, compute, storage, keys, alarms
evals/           Labelled he/en datasets + harness for structuring, screening, emergency detection
```

**Module boundaries inside `apps/api`.** Each module exposes a service interface. Cross-module imports of internals are banned by `dependency-cruiser` in CI.

| Module | Owns | Key PRD refs |
|---|---|---|
| `identity` | sign-up, OTP auth, sessions, accounts, Organizations, operators, verification tiers, acting context | FR-1, FR-2 |
| `profiles` | profiles, capability claims (source + verification status), private preferences, AI suggestions | FR-3 |
| `requests` | Help Requests, roles, shifts, capacity, Availability Offers, request state machine | FR-4, FR-6, FR-12 |
| `screening` | rules engine, LLM classifier orchestration, emergency detection, `ScreeningDecision`, appeals intake | FR-5, FR-16 |
| `discovery` | feed ranking, search, eligibility reasons, match fan-out for notifications | FR-7 |
| `messaging` | request-scoped threads and messages, leave-thread | FR-8 |
| `engagements` | interests, terms versions, confirmations, attestations, evidence, lifecycle events, group capacity | FR-9, FR-11, FR-12, FR-15, FR-19 |
| `disclosure` | **sole reader of the `vault` schema**; disclosure policy; disclosure events | FR-10 |
| `trust` | feedback (double-blind), derived public aggregates | FR-13, FR-14 |
| `safety` | reports, moderation actions, restrictions, pattern detection, review items | FR-17, FR-18 |
| `notifications` | APNs/FCM push, SMS, email; opt-in preferences; restraint rules | §5.3 |
| `audit` | append-only audit log written by all modules | §5.3 |
| `analytics` | server-side product events, metric views for §8 | §8 |
| `privacy` | data-subject access/export, deletion (crypto-shred), retention jobs | §5.3, AC-13 |

### 3.3 Runtime topology (pilot)

```
 iOS / Android (Expo app)                     Operator browser
        │  HTTPS (JWT)                               │ HTTPS (operator session + TOTP)
        ▼                                            ▼
 ┌──────────────────────────── AWS il-central-1 ───────────────────────────────┐
 │  EC2 (t4g.small, Docker): Caddy (TLS) → api container ──┐                    │
 │                                       worker container ─┤                    │
 │                                                         ▼                    │
 │  RDS PostgreSQL 16 + PostGIS (db.t4g.micro, private subnet, encrypted, 7-day  │
 │  automated backups)  ── schemas: app, vault, audit, jobs (pg-boss)           │
 │  S3 (private, SSE-KMS): evidence docs, org registry docs                     │
 │  KMS: one CMK = key-encryption key for per-user data keys                    │
 │  SSM Parameter Store (SecureString): secrets   CloudWatch: logs, alarms      │
 └──────────────────────────────────────────────────────────────────────────────┘
        │                 │                 │                 │
   Anthropic API     APNs / FCM       SMS + email        Registry data source,
   (via llm port)    (push)           providers          identity vendor (flagged)
```

- The **API and worker** run as two containers from one image. The worker uses **pg-boss**, a Postgres-backed queue, with a **transactional outbox**: domain writes and their follow-up jobs commit atomically. This is how "publish → notify within 1 minute" survives crashes.
- **Single-host, single-AZ** is a conscious pilot trade-off for cost. The recovery path is in §7 and the risk in §9. Everything is containerized and defined in CDK, so moving to ECS Fargate with Multi-AZ RDS is a change to the infrastructure stack only.
- **Mobile releases** use EAS Build. EAS Update ships JS-only fixes and rollbacks within the same runtime version.

### 3.4 Identity and acting context

- **Every authenticated principal is an Individual login.** This follows PRD A2.
  - Authentication: phone OTP plus a verified email.
  - Tokens: a short-lived JWT access token (15 min), plus a rotating refresh token stored hashed.
- **Organization actions use an acting context.** Requests on behalf of an Organization carry an `X-Acting-As: <orgAccountId>` header.
  - Middleware resolves the `Principal { individualAccountId, actingAccountId, operatorMembershipId }` and rejects the request if there is no active operator membership.
  - Every command and audit row records **both** `actor_individual_id` and `acting_account_id`. That gives AC-22 attribution without a shared password.
- **Organization sign-up** creates two things together: the Organization account, and the owner's Individual login (or links an existing one).
  - The owner is pushed to the strongest identity tier available in the pilot.
  - The Organization remains `pending_validation` until it is validated. Until then it can edit its profile and drafts, but cannot publish.
- **Account type** is set once, at sign-up. No API changes it. A console-only support action with an audit rationale can, following the Q13 policy once it is defined.
- **Closed pilot:** sign-up requires an invite code. Codes are issued per partner or channel so liquidity can be attributed (A6).
- **Tier 1 verification** goes through an `IdentityVerificationProvider` adapter, behind the `tier1_verification` flag.
  - The vendor's native SDK is integrated via an Expo config plugin (a dev-client build).
  - Handraise stores only status, vendor reference, attempt count, and timestamps.
  - Three consecutive failures create a console review item instead of a lock (FR-1).

### 3.5 Request creation, screening, and emergency path

**Asking flow:**

```
describe (text or on-device speech-to-text)
  → [emergency detector: rules first, then LLM]   ──emergency──▶ redirect screen (100/101/102/1201)
  → structuring (LLM, ≤5 s p95; manual form if unavailable)
  → single review screen (what/where/when required; optional fields collapsed)
  → submit ──▶ screening (≤10 s p95) ──▶ published | blocked(category, explanation, appeal) | held(screening unavailable)
```

**Voice input** uses the platform's on-device speech recognizers, which support Hebrew, through an Expo module. Audio never leaves the device.

**Emergency detection** is its own service. It runs on the raw description before structuring, and again inside screening.

- The first stage is a deterministic keyword and pattern list for Hebrew and English, from `packages/policy`. It responds in milliseconds and does not depend on the LLM.
- An LLM check follows.
- Either stage can trigger the redirect.
- An emergency result is final and cannot be appealed. It is recorded in a `ScreeningDecision` with `outcome = emergency_redirect`.

**Screening** runs synchronously within a 10 s budget:

1. **Rules stage** (under 50 ms): matches emergency patterns and prohibited-category patterns.
2. **LLM classification stage:**
   - The default model is the fast tier (`claude-haiku-5-5`), with a 7 s timeout.
   - One retry is allowed only if the first attempt failed in under 3 s.
   - The output is structured JSON with these fields: `emergency`, `prohibited_category | null`, `safety_sensitive_categories[]`, `org_eligibility {pass, reason}` (for Organization requesters), `confidence`, `explanation_key`.
3. **Combination: block if either stage says prohibited.** Taking the union of both stages maximizes recall and puts an independent backstop against prompt injection. Safety-sensitive labels are the union of both stages too.
4. **Failure:**
   - If the LLM stage cannot answer within the budget, the request returns to `draft` with `held_reason = screening_unavailable` and nothing is published (AC-6).
   - A circuit breaker shows the "screening temporarily unavailable" message immediately while the provider is down.
5. **Persistence:** every decision writes a `ScreeningDecision` recording:
   - `ruleset_version`, `policy_version`, `provider`, `model`, `prompt_version`;
   - matched category, confidence, and latency;
   - the input hash.
6. **Explanations to the user** are localized policy texts keyed by category. The raw model text is never shown.

**Appeals** from a `blocked` request create an `Appeal` with a 2-business-day SLA in the console. Upholding or overturning it is a `ModerationAction` with a rationale the user can see. An overturn publishes the request without running screening again.

### 3.6 Replaceable LLM port (`packages/llm`)

```ts
type LlmTask = 'structure_request' | 'screen_request' | 'detect_emergency' | 'suggest_profile';

interface LlmClient {
  generateStructured<T>(req: {
    task: LlmTask;
    promptVersion: string;          // resolved from prompts/<task>/<version>
    input: { text: string; locale: 'he' | 'en'; context?: Record<string, unknown> };
    schema: ZodType<T>;             // output validated; invalid JSON = provider error
    timeoutMs: number;
  }): Promise<{ output: T; provider: string; model: string; promptVersion: string;
                latencyMs: number; usage: { inputTokens: number; outputTokens: number } }>;
}
```

- **Adapters:**
  - `anthropic`: the default. Uses the Anthropic SDK and requests JSON output against the schema.
  - `openai-compatible`: a Should goal. It also covers many hosted and self-hosted models.
  - `fake`: deterministic, for tests.
- **Configuration per task:** `LLM_<TASK>_PROVIDER`, `LLM_<TASK>_MODEL`, and `LLM_<TASK>_TIMEOUT_MS`. Defaults:
  - `screen_request` and `detect_emergency`: `claude-haiku-5-5`.
  - `structure_request` and `suggest_profile`: `claude-sonnet-5-5`, dropping to Haiku if the latency budget requires it.
- **What the port does not allow:**
  - Domain code cannot use vendor features such as provider-specific tool-use shapes, prompt-cache directives, or system-prompt quirks.
  - Prompts are provider-neutral Markdown templates in `packages/llm/prompts/<task>/<version>.md`. An optional per-provider override file is allowed when the eval harness shows it is needed.
- **Swapping provider** takes four steps:
  1. Implement or enable an adapter.
  2. Run `evals` for every task on that adapter.
  3. Pass the gates: AC-7 recall, AC-8 emergency recall, and the AC-25 latency check.
  4. Flip the configuration.

  No migration is needed, because decisions record the provider and model.
- **Data minimization before any LLM call:**
  - Regex redaction of phone numbers, emails, and Israeli ID numbers.
  - The structured address is never sent; only free-text descriptions are.
  - No account identifiers are sent.

### 3.7 Location model

The rule that address precision never reaches a third party before confirmation (§5.2) is met by **never geocoding addresses at all**:

- The pilot city's neighbourhoods ship as a GeoJSON polygon set in `packages/policy/jurisdictions/il/areas/<city>.geojson` (Q1).
- Requesters pick a neighbourhood. The exact address is entered as text and stored only in the vault.
- Helpers pick a home neighbourhood and a travel radius.
- Distance is computed with PostGIS from neighbourhood centroids.
- Maps render neighbourhood polygons only.

### 3.8 Discovery, matching, and notifications

- **Feed ranking** is a deterministic SQL scoring function over:
  - capability and interest overlap;
  - availability-window overlap;
  - distance from centroid to centroid, compared with the travel radius;
  - exclusions from private preferences;
  - safety-requirement fit.

  **Urgency is not an input to the function** (AC-17).
- **Ineligible requests** (outside the travel radius, an excluded activity, or an Organization request the viewer can't fill) are returned with a `reason` code instead of being filtered out (AC-18).
- **Search** uses Postgres full-text search with the `simple` configuration plus `pg_trgm`, because Postgres has no Hebrew stemmer. This is enough for the pilot's volume.
- **Notifications on publish:**
  - The publish transaction writes an outbox job.
  - The worker selects opted-in Helpers whose eligibility and fit pass a threshold, respecting quiet hours and a per-user daily cap.
  - It sends through APNs and FCM directly, using native tokens from `expo-notifications`, so there is no extra relay processor.
  - Budget: under 5 s typically, 60 s at p95. The send time is recorded for AC-25.
  - Payloads carry IDs and generic text only, with no need details.
- **Raising a hand** is one `POST /requests/:id/interests` call (one tap), protected by an idempotency key.

### 3.9 Messaging

- Threads are always bound to a `HelpRequest` (and optionally to an Interest). No API creates a free-standing thread (FR-8).
- An Availability Offer inquiry requires choosing a specific Help Request owned by the requester (FR-6).
- Delivery is push plus a fetch when the thread opens, polling every 5 s while it is open. WebSocket is a Nice goal.
- Leaving a thread before confirmation sets a flag on the participant. It sends no notification to the other party.

### 3.10 Engagement core (confirmation, terms, lifecycle)

**Records involved:**

- **Interest:** created when a Helper raises a hand, scoped to a request (and to a role and shift if it is a group request). State is `active` or `withdrawn`. A withdrawal is neutral: it is never surfaced to anyone or counted (AC-2).
- **EngagementTerms (versioned):** the canonical terms object.
  - Always: `what`, `when`, `where_area`.
  - Only when they apply: `expense_terms`, `role_id`/`shift_id`, `safety_categories` plus the attestation text version, `named_attendees[]`.
  - Each version stores a `content_hash` (SHA-256 of the canonical JSON).
- **TermsConfirmation:** `(terms_version_id, party_account_id, actor_individual_id, confirmed_at)`, unique per party per version.

**Confirmation flow:**

1. The requester taps "Confirm <Helper>". In one call, the API creates terms v1 from the request and records the requester's confirmation.
2. The Helper receives a push and confirms with one tap, passing `terms_version_id` and `content_hash`.
3. The API runs the DB function `confirm_and_create_engagement()` in a serializable transaction. The function:
   - verifies two confirmations from the two distinct parties on the same version and hash;
   - verifies the required attestations and named attendees exist;
   - locks the group slot and checks capacity (`SELECT … FOR UPDATE`);
   - inserts the `Engagement`.

**A deferred constraint trigger on `engagements` rejects any insert that lacks those confirmations**, so no code path can bypass it (AC-1).

**Material change (AC-3):**

- Either party proposes terms vN+1, which receives a confirmation from the proposer.
- `engagements.current_terms_version_id` moves only when the counterparty confirms the same hash.
- If the change touches safety conditions, both parties must attest again.
- If it touches expenses, the new terms are a new version like any other (FR-19).

**Lifecycle**, spanning the Interest and Engagement records:

```
Interest:   active ──withdraw──▶ withdrawn (neutral, not counted)
               │
               └─both confirm same terms──▶ Engagement: confirmed
Engagement: confirmed ──start (either party)──▶ in_progress
            confirmed ──scheduled end + grace passes, never started──▶ expired (worker)
            confirmed | in_progress ──cancel (either party, reason)──▶ cancelled  [notice period recorded]
            confirmed | in_progress ──counterparty reports──▶ no_show  [disputable]
            in_progress ──Helper reports──▶ completed | partially_completed | not_completed
            outcome ──Requester──▶ outcome_status: confirmed | disputed ; no response in 7 d ──▶ unconfirmed
```

- The PRD lists `interest_expressed` and `withdrawn` as Engagement states. This plan models them on **Interest**, so that FR-9's rule "an Engagement exists only after both confirm" holds literally. The product vocabulary is unchanged.
- Every transition appends an `engagement_events` row recording actor, acting account, timestamp, from-state, to-state, and context.
- `engagements` is a projection maintained in the same transaction, and can be rebuilt from the events.

**Safety-sensitive requests (FR-15):**

- Confirmation requires one `Attestation` per party per engagement, linked to `(terms_version_id, attestation_text_version)`.
- When there is evidence, the requester must also acknowledge it (`EvidenceAcknowledgement`).
- Ordinary requests render none of these steps (AC-26).

**Organization as Helper (FR-2):**

- `named_attendees` is part of the terms, so it is covered by the hash.
- Confirmation is impossible with zero attendees (AC-23).
- The requester's confirm screen shows each attendee's display name and identity status.

**Group requests:** each Helper gets an individual Engagement against one role and shift slot. Records are independent by construction: every event and count is keyed by the engagement, never by the group (AC-16). The request's state is `filled` when every slot is full, and it reopens on a cancellation.

### 3.11 Disclosure service

`DisclosureService.getContact(viewer: Principal, engagementId)` is the **only** code path that reads the `vault` schema. Policy:

| Engagement state | Who may read address + phone |
|---|---|
| no engagement / interest only | nobody |
| `confirmed`, `in_progress` | the counterparty (Individual), **or** the Organization's handling operator + named attendees |
| closed (`completed`, `partially_completed`, `not_completed`, `cancelled`, `no_show`, `expired`) | same parties, until `closed_at + 14 days` (policy-configurable) |
| after window | nobody (factual record remains) |

- **Each successful read writes a `DisclosureEvent`.** The first disclosure on confirmation also sends in-app notices to both parties (FR-10).
- **Database enforcement:**
  - The `app` role has no grants on `vault`.
  - The disclosure module uses a separate connection pool with the `vault_reader` role.
  - Vault columns are encrypted at the application layer with the owner's data key (§4.4).
- **Evidence documents:**
  - The disclosure service also issues S3 pre-signed GET URLs (60 s expiry), only to counterparties within the engagement's context.
  - Every surface that shows evidence renders it through one shared component that always shows the unverified label (AC-10).

### 3.12 Trust, safety, and console

- **Feedback (FR-13):**
  - Accepted only from a party to a closed engagement.
  - Visibility to the counterparty is computed: both have submitted, or 7 days have passed since closure.
  - Free text is visible only to the counterparty and to moderators.
- **Public aggregates (FR-14):**
  - Recomputed by the worker from events and feedback into `trust_aggregates`, which is derived and has no write API.
  - Dimensions with fewer than 3 ratings are suppressed when the record is serialized.
  - No composite field exists in the schema or the contracts (AC-14).
- **Reports (FR-17):**
  - Categories come from policy.
  - A report is safety-routed if its category is physical safety or the engagement is in progress.
  - SLA timers: 4 h for safety reports, 2 business days otherwise. A safety report sends an SMS and email page to the on-call operator.
  - A restriction is a `ModerationAction` with a rationale the user can see, plus an appeal path.
- **Exploitation patterns (FR-18):**
  - The worker runs after each confirmation and nightly.
  - It aggregates scheduled duration and engagement count per requester–Helper pair and per Organization over rolling windows.
  - Thresholds live in the `policy_settings` table and are edited in the console with an audit record (Q9).
  - Crossing a threshold creates a `PatternReviewItem`. It never blocks anyone automatically.
- **Console:**
  - A React SPA for operators, which is a separate role from end users.
  - Login uses email, password, and TOTP MFA, plus an optional IP allowlist.
  - Queues: Organization validation, appeals, reports, pattern items, verification reviews, and account-type change requests.
  - Each queue shows SLA countdowns. Every action requires a rationale.

### 3.13 Localization and accessibility

- **Localization:**
  - Mobile uses `i18next` with Hebrew and English bundles.
  - Language is chosen in the app. Switching direction calls `I18nManager.forceRTL` and reloads the app (`expo-updates` `reloadAsync`), independent of the device locale (FR-20).
  - Server texts (block explanations, attestation texts, moderation reasons) are policy-versioned and localized. The API returns keys plus parameters, and the client renders them.
- **Accessibility:**
  - Logical start/end styles only, with no left/right.
  - Minimum touch target of 44 pt.
  - `allowFontScaling` everywhere, with layouts tested at the largest dynamic-type size.
  - No gesture-only actions.
  - `eslint-plugin-react-native-a11y` in CI.

### 3.14 Observability and metrics

- **Logs:** structured JSON (`pino`) shipped to CloudWatch Logs, with PII redaction serializers.
- **Crash reporting:** Sentry on the free developer plan, with PII scrubbing on and no request bodies.
- **Product events:** an `analytics_events` table written server-side, for example `request_started`, `request_published`, `interest_raised`, `engagement_confirmed`, and `notification_sent`.
- **Client timing:** the client sends `request_started_at` so time-to-publish is measured client-to-server (AC-24, §8).
- **Metric views:** §8 metrics are SQL views shown on a console metrics page, covering:
  - time to a confirmed helper;
  - confirmation and completion rates;
  - unanswered requests after 24 h;
  - screening p95;
  - notification p95;
  - appeal overturn rate.

---

## 4. Data Model

These are proposed Postgres tables in the `app` schema unless marked otherwise. All IDs are UUIDv7, and all timestamps are `timestamptz`. Names are proposals and will be finalized in task T03.

### 4.1 Accounts and identity

| Table | Key fields | Notes |
|---|---|---|
| `accounts` | id, type (`individual`\|`organization`), status (`active`\|`restricted`\|`deleted`), created_at | `type` immutable except via console support action (audited) |
| `individual_logins` | account_id, phone_e164_enc, email_enc, phone_verified_at, email_verified_at, identity_tier (0\|1), tier1_status, locale | PII columns encrypted with user DEK |
| `identity_verifications` | id, account_id, provider, vendor_ref, outcome, reason_code, attempt_no, created_at | append-only |
| `organizations` | account_id, legal_name, legal_form (`amuta`\|`chalatz`), registry_number, validation_status (`pending`\|`validated`\|`rejected`), validated_at, validation_method (`auto`\|`manual`), proper_mgmt_cert_until, owner_individual_id | |
| `organization_operators` | id, org_account_id, operator_individual_id, added_by, added_at, removed_at | equal permissions in v1 |
| `refresh_tokens`, `invite_codes` | standard | |

### 4.2 Profiles

| Table | Key fields | Notes |
|---|---|---|
| `profiles` | account_id, display_name, photo_key, city, age_band, visibility flags, bio, org mission/areas/logo | public-facing fields only |
| `capability_claims` | id, account_id, kind (`capability`\|`experience`\|`tool`\|`language`\|`interest`), value, source (`self`\|`ai_suggested`\|`evidence`), status (`unconfirmed`\|`active`), verification (`unverified` only in v1) | AI suggestions inserted `unconfirmed`, not public (AC-5) |
| `private_preferences` | account_id, excluded_activities[], constraints jsonb, comfort_limits jsonb | read only by `discovery`; no output schema includes it (AC-12) |
| `helper_settings` | account_id, home_area_id, travel_radius_km, availability_windows jsonb, notify_opt_in, quiet_hours | |

### 4.3 Requests, offers, screening

| Table | Key fields |
|---|---|
| `help_requests` | id, requester_account_id, created_by_individual_id, state, what_text, area_id, window_start, window_end, urgency (display only), desired_outcome, effort_estimate, tools, physical_conditions, expense_terms jsonb, safety_categories[], is_group, structured_by (`ai`\|`manual`\|`copy`), held_reason, published_at |
| `request_roles`, `request_shifts`, `request_slots` | slot = (role_id, shift_id, capacity) |
| `request_addresses` (**vault**) | request_id, address_enc, access_notes_enc |
| `availability_offers` | id, account_id, kinds[], area_id, radius_km, windows jsonb, capabilities[], state |
| `screening_decisions` | id, request_id, outcome (`published`\|`blocked`\|`emergency_redirect`\|`unavailable`), stage_results jsonb, matched_category, confidence, ruleset_version, policy_version, provider, model, prompt_version, input_hash, latency_ms — **append-only** |
| `appeals` | id, request_id, screening_decision_id, text, status, sla_due_at, resolved_by, rationale |
| `areas` | id, jurisdiction, city, name_he, name_en, geom (PostGIS polygon), centroid |

**Help Request state machine:**

```
draft ──submit──▶ screening ──▶ published | blocked | draft(held_reason=screening_unavailable)
blocked ──edit+resubmit──▶ screening ;  blocked ──appeal overturned──▶ published
published ──all slots confirmed──▶ filled ──cancellation──▶ published
published | filled ──requester──▶ closed | withdrawn ;  published ──window_end passed──▶ expired
```

Emergency redirects never leave `draft`. They are recorded only as a `ScreeningDecision`.

### 4.4 Engagements, disclosure, trust, safety

| Table | Key fields | Mutability |
|---|---|---|
| `interests` | id, request_id, slot_id, helper_account_id, actor_individual_id, state, created_at | state flag only; events in `engagement_events` |
| `engagement_terms` | id, request_id, interest_id, version, terms jsonb, content_hash, proposed_by | **append-only** |
| `terms_confirmations` | terms_id, party_account_id, actor_individual_id, confirmed_at | **append-only**, unique (terms_id, party) |
| `engagements` | id, request_id, slot_id, requester_account_id, helper_account_id, handling_operator_id, current_terms_id, state, outcome, outcome_status, scheduled_start, closed_at | projection; insert guarded by constraint trigger |
| `engagement_events` | id, engagement_id/interest_id, from_state, to_state, actor_individual_id, acting_account_id, reason, context jsonb, at | **append-only** |
| `attestations` | id, engagement_terms_id, party_account_id, actor_individual_id, category, text_version, attested_at | **append-only** |
| `evidence_documents` | id, owner_account_id, request_id, s3_key, mime, label (`user_provided_unverified`), uploaded_at | file in S3 |
| `evidence_acknowledgements` | evidence_id, terms_id, requester_actor_id, at | **append-only** |
| `named_attendees` | stored inside terms jsonb (hashed) + denormalized `engagement_attendees(engagement_id, individual_id)` for disclosure checks | |
| `disclosure_events` (**audit**) | id, engagement_id, viewer_individual_id, data_class, at | **append-only** |
| `message_threads`, `thread_participants`, `messages` | thread bound to request_id (NOT NULL); body_enc with author DEK | messages immutable |
| `feedback` | id, engagement_id, author_account_id, ratings jsonb (6 fixed dims, 1–5), comment_enc, submitted_at | immutable |
| `trust_aggregates` | account_id, completed_as_requester, completed_as_helper, cancellations, no_shows, total_engagements, dim_counts/dim_avgs jsonb | derived only |
| `reports`, `moderation_actions`, `restrictions`, `pattern_review_items` | category, target, sla_due_at, rationale, actor | actions **append-only** |
| `audit.audit_log` | id, actor_individual_id, acting_account_id, action, target, rationale, metadata, at | **append-only** |
| `policy_settings` | key, value, version, changed_by, changed_at | history kept in audit |
| `feature_flags` | key, enabled, rollout jsonb | |

**How "append-only" is enforced:** the `app` role is granted only `INSERT` and `SELECT` on these tables, and a `BEFORE UPDATE OR DELETE` trigger raises an exception as defence in depth. A correction is a new row, for example an `engagement_events` row of type `correction` that references the original.

### 4.5 Encryption and deletion (AC-13)

**Envelope encryption:**

- Each account has a **data encryption key** (DEK, AES-256-GCM), stored in `vault.account_keys`.
- Each DEK is wrapped by one KMS customer-managed key. A single CMK keeps KMS cost to about $1 per month.
- The DEK encrypts the account's PII:
  - phone, email, legal name if present;
  - addresses, access notes;
  - message bodies, feedback comments;
  - identity vendor references.
- Evidence files in S3 are deleted directly.

**Deletion request:**

1. The account `status` becomes `deleted`.
2. The profile is replaced by the "Former member" tombstone, and photo and evidence objects are deleted from S3.
3. **The wrapped DEK row is deleted (crypto-shred).** All ciphertext belonging to the account becomes unreadable, everywhere, at once.
4. The free text of the user's own Help Requests and Offers is redacted. These are content, not engagement history.
5. Engagement events, terms hashes, counts, and the counterparty's received feedback ratings remain, keyed by the opaque account UUID. **The counterparty's history and aggregates are therefore unchanged.**
6. RDS automated backups expire within 7 days, so erasure is complete within that window. The privacy notice will state this.

**Retention:** per-data-class periods come from `packages/policy` (Q8). A nightly `privacy` job enforces them. For example, identity vendor references are purged after verification plus N days.

### 4.6 Migrations

- SQL-first migrations live in `packages/db/migrations`, run through Kysely's migrator in a deploy step before the API starts.
- Migrations are forward-only. Breaking changes follow expand → migrate → contract.
- Since this is greenfield, there are no backfills, but seed data is needed: the pilot city's areas, taxonomy, and attestation texts are loaded from `packages/policy` by an idempotent seed.

---

## 5. Security

- **Authentication:**
  - End users sign in with an SMS OTP to the phone, plus a verified email. OTP sends are rate-limited per phone, device, and IP.
  - Access JWT expires in 15 min. Refresh tokens rotate and are revocable, and reuse of a refresh token revokes the whole token family.
  - Operators authenticate separately (§3.12) and cannot use mobile tokens.
- **Authorization:**
  - A central `authorize(principal, action, resource)` policy, unit-tested as a matrix covering Individual, Organization operator, counterparty, and moderator.
  - Organization actions require an active operator membership. Removing an operator takes effect on the next request, because the operator check is never cached in the JWT.
- **Disclosure and privacy:**
  - Vault access is limited to one module, one database role, and one service, and every read is logged (§3.11).
  - Response DTOs are explicit Zod output schemas. Serialization strips any key that is not in the schema.
  - A CI test enumerates every route reachable by a non-owner and asserts no private field appears in its schema (AC-12).
  - Notification payloads and logs contain no need details, addresses, or phone numbers.
- **Encryption:**
  - TLS everywhere.
  - RDS and S3 encrypted with KMS.
  - Application-layer encryption for vault and PII columns (§4.5).
  - Secrets held in SSM Parameter Store as SecureString, read by the instance role. No secrets in the repo or in EAS environment variables beyond public configuration.
- **Network:** RDS sits in a private subnet. The EC2 security group allows only 443 inbound. SSH is disabled; administrative access is through SSM Session Manager.
- **Audit:** every moderation action, screening decision, appeal outcome, attestation, disclosure event, Organization action, account-type change, and policy-setting change is written to `audit.audit_log` with actor, acting account, timestamp, and rationale (§5.3).
- **Abuse resistance:**
  - Postgres-backed rate limits (`rate-limiter-flexible`) on request creation, interests, reports, OTP, and messages.
  - Feedback only from parties to a closed engagement.
  - Invite-gated sign-up for the closed pilot.
- **LLM-specific risks:**
  - *Prompt injection in descriptions:* the rules stage is an independent backstop, blocking is a union, and outputs are schema-validated. Injection probes are part of the eval set.
  - *PII sent to the provider:* redacted before the call.
  - *Vendor data use:* the vendor's retention and no-training terms are to be confirmed under a DPA (counsel, Q5).
- **Uploads:**
  - Evidence and logos only, with MIME allowlist (JPEG, PNG, PDF) and a 10 MB cap.
  - Uploads go to S3 through a pre-signed PUT bound to the content type, under a key namespaced per account.
  - EXIF data is stripped from images in the worker.
- **Safe defaults:**
  - Notification opt-in is off until the user chooses.
  - Profile fields are private unless toggled public.
  - Screening fails closed.
  - Tier 1 is off unless the vendor is configured.

---

## 6. Testing Strategy

The test stack is new, so this plan sets the conventions.

| Layer | Tooling | What it must prove |
|---|---|---|
| Unit | Vitest | policy evaluation, ranking function, terms canonicalization/hash, rules-stage taxonomy, authorization matrix |
| DB invariants | Vitest + Testcontainers (Postgres 16 + PostGIS) | AC-1: direct `INSERT` into `engagements` without two matching confirmations fails; append-only tables reject `UPDATE`/`DELETE`; `app` role cannot read `vault`; capacity cannot be exceeded under concurrent confirms |
| State machines | fast-check property tests | random command sequences never yield an illegal transition, never create an engagement from one confirmation, material change never alters current terms before both confirm (AC-3) |
| API / integration | Vitest + Fastify `inject` against Testcontainers | every FR flow end-to-end on the server; idempotency; acting-as attribution (AC-22); org attendee rule (AC-23); account-type immutability (AC-21) |
| Disclosure matrix | generated tests | every viewer role × every engagement state × every data class → allowed/denied exactly as §3.11, including Organization non-handling operators denied (AC-11) |
| Contract / privacy | route-schema scanner | no private-preference, vault, free-text-feedback, or composite-score field in any non-owner output schema (AC-12, AC-14, AC-15) |
| Ranking | fixture test | two feeds identical except urgency produce identical order (AC-17); ineligible items carry reasons (AC-18) |
| AI evals | `evals/` harness, runs against any adapter | prohibited recall ≥95% per language (AC-7); emergency detection recall target 100% on the labelled set (AC-8); precision tracked for overturn-risk; latency p95 ≤10 s screening; structuring field accuracy. Run on every change to prompts, policy taxonomy, model, or provider, and nightly against prod config. |
| Failure injection | integration | LLM adapter timeout/error → request held, never published (AC-6); structuring down → manual form works |
| Load / latency | k6 against staging | screening p95 ≤10 s and notification p95 ≤60 s at 5× expected pilot peak (AC-25) |
| Mobile unit/component | Jest + React Native Testing Library | forms, review screen, confirm button states, unverified-evidence label everywhere (AC-10) |
| Mobile E2E | Maestro on iOS simulator + Android emulator | sign-up (both types), ask → publish, raise hand, confirm (one tap each, AC-26), safety-sensitive attest, group request, feedback; each in `he` (RTL) and `en` |
| Accessibility | manual on device + a11y lint | VoiceOver and TalkBack through all core flows, largest dynamic type, RTL (AC-20) |
| Deletion | integration | crypto-shred makes PII unreadable; counterparty aggregates unchanged; tombstone rendered (AC-13) |
| Usability | moderated sessions | AC-24 time-to-publish with pilot-representative and elderly groups |

**CI:** GitHub Actions runs lint, type checks, `dependency-cruiser` boundaries, unit tests, DB tests, and API tests on every PR. Maestro and evals run on the main branch and nightly. Evals also run on any PR touching `packages/llm`, `packages/policy`, or `evals/`.

---

## 7. Rollout Plan

- **Environments:**
  - `local`: Docker Compose with Postgres + PostGIS, the api and worker, and the fake LLM.
  - `staging`: a small EC2 instance and RDS, stopped outside working hours to save credits.
  - `prod`: AWS `il-central-1`.

  All three are defined in CDK.
- **Cost guard:**
  - AWS Budgets alerts at 50%, 80%, and 100% of the remaining credits.
  - Anthropic usage limit set in the console.
  - Expected pilot run rate is about $50–80 per month for AWS, plus LLM and SMS usage.
  - If the credits will not last the 3 months, the fallback is the same containers on Supabase Postgres plus a free-tier host. The code is portable because it uses plain Postgres and has no AWS-only logic outside `infra/` and the S3/KMS adapters.
- **Feature flags** (DB-backed): `tier1_verification`, `llm_structuring`, `ai_profile_suggestions`, `org_auto_validation`, `availability_offers`, `group_requests`. The flags let the team dark-launch and kill-switch features without a release.
- **Deploy sequence:**
  1. Run migrations.
  2. Seed policy and areas.
  3. Roll the api and worker containers, keeping the previous image tag for rollback.
  4. Ship the mobile build through TestFlight and Google Play closed testing, then the stores.
  5. Push JS-only fixes through EAS Update.
- **Rollback:**
  - Backend: redeploy the previous image tag. Migrations are additive, so the old code keeps working.
  - Mobile: roll back with EAS Update.
  - LLM: switch provider or model by configuration.
  - Data: RDS point-in-time restore. The disaster-recovery runbook targets an RTO of 4 h and an RPO of 5 min.
- **Launch gates:** all must pass before the first real user.
  1. The Hebrew and English screening eval passes AC-7 and AC-8 on the Q6 taxonomy dataset, at production latency (AC-25).
  2. The disclosure matrix, append-only, and AC-1 invariant suites are green, and a security review of the vault path is done.
  3. The deletion model is tested and signed off by counsel (AC-13, Q8).
  4. Counsel signs off on Q4 (liability and insurance) and Q5 (privacy law, DPA with the LLM provider, data residency).
  5. VoiceOver, TalkBack, and RTL are verified on device (AC-20).
  6. Moderation staffing and out-of-hours safety escalation are in place (Q10). The on-call page has been tested.
  7. Pattern thresholds and retention periods are recorded in the ops runbook (Q8, Q9).
  8. The AC-24 usability test is passed.
- **Pilot operations:**
  - Partners onboard through invite codes.
  - The console metrics page is reviewed weekly against §8.
  - Every incident is individually reviewed.
- **Support:** in-app "Contact support" goes to a shared mailbox. Account-type change requests are handled in the console (Q13).

---

## 8. Milestones

Each milestone is deployable to staging and demoable. Only M6 goes to real users.

| # | Milestone | Contents | Exit |
|---|---|---|---|
| M0 | Foundations | monorepo, CI, CDK infra, DB roles, auth skeleton, policy package, LLM port + fake | staging deploy from CI; invariants test harness running |
| M1 | Accounts & profiles | Individual/Org sign-up, operators, acting context, profiles, claims, private prefs, org validation (manual) | AC-21, AC-22 green |
| M2 | Ask | structuring, manual creation, review screen, screening + emergency path, appeals intake, eval harness | AC-6, AC-7 (EN), AC-8 (EN) green; Hebrew eval in progress |
| M3 | Find & talk | areas, feed, search, eligibility reasons, notifications, interests, request threads, Availability Offers | AC-17, AC-18, AC-25 (notifications) green |
| M4 | Engage | terms, one-tap confirmation, lifecycle, disclosure vault, group slots, safety-sensitive attestation + evidence, expenses | AC-1 – AC-3, AC-9 – AC-11, AC-16, AC-23, AC-26 green |
| M5 | Trust & safety ops | feedback, aggregates, reports, moderation, pattern detection, console queues + SLAs, deletion/retention | AC-12 – AC-15, AC-19 green |
| M6 | Pilot readiness | Hebrew eval gate, a11y/RTL pass, load test, usability test, launch gates, store release | all launch gates (§7) passed |

---

## 9. Risks

- **Hebrew screening misses AC-7 at the 10 s budget (A4, Q11).**
  - Mitigation: the rules stage plus union blocking, with the fast model tier by default.
  - The eval harness starts in M2, not M6.
  - Fallback: a stronger model within the budget, or an expanded Hebrew rules list.
  - Go/no-go at M2 exit plus 2 weeks.
- **Over-blocking suppresses legitimate requests.** Union blocking raises recall but costs precision.
  - Mitigation: track precision in evals and the appeal overturn rate in production.
  - Tune rules first. Appeals keep a human path open.
- **Disclosure leak (highest reputational risk).**
  - Mitigation: one-module vault, a separate DB role, app-layer encryption, generated matrix tests, schema-scanner tests, and a security review as a launch gate.
- **State machine or terms bugs invalidate the pilot.**
  - Mitigation: DB-level constraint trigger, property-based tests, and an events-first design with a rebuildable projection.
- **The LLM provider changes, degrades, or has an outage.**
  - Mitigation: a provider-agnostic port with a second adapter (a Should goal), eval-gated swaps, and configuration-only switching.
  - Screening fails closed with a clear message.
  - Structuring always has a manual fallback.
- **AWS credits run out or are smaller than expected.**
  - Mitigation: budgets and alerts, staging stopped off-hours, portable containers and plain Postgres, and the Supabase fallback (§7).
- **Single-host, single-AZ outage.**
  - Mitigation: automated RDS backups with PITR, CDK-reproducible infrastructure, a 4 h RTO runbook, and an EC2 auto-recovery alarm. This is an accepted trade-off for the pilot.
- **Regional service availability in `il-central-1`.** Some AWS services (for example SES or End User Messaging SMS) may not be offered there.
  - Mitigation: notification providers sit behind adapters.
  - Email and SMS can use another region or a third-party provider (for example Twilio Verify), because they carry only OTPs and generic text.
- **SMS cost or deliverability in Israel.**
  - Mitigation: the OTP provider sits behind an adapter, and sends are rate-limited.
  - Email OTP is available as a secondary channel for logins that are already verified.
- **Registry data cannot support automated validation (Q12).**
  - Mitigation: manual validation in the console from M1, with automation as a Should goal.
- **No identity vendor (Q3, A3).**
  - Mitigation: Tier 1 sits behind a flag, and the pilot runs with Tier 0 labels as the PRD allows.
  - Organization owners get manual document review in the console instead.
- **Hebrew on-device speech recognition quality varies by device.**
  - Mitigation: text input is always available, and the transcript can be edited before structuring.
- **Changing RTL requires an app reload.**
  - Mitigation: switch language once at onboarding and from settings, with a clear "the app will restart" notice.
- **AI coding drift on invariant-critical code.**
  - Mitigation: tasks T04 and T13 land the DB invariants and their tests first. Later tasks must keep those suites green. Boundary rules are enforced by `dependency-cruiser`.
- **Deletion versus backups.**
  - Mitigation: crypto-shredding plus 7-day backup expiry, documented in the privacy notice.
  - Counsel confirms this is acceptable (launch gate 3).

---

## 10. Assumptions

- **Hosting:** AWS new-account credits, possibly with Activate, cover about $50–80 per month of AWS for the 3-month pilot plus staging. The product owner chose AWS with credits.
- **Regional services:** EC2, RDS PostgreSQL with PostGIS, S3, KMS, SSM, and CloudWatch are available in `il-central-1`. Email and SMS may need another region or provider. This has not been verified.
- **Data residency:** keeping primary data in Israel satisfies the pilot's requirements, and sending redacted request text to the Anthropic API (US-hosted) is permissible under a DPA. This needs counsel confirmation (Q5).
- **Anthropic models and terms:** `claude-haiku-5-5` meets the screening latency budget and `claude-sonnet-5-5` the structuring budget. The API terms allow commercial use without training on inputs.
- **Pilot city areas:** neighbourhood-level polygons for the pilot city can be obtained (municipal open data or drawn manually) once Q1 is answered.
- **Registry data:** an Israeli nonprofit registry data source (for example the data.gov.il registrar datasets) is accessible for עמותות and חל"צ status lookups. If it isn't, validation is manual (Q12).
- **Organization sign-up:** it creates or links the owner's Individual login. An Organization account is never a login in its own right (PRD A2).
- **Pilot scale:** under 5,000 accounts and under 50 concurrent active users at peak. One small instance and Postgres-only search and queueing are enough.
- **Expiry rules:** an engagement that is never started expires 24 h after its scheduled end. A Helper-reported outcome becomes `unconfirmed` if the requester doesn't respond within 7 days. These values are not in the PRD; both are configurable in policy.
- **Thread delivery:** polling while a thread is open is acceptable for pilot chat responsiveness.
- **Pending PRD inputs:** the Q6 taxonomy dataset and the Q8 retention periods will arrive before M2 and M5 respectively. Until then, engineering uses placeholder policy files marked `DRAFT`.
- **Team:** a small team of about 2–4 engineers, all comfortable with TypeScript. Milestones are sequenced, not dated.

---

## 11. Task Breakdown

Tasks are sequenced. "Depends on" lists hard prerequisites. Every task must keep the existing test suites green.

**T01 · Monorepo and CI scaffold**
- **Goal:** pnpm workspaces plus Turborepo with `apps/{mobile,api,console}`, `packages/{contracts,policy,llm,db}`, `infra/`, and `evals/`. Shared tsconfig, ESLint, and Prettier. `dependency-cruiser` module-boundary rules. GitHub Actions running lint, typecheck, and tests.
- **Depends on:** none.
- **Scope:** repo root, all package skeletons.
- **Finished when:** `pnpm build` and `pnpm test` pass in CI, and an illegal cross-module import fails CI.
- **Validation:** CI run on a PR, plus a deliberate boundary-violation check.

**T02 · AWS infrastructure (CDK)**
- **Goal:** VPC, EC2 (Docker, Caddy), RDS Postgres 16 with PostGIS, S3 with SSE-KMS, a KMS CMK, SSM parameters, CloudWatch log groups and alarms, and AWS Budgets alerts. `staging` and `prod` stacks in `il-central-1`.
- **Depends on:** T01.
- **Scope:** `infra/`.
- **Finished when:** `cdk deploy` creates staging, the API health check is reachable over TLS, RDS is private, and budget alerts are configured.
- **Validation:** the deploy succeeds, a connectivity smoke test passes, and a public port scan shows only 443.

**T03 · Database foundation and roles**
- **Goal:** the Kysely migrator. Schemas `app`, `vault`, `audit`, and `jobs`. Roles `app`, `vault_reader`, and `migrator` with their grants. Base tables for §4.1 and `audit_log`. A UUIDv7 helper. A reusable append-only trigger.
- **Depends on:** T01.
- **Scope:** `packages/db`.
- **Finished when:** migrations apply from empty, the `app` role cannot select from `vault`, and append-only tables reject `UPDATE`/`DELETE`.
- **Validation:** Testcontainers DB tests.

**T04 · Envelope encryption and crypto-shred primitives**
- **Goal:** per-account DEK creation, wrapped by KMS (with a local KMS stub for tests). `encrypt`/`decrypt` helpers for PII columns. A `shred(accountId)` primitive.
- **Depends on:** T03.
- **Scope:** `apps/api/src/modules/privacy`, `packages/db`.
- **Finished when:** PII round-trips, and after `shred` decryption fails while non-PII rows remain.
- **Validation:** unit and integration tests.

**T05 · Policy package (jurisdiction configuration)**
- **Goal:** a loader with schema validation for `jurisdictions/il`, covering:
  - emergency numbers and patterns (he/en);
  - prohibited and safety-sensitive taxonomy, as a `DRAFT` placeholder until Q6;
  - versioned attestation texts and block-explanation texts;
  - retention periods, pattern thresholds, and disclosure window;
  - adapter selection and pilot areas GeoJSON.

  Plus a seed command for the `areas` table and the `policy_settings` table.
- **Depends on:** T03.
- **Scope:** `packages/policy`, seed script.
- **Finished when:** the API boots with policy version X loaded, invalid policy fails at boot, and areas are seeded with PostGIS geometry.
- **Validation:** unit tests on the loader and the seed's idempotency.

**T06 · Auth, accounts, and acting context**
- **Goal:**
  - Invite-gated sign-up with a required Individual/Organization choice.
  - Phone OTP (through an adapter) and email verification, with JWT and refresh rotation.
  - Organization creation with the owner login, `pending_validation` state, and operator add/remove.
  - The `X-Acting-As` principal resolution.
  - Audit attribution on every command.
  - Rate limits on OTP sends.
- **Depends on:** T03, T04, T05.
- **Scope:** `modules/identity`, `audit`, `packages/contracts`.
- **Finished when:** AC-21 holds (no type, no account; no API changes the type). Every Organization command logs both actor and acting account (AC-22, attribution half).
- **Validation:** API integration tests and the authorization matrix unit tests.

**T07 · Mobile app shell**
- **Goal:**
  - Expo app with expo-router, the auth flow, and secure token storage.
  - i18next with he/en, and an in-app language switch with RTL reload.
  - Typed API client from `packages/contracts`.
  - Accessibility lint, plus a design-system primitives package with 44 pt targets and dynamic type.
  - EAS Build profiles.
- **Depends on:** T06.
- **Scope:** `apps/mobile`.
- **Finished when:** sign-up and login work on both platforms against staging in both languages, and the RTL switch works.
- **Validation:** Jest/RNTL tests, Maestro sign-up flows in he and en, and a manual VoiceOver/TalkBack smoke test.

**T08 · Profiles, capability claims, private preferences**
- **Goal:**
  - Individual and Organization profile CRUD with public visibility controls.
  - Capability claims with source and verification status.
  - Private preferences stored and readable only by the owner and `discovery`.
  - Helper settings (home area, radius, availability, notification opt-in).
  - Mobile screens.
- **Depends on:** T06, T07.
- **Scope:** `modules/profiles`, mobile profile screens.
- **Finished when:** a minimal profile is sufficient to ask or help. Claims display source and status, and an AI-sourced claim is never public while `unconfirmed`.
- **Validation:** API tests, plus the first version of the route-schema privacy scanner (AC-12).

**T09 · Organization validation (manual first)**
- **Goal:**
  - Console login with TOTP for operators of Handraise.
  - An Organization validation queue with a 2-business-day SLA timer, approve/reject with rationale, and the proper-management certificate badge.
  - Publishing is blocked until the Organization is validated.
  - A `RegistryAdapter` interface with a stub.
- **Depends on:** T06.
- **Scope:** `apps/console`, `modules/identity`.
- **Finished when:** an unvalidated Organization cannot publish a request or offer (AC-22, publish half), and validation is audited.
- **Validation:** API and console tests.

**T10 · LLM port and eval harness**
- **Goal:**
  - The `LlmClient` interface, with `anthropic` and `fake` adapters and per-task configuration.
  - Prompt versioning and output validated by schema.
  - A PII redactor.
  - A timeout and circuit breaker.
  - `evals/` harness: dataset format (he/en, labelled), metrics (recall and precision per category, emergency recall, latency p50/p95), and a CLI `pnpm eval --task screen_request --provider anthropic`.
- **Depends on:** T01, T05.
- **Scope:** `packages/llm`, `evals/`.
- **Finished when:**
  - Switching provider or model changes only environment configuration.
  - No vendor SDK import exists outside `packages/llm/adapters` (a boundary rule).
  - The harness produces a report against the placeholder dataset.
- **Validation:** unit tests with the fake adapter, an eval CLI run, and the boundary rule in CI.

**T11 · Help Request creation (AI, manual, from previous)**
- **Goal:**
  - Help Request model and state machine (§4.3) for single and group requests (roles, shifts, slots).
  - Structuring endpoint that asks follow-ups only for a missing what, where, or when, or for a safety consideration.
  - Manual creation and copy-from-previous.
  - Vault-stored address.
  - Mobile ask flow: text and on-device speech, a single review screen, and client timing events.
- **Depends on:** T08, T10.
- **Scope:** `modules/requests`, mobile ask flow.
- **Finished when:** a request can be drafted three ways, with AI never publishing it. If structuring is down, the manual flow works.
- **Validation:** API tests, Maestro "ask" flow in he and en, failure-injection test.

**T12 · Screening, emergency redirect, appeals**
- **Goal:**
  - Rules stage, LLM stage, union combination, and the 10 s budget.
  - Fail-closed hold.
  - `ScreeningDecision` persistence with all version fields.
  - Emergency path with the redirect screen (numbers from policy) at both the describe and submit stages.
  - Blocked state with localized explanation; appeal creation; console appeal queue with SLA; overturn publishes.
  - Organization-eligibility check for Organization requesters.
- **Depends on:** T09, T11.
- **Scope:** `modules/screening`, console, mobile.
- **Finished when:** AC-6 passes; AC-7 and AC-8 pass on the English set; latency p95 ≤10 s on staging; Hebrew results reported.
- **Validation:** integration, failure-injection, and eval runs, plus a k6 latency test.

**T13 · Engagement core: interests, terms, confirmation, invariants**
- **Goal:**
  - Interest (raise hand, withdraw neutrally).
  - Canonical `EngagementTerms` and hash.
  - Confirmations and the `confirm_and_create_engagement()` DB function.
  - **A constraint trigger that guards engagement inserts.**
  - Group slot capacity locking.
  - Material-change proposal and re-confirmation.
  - Expense terms (FR-19).
  - Named attendees for Organization Helpers.
  - The `engagement_events` append-only log.
- **Depends on:** T03, T11.
- **Scope:** `modules/engagements`, `packages/db`.
- **Finished when:**
  - AC-1, AC-2, AC-3, AC-16, and AC-23 pass at the API and DB level.
  - One call per party confirms (AC-26, server side).
  - Concurrent confirms never exceed capacity.
- **Validation:** DB invariant tests, fast-check state-machine properties, concurrency tests.

**T14 · Engagement lifecycle after confirmation**
- **Goal:**
  - Start, cancel (with reason and notice period), and no-show report with dispute.
  - Helper outcome report; requester confirm or dispute; unconfirmed after 7 d.
  - Worker-driven expiry.
  - Request `filled` and reopen on cancellation.
  - Mobile engagement screens.
- **Depends on:** T13.
- **Scope:** `modules/engagements`, worker jobs, mobile.
- **Finished when:** every transition in §3.10 is reachable only by the permitted actor and recorded with actor, time, and context.
- **Validation:** state-machine property tests, API tests, Maestro engagement flow.

**T15 · Disclosure service and vault**
- **Goal:**
  - `DisclosureService` with the §3.11 policy, including the handling operator and named attendees, and the 14-day window.
  - `disclosure_events` and the in-app notices on first disclosure.
  - Evidence pre-signed URLs.
  - The `vault_reader` pool, used only by this module.
- **Depends on:** T04, T13.
- **Scope:** `modules/disclosure`.
- **Finished when:** the generated disclosure matrix passes, including denial of non-handling Organization operators (AC-11), and no other module can import a vault accessor.
- **Validation:** matrix tests, boundary rule, a manual API probe on staging.

**T16 · Safety-sensitive flow and evidence**
- **Goal:**
  - Safety labels taken from screening.
  - Per-engagement, per-party attestations bound to the terms and text version.
  - Evidence upload (pre-signed PUT, MIME and size limits, EXIF strip) and the requester's acknowledgement.
  - One shared "user-provided, not verified by Handraise" component.
  - Confirmation blocked until attestations are complete.
- **Depends on:** T12, T13, T15.
- **Scope:** `modules/engagements`, mobile.
- **Finished when:** AC-9 and AC-10 pass, and ordinary requests show no extra steps (AC-26).
- **Validation:** API tests, component tests asserting the label on every surface, Maestro safety flow.

**T17 · Discovery: feed, search, eligibility reasons**
- **Goal:**
  - The ranking SQL function (urgency excluded).
  - Feed and search endpoints with eligibility reason codes.
  - Availability Offer discovery and public profile browsing.
  - Mobile feed with one-tap raise hand.
- **Depends on:** T08, T12, T13.
- **Scope:** `modules/discovery`, mobile.
- **Finished when:** AC-17 and AC-18 pass, and raising a hand is one tap from the feed and from the request page.
- **Validation:** fixture ranking tests, API tests, Maestro.

**T18 · Notifications and matching fan-out**
- **Goal:**
  - The outbox to pg-boss pipeline.
  - Match selection on publish.
  - APNs and FCM senders, plus SMS and email adapters.
  - Opt-in, quiet hours, and daily cap.
  - Notifications on thread activity, state changes, and safety events, and nothing else.
  - Delivery timing recorded.
- **Depends on:** T17.
- **Scope:** `modules/notifications`, worker, mobile `expo-notifications`.
- **Finished when:** the notification p95 is ≤60 s on staging (AC-25), a raise hand from a notification is one tap, and payloads contain no PII.
- **Validation:** k6 load test, integration tests with fake senders, on-device check.

**T19 · Request-scoped messaging and Availability Offers**
- **Goal:**
  - Threads bound to a request, with encrypted bodies.
  - Leave-thread without notifying the other party.
  - Availability Offer CRUD with the "not an obligation" text.
  - Offer inquiry that requires choosing a Help Request.
  - Mobile thread UI with polling.
- **Depends on:** T13, T17.
- **Scope:** `modules/messaging`, `modules/requests`, mobile.
- **Finished when:** no API creates a free-standing thread, and a withdrawal or leave produces no rejection signal (AC-2).
- **Validation:** API tests, Maestro.

**T20 · Feedback and public trust record**
- **Goal:**
  - Double-blind feedback on six dimensions.
  - Private free text.
  - Worker-derived `trust_aggregates`.
  - Public profile trust section: counts by role, cancellations and no-shows with tenure, dimensions at ≥3 ratings, account type, identity or validation status.
- **Depends on:** T14.
- **Scope:** `modules/trust`, mobile profile.
- **Finished when:** AC-14 and AC-15 pass, and feedback is accepted only from parties to a closed engagement.
- **Validation:** API tests, schema-scanner extension, component tests.

**T21 · Reports, moderation, restrictions, pattern detection**
- **Goal:**
  - Report intake with safety routing and SLAs; on-call SMS and email page.
  - Console queues for reports and moderation actions: unpublish, restrict, uphold or overturn, with a visible rationale.
  - Restriction notices and appeals.
  - Pattern detection job with console-editable thresholds and review items.
- **Depends on:** T09, T14.
- **Scope:** `modules/safety`, console.
- **Finished when:** AC-19 SLAs are measured and shown in the console, every action is audited with its rationale, and no automatic blocking comes from patterns.
- **Validation:** API and console tests, a page test on staging.

**T22 · Privacy operations: deletion, export, retention**
- **Goal:**
  - Data-subject export (JSON).
  - Deletion flow: tombstone, crypto-shred, S3 purge, redaction of the user's own request text.
  - Nightly retention jobs from policy.
- **Depends on:** T04, T20.
- **Scope:** `modules/privacy`, worker.
- **Finished when:** AC-13 passes. After deletion the counterparty's history and aggregates are unchanged and the deleted user's PII is unreadable.
- **Validation:** integration tests, runbook review with counsel.

**T23 · Tier 1 verification and automated Organization validation (Should, flagged)**
- **Goal:**
  - `IdentityVerificationProvider` adapter with the vendor SDK via an Expo config plugin, retry path, and human review after 3 failures.
  - `RegistryAdapter` implementation for automated Organization validation.
- **Depends on:** T06, T09; vendor decision (Q3) and data-source check (Q12).
- **Scope:** `modules/identity`, mobile, console.
- **Finished when:** with the flag on, Tier 1 shows the check mark; with the flag off, behaviour is unchanged. Automated validation completes in minutes for registry matches.
- **Validation:** sandbox vendor tests, API tests.

**T24 · Second LLM adapter and AI profile suggestions (Should)**
- **Goal:**
  - An `openai-compatible` adapter that passes the same evals.
  - AI profile suggestions, created `unconfirmed`, with an accept/edit UI.
- **Depends on:** T10, T08.
- **Scope:** `packages/llm`, `modules/profiles`, mobile.
- **Finished when:** the eval report runs for both adapters with no domain code changes, and AC-4 and AC-5 hold.
- **Validation:** eval runs, API tests, a code-search check that no AI path writes public profile fields, publishes, or confirms.

**T25 · Metrics and observability**
- **Goal:**
  - Server analytics events for §8.
  - Client time-to-publish events.
  - SQL metric views and the console metrics page.
  - pino logs with redaction, Sentry, and CloudWatch alarms (5xx rate, screening p95, notification p95, queue lag).
- **Depends on:** T12, T18, T20.
- **Scope:** `modules/analytics`, console, infra alarms.
- **Finished when:** every §8 primary, speed, and safety metric is visible in the console from staging data.
- **Validation:** fixture data compared against the expected metric values.

**T26 · Pilot hardening and launch gates**
- **Goal:**
  - Hebrew eval pass on the final Q6 dataset.
  - Full VoiceOver, TalkBack, and RTL device pass.
  - k6 load test.
  - Security review of auth, vault, and uploads.
  - DR runbook and restore drill.
  - Store submissions.
  - AC-24 usability sessions.
  - Ops runbook with thresholds, retention, and on-call.
- **Depends on:** T12 – T25.
- **Scope:** cross-cutting.
- **Finished when:** every launch gate in §7 is checked off with evidence linked in the runbook.
- **Validation:** gate checklist review with the Pilot Program Lead.
