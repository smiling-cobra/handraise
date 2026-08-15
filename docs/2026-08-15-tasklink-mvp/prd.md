# PRD: TaskLink MVP

## 1. Summary

TaskLink is a trusted labor exchange for businesses that need real-world tasks completed and workers seeking suitable local work. It combines structured task definition, skill and evidence matching, rapid direct offers, searchable marketplace discovery, and a transparent work lifecycle.

The MVP will validate whether one pilot market can produce good matches and completed work while preserving human control. AI may structure task descriptions and suggest matching attributes, but customers approve every published order, workers choose their work, and evidence or licensing decisions cannot be fabricated or waived by AI.

## 2. Problem and Context

### 2.1 Current state

Businesses commonly publish vague job advertisements, manually screen unsuitable applicants, and struggle to assess whether a worker's claimed skills are credible. Workers apply with limited information about expectations, conditions, compensation, or fit and may receive irrelevant opportunities.

Existing marketplaces address only parts of this problem. Ride and delivery platforms match quickly but support narrow work types; digital freelance platforms provide broader discovery but are designed for remote work, slow proposal cycles, and self-managed profiles. General marketplaces also permit unsolicited messaging and often conflate ratings with verified skill evidence.

### 2.2 Why this matters

Customers need faster access to workers who meet practical, geographic, safety, and skill requirements. Workers need transparent tasks, control over which work they accept, and a credible record that distinguishes verified experience from self-declared capability.

Without a structured and trusted exchange, both sides lose time, poor-fit engagements increase, and urgent local tasks remain difficult to fill. The MVP should test demand, matching quality, and completion of the core marketplace loop before expanding verification, payment, or profile intelligence.

## 3. Users and Use Cases

### 3.1 Primary users

- **Customer:** A business representative who defines an on-site or physical task, finds suitable workers, sends or reviews offers, monitors execution, and approves the outcome. Success means hiring an appropriate worker with clear expectations and a traceable result.
- **Worker:** A person offering labor who progressively describes their skills, evidence, availability, location, tools, languages, and preferences; discovers suitable work; and completes accepted tasks. Success means finding relevant work without spam, hidden conditions, or penalties for declining.

### 3.2 Secondary users

- **Pilot operator:** Reviews verification or safety exceptions, handles reports, manages marketplace taxonomies and matching configuration, and monitors pilot quality. Success means resolving exceptional cases without silently changing user decisions or evidence.

### 3.3 Core use cases

- A new user selects exactly one role and completes role-specific onboarding in stages.
- A customer describes a task in natural language, reviews an AI-proposed structure, resolves blocking gaps, and publishes it.
- A worker searches or browses relevant orders and applies to a feed order.
- A customer searches workers by skill, evidence level, rating, location, and availability, then creates an order before contact begins.
- A customer sends an expiring instant offer for an eligible, clearly specified task; the first valid acceptance reserves the worker's capacity.
- A worker publishes a standing service offer that a customer can discover and use as the starting point for an order.
- Both parties follow an explicit accept, start, complete, and approve lifecycle, with traceable decline, cancellation, failure, and expiry outcomes.

## 4. Goals and Non-Goals

### 4.1 Goals

- Enable a complete customer-to-worker marketplace loop in one pilot market.
- Improve task clarity by converting natural-language requests into customer-approved structured orders.
- Support both feed-based hiring and time-sensitive instant offers.
- Let customers and workers actively search the marketplace, in addition to receiving recommendations.
- Represent skill evidence separately from ratings and support evidence levels L0-L2.
- Make order, application, and offer outcomes explicit, timestamped, and understandable to both parties.
- Keep declines neutral while distinguishing them from post-commitment cancellations, no-shows, and failed execution.
- Prevent unsolicited direct messaging before an order-based relationship exists.

### 4.2 Non-goals

- In-app payments, escrow, invoicing, payment disputes, or guarantees that an external payment will occur.
- Certificate, license, or specialist verification represented by evidence levels L3-L5.
- Automatic evolution of worker profiles from outcomes or a worker-facing inference correction center.
- Multi-country launch, cross-jurisdiction compliance, or broad localization.
- AI selection of a worker, autonomous publication, autonomous acceptance, or autonomous approval of work.
- Replacement of employers' or workers' legal, tax, insurance, or workplace-safety obligations.

## 5. Product Requirements

### 5.1 Functional requirements

#### Accounts and onboarding

- A new account must choose either Customer or Worker. Role changes are not self-service in v1 and require operator handling.
- Customer onboarding must capture identity and business details, billing readiness, saved work locations, typical task categories, and common worksite conditions. Payment details are not processed in v1.
- Worker onboarding must be conversational and resumable. It must cover identity, service location, travel radius, availability, skills, CV or work history, portfolio, certificates, tools, languages, environments, instruction preferences, compensation preferences, and excluded work.
- Users must be able to reach the usable marketplace after completing only the fields required for safe and meaningful participation; incomplete optional sections must remain available later.
- Public professional information and private matching preferences must be visibly separated. Customers must never see excluded tasks, compensation thresholds, decline reasons, or other private matching inputs.

#### Order creation and publication

- A customer must be able to describe a task in natural language and provide location, schedule or time window, duration or effort, compensation terms, and worksite details.
- AI must propose a standardized title, category, required and optional skills, minimum proficiency, risk class, physical requirements, safety flags, and missing information.
- Each AI-proposed field must be reviewable and editable. The customer must explicitly approve the structured order before publication.
- Publication must be blocked when required safety, location, scheduling, compensation, or task-scope information is missing, or when a mandatory credential requirement is unresolved.
- AI must not invent worker evidence, remove mandatory credential requirements, publish an order, hire a worker, or approve completed work.
- A published feed order must remain editable only through a change flow that records material changes and notifies affected applicants. Material changes to scope, location, schedule, compensation, risk, or required skills require reconfirmation from an accepted worker.

#### Discovery, search, and matching

- Workers must receive a ranked feed of published orders based on required skills, evidence level, location and travel radius, availability, and private preferences.
- Workers must be able to search and filter orders by keyword, category, location or distance, schedule, compensation, and required skill.
- Customers must be able to search and filter public worker profiles by skill, evidence level, category-specific rating, location or service radius, and availability.
- Search results and recommendations must not expose private worker matching data.
- A customer may view a worker profile before creating an order but cannot start direct messaging until an order or order invitation connects the parties.
- The product must notify workers about relevant new orders according to their notification preferences and record whether a notification was delivered.
- Matching must exclude workers who fail mandatory credentials, availability, location, or explicit safety requirements. Ranking must not convert a missing requirement into a soft preference.

#### Feed applications and hiring

- A worker may apply to or ignore a feed order. Ignoring or declining before commitment must not reduce reliability metrics.
- An application must show the worker the current order terms and show the customer the worker's relevant public profile, evidence, rating, and availability.
- A customer must explicitly select a worker from applicants. Selection creates the accepted engagement and closes or pauses further hiring for that position.
- Unselected applications must receive an explicit declined or closed outcome when hiring ends.
- If an order supports multiple positions, the customer must declare the number before publication and each position must have an independently traceable engagement.

#### Instant offers

- Instant offers may be created only for tasks that pass completeness and risk eligibility rules configured for the pilot.
- A customer may send an instant offer only to workers who satisfy mandatory matching requirements and appear available for the requested period.
- The offer must display the full structured order, compensation terms, location, schedule, expiry time, and required credentials before acceptance.
- Each offer must expire automatically after the pilot-configured response window. Expired offers cannot be accepted.
- Acceptance must atomically reserve the worker's relevant capacity and prevent conflicting acceptance. If capacity is no longer available, the user must see a clear failure and the offer must remain unaccepted.
- Declining or ignoring an instant offer is neutral. An accepted instant offer follows the same engagement lifecycle and cancellation rules as a feed-based hire.

#### Standing service offers

- A worker must be able to publish, pause, edit, and withdraw a standing service offer containing a service title, description, category, skills and evidence, service area, availability, compensation basis, and relevant conditions.
- Customers must be able to find standing service offers through search and feed discovery.
- A standing offer is not itself an accepted engagement. A customer must create or confirm a structured order from it, and both parties must agree before capacity is reserved.
- Publishing a standing offer must not expose private matching preferences or personal location beyond the worker's chosen public service area.

#### Engagement lifecycle and communication

- The primary engagement states must be: `SUBMITTED`, `PENDING`, `ACCEPTED`, `IN_PROGRESS`, `COMPLETED`, `APPROVED`, `DECLINED`, `REJECTED`, `WITHDRAWN`, and `EXPIRED`.
- `SUBMITTED` means an order or response awaits required review; `PENDING` means it is live and awaiting the other party's decision; `ACCEPTED` means both parties are committed; `IN_PROGRESS` begins when the worker selects Start; `COMPLETED` awaits customer review; `APPROVED` is successful and final.
- `DECLINED` represents a party's explicit refusal; `REJECTED` represents customer rejection of submitted work; `WITHDRAWN` represents cancellation by the initiating or committed party; and `EXPIRED` represents timeout without the required decision.
- Every state transition must identify the actor, timestamp, reason where required, and preceding state. Both parties must see the current state and relevant history.
- The system must allow either party to withdraw after acceptance, require a reason, and classify timing separately from pre-commitment declines.
- A worker must be able to mark execution as completed, postponed, or failed. Postponement requires a proposed schedule and the customer's agreement; failure closes execution with a reason and does not appear as approved work.
- A customer reviewing completed work must approve it or reject it with a reason. Rejection must preserve the order history and permit the pilot's operator-assisted resolution process; it must not silently return the engagement to an earlier state.
- A no-show must be recorded separately from a decline, withdrawal, rejection, or failed task.
- Messaging becomes available only after a customer creates an order or invitation that connects the parties. Messages must remain associated with that order or engagement.

#### Trust, evidence, and ratings

- Worker skills must display an evidence level independently from ratings: `L0 Self-declared`, `L1 Supported`, or `L2 Platform-demonstrated`.
- L1 requires worker-provided supporting material or structured work history and must be labeled as not independently verified. AI may extract or suggest skills from material, but the worker must confirm them.
- L2 requires qualifying completed TaskLink work in the same skill or category under pilot-configured rules. AI inference alone cannot grant L1 or L2.
- Customers must be able to see category-specific ratings, completed-task counts, evidence labels, and disclosed safety or mismatch indicators relevant to hiring.
- Ratings and feedback may be created only after a real engagement reaches an eligible terminal outcome. Evidence level must not be calculated from star ratings alone.
- Workers must be able to report inaccurate public profile data and request operator correction during v1.

### 5.2 Content, data, or API requirements

- The product must maintain distinct records for an order, feed application, instant offer, standing service offer, and accepted engagement so their states are not conflated.
- Structured orders must include customer, category, description, required and optional skills, proficiency, risk and safety fields, location, schedule, capacity, compensation terms, worksite conditions, and publication metadata.
- Worker profiles must maintain a public professional layer and a private matching layer with separate access permissions.
- Evidence records must retain skill, level, source type, source reference, date, and review status. User-facing labels must state what each level does and does not prove.
- Reliability events must distinguish neutral pre-commitment declines from withdrawals, late cancellations, no-shows, failed tasks, and customer rejections.
- Compensation must be shown in a consistent amount or range, currency, and basis such as hourly or fixed. The product must state that payment occurs outside TaskLink in v1.
- Search and matching outputs must use the latest published order version and current worker availability.
- Notifications must cover publication matches, application or offer decisions, offer expiry, material order changes, upcoming accepted work, lifecycle actions, and customer review requests.
- User-visible dates, distances, currencies, and legal content must follow the configured pilot locale.

### 5.3 Quality, safety, and failure requirements

- Mandatory credential, risk, and safety constraints must be hard eligibility rules, not ranking suggestions.
- The system must present AI output as a proposal, identify unresolved fields, and preserve the customer's final approved version.
- Users must receive clear recovery guidance when AI structuring fails, notifications cannot be delivered, an offer expires, capacity conflicts, publication is blocked, or a lifecycle action is no longer valid.
- Concurrent applications or offer acceptances must not create more accepted engagements than the order's declared capacity.
- Material actions must be auditable, including publication approval, order edits, evidence changes, hiring, acceptance, start, completion, approval, rejection, withdrawal, and operator intervention.
- Role, profile-layer, messaging, and order access permissions must prevent one user from viewing or changing another user's private or unrelated information.
- Precise worker home location and private preferences must not appear in public profiles, search results, standing offers, or customer-facing explanations.
- Accounts and user-generated content must support reporting and operator review. Safety reports must not automatically become public accusations without review.
- The experience must support keyboard navigation, readable labels and errors, and core workflows on current mobile and desktop browsers.
- If a required service is unavailable, the system must fail closed for publication, credential eligibility, acceptance, and approval rather than guessing or losing an action.

## 6. Scope

### 6.1 In scope for v1

- Role-based account creation and progressive customer and worker onboarding.
- Public worker profiles and private matching preferences.
- AI-assisted, customer-approved structured order creation.
- Worker order feed, order search, customer worker search, recommendations, and notifications.
- Feed applications and customer selection.
- Expiring instant offers with eligibility checks and capacity reservation.
- Worker standing service offers and customer discovery.
- Order-linked messaging.
- Accept, start, complete, approve, reject, postpone, fail, withdraw, decline, no-show, and expire behaviors.
- Evidence levels L0-L2, category ratings, outcome metrics, and basic reporting/operator review.
- One configured pilot market and locale.

### 6.2 Out of scope for v1

- Platform payment processing, escrow, refunds, or payment enforcement.
- L3-L5 credential and specialist verification.
- Automated profile inference from outcomes and a dedicated inference explanation/correction experience.
- Multiple roles per account or self-service role switching.
- Multi-country operations and jurisdiction-specific employment classification workflows.
- Automated dispute adjudication or guaranteed resolution outcomes.

### 6.3 Future considerations

- Verified certificates, licenses, issuing-body integrations, and specialist verification.
- Dynamic worker profile updates with transparent inference history and self-service correction.
- In-app payments, escrow, invoicing, and payment disputes.
- Additional countries, languages, currencies, legal regimes, and labor models.
- More sophisticated fairness evaluation, recommendation explanations, and matching controls.
- Repeat bookings, teams, staffing agencies, subscriptions, and enterprise customer administration.

## 7. Acceptance Criteria

- A new customer and a new worker can select a role, complete minimum onboarding in stages, leave, and resume without losing entered information.
- A customer can submit an unstructured task, review and edit every AI-proposed field, resolve blocking gaps, explicitly approve it, and publish it.
- No order is published automatically by AI or with unresolved mandatory safety or credential requirements.
- A worker can receive relevant notifications, browse and search orders, inspect full terms, apply or decline, and see an explicit application outcome.
- A customer can search public worker profiles by skill, L0-L2 evidence, category rating, location, and availability without seeing private preferences or precise home location.
- A customer can select an applicant and create exactly the declared number of accepted engagements; other applicants receive an explicit outcome.
- An eligible worker can accept a valid instant offer before expiry and have capacity reserved; expired, ineligible, or conflicting acceptance fails without creating an engagement.
- A worker can publish, pause, edit, and withdraw a standing service offer, and a customer can discover it and create an order from it.
- Neither party can initiate direct messaging solely from profile browsing; order-linked messaging becomes available after an order or invitation connects them.
- Both parties can observe the accepted engagement move through `ACCEPTED`, `IN_PROGRESS`, `COMPLETED`, and `APPROVED`, with actor and timestamp history.
- Declines, withdrawals, late cancellations, no-shows, failures, rejections, and expiries remain distinguishable in user-visible history and reliability data.
- A pre-commitment decline or ignored opportunity does not reduce a worker's reliability signal.
- Skill evidence displays L0-L2 labels and sources separately from category ratings; AI suggestion alone never raises an evidence level.
- Compensation terms are visible before application or acceptance, and the product clearly states that payment is handled outside TaskLink.
- Core customer and worker journeys are usable on supported mobile and desktop browsers and meet the pilot's agreed accessibility review.

## 8. Metrics and Signals

- **Marketplace activation:** percentage of onboarded customers publishing an order; percentage of onboarded workers completing a profile and viewing relevant work.
- **Liquidity:** percentage of published orders receiving at least one eligible application or accepted instant offer; median time to first eligible response and accepted engagement.
- **Matching quality:** application-to-selection rate, instant-offer acceptance rate, worker-reported mismatch rate, and customer rejection rate.
- **Completion:** percentage of accepted engagements started, completed, and approved; time from acceptance to start and from completion to approval.
- **Clarity and safety:** AI drafts edited before publication, publication blocks by reason, material-change reconfirmations, safety reports, credential eligibility blocks, and operator interventions.
- **Reliability fairness:** neutral declines versus withdrawals, late cancellations, and no-shows; complaints that neutral declines affected visibility or reliability.
- **Trust:** use of evidence filters, conversion by evidence level, rating completion, and corrections requested for public worker data.
- **Qualitative signals:** customer confidence in worker fit, worker confidence in task clarity, perceived control over decisions, and reasons either side leaves the marketplace.

Pilot target values must be agreed before launch after estimating expected marketplace volume and task mix.

## 9. Alternatives Considered

- **Feed-only MVP:** Simpler, but rejected for v1 because urgent direct offers and proactive worker services are core to the intended marketplace proposition.
- **Instant matching only:** Rejected because complex, higher-risk, or less standardized work needs browsing, applications, and customer selection.
- **Open direct messaging:** Rejected to reduce spam, harassment, and off-platform leakage; communication is gated by an order relationship.
- **Ratings as the trust model:** Rejected because satisfaction does not prove a skill, credential, or license. Evidence and ratings remain separate.
- **AI-autonomous publishing or hiring:** Rejected because task accountability, worker choice, and evidence integrity require explicit human decisions.
- **Full L0-L5 verification in v1:** Deferred because certificate, licensing authority, and specialist review workflows would expand operational and regulatory scope before the core loop is validated.
- **In-app payments in v1:** Deferred to focus the pilot on matching and execution while compensation terms remain transparent.

## 10. Open Questions and Assumptions

### 10.1 Policies to resolve before implementation

- **Pilot market:** Product and legal owners must choose the pilot locality, supported language, currency, distance units, required legal notices, and prohibited task categories.
- **Marketplace limits:** Product must set maximum active orders per customer, applications per worker and order, standing offers per worker, instant-offer recipients, instant-offer response time, and positions per order.
- **Instant-offer rules:** Product and operations must define eligible task categories and risk classes and decide whether offers are sent sequentially or simultaneously to a limited cohort.
- **Evidence thresholds:** Product and operations must define acceptable supporting material for L1 and the number and type of approved TaskLink outcomes required for L2.
- **Identity verification:** Product, legal, and operations must define required customer identity and business checks and required worker identity checks.
- **Cancellation and no-show policy:** Product and operations must define the threshold for a late cancellation, the criteria for a no-show, and which reliability events are public, private, or subject to operator review.
- **Rejected-work handling:** Product and operations must define the operator-assisted resolution process, whether disputed ratings remain visible, and how rejection affects reliability. Monetary disputes remain out of scope.

### 10.2 Policies to resolve before pilot launch

- **Pilot success thresholds:** Product must set minimum marketplace activity and fill rates; acceptable matching, completion, rejection, and safety rates; and customer and worker satisfaction targets for the pilot go/no-go review.
- **External availability:** Product must confirm that TaskLink reserves capacity only within TaskLink and decide whether external calendar integration is necessary for launch.

### 10.3 Product assumptions

- **Position capacity:** This PRD assumes an order may declare one or more identical positions; materially different roles require separate orders.
- **Availability reservation:** This PRD assumes TaskLink reserves capacity only within TaskLink and cannot prevent conflicts with external calendars unless an integration is later added.

## 11. AI Implementation Readiness Notes

Use this PRD as the product contract for a later technical plan or SRS. The next artifact should define the domain model, state machines for orders/applications/offers/engagements, permissions, matching and eligibility rules, AI structuring contract, notification behavior, concurrency handling, audit events, and test strategy. It should resolve or explicitly configure the open policy thresholds rather than embedding unexplained constants.

Suggested implementation-planning prompt:

> Create an SRS and phased implementation plan for the TaskLink MVP from this PRD. Preserve the separation between orders, applications, instant offers, standing service offers, and engagements; model all lifecycle transitions and permissions explicitly; identify policy decisions that block implementation; and propose vertical slices that validate onboarding, structured publication, feed hiring, instant offers, standing offers, execution, and trust without introducing in-app payments or L3-L5 verification.
