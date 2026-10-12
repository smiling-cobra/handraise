# PRD: Handraise MVP — Mobile Help Coordination Network

**Status:** Draft for review
**Date:** 2026-10-12
**Source material:** `handraise-concept-brief.md`
**Pilot:** One city in Israel, 3-month closed pilot

---

## 1. Summary

Handraise is a mobile help coordination network that connects people who need a helping hand with people nearby who are willing to give one. Its mission is to make both sides fast: **quickly find a helping hand, or quickly give one.** Asking for help should feel as easy as ordering food or a taxi: say what you need, see who can help, agree, and it's on.

Anyone can join, either as an **individual** or as an **organization**. Every account must declare its type at sign-up. Either kind of account can ask for help or offer it. The help itself is always given by people, hand to hand. Help is unpaid and freely given. Receiving help creates no debt and no obligation to give anything back.

The MVP tests one question: **can Handraise get a person from "I need a hand" to a confirmed helper quickly, simply, and safely in a single pilot city?** It does this through four things:

- **Fast asking.** The Requester describes the need in plain words, by text or voice. AI turns it into a clear Help Request the Requester reviews and publishes in about a minute.
- **Fast offering.** Nearby people see the requests that fit them and can raise a hand with one tap.
- **Clear agreement.** Help is on once both people confirm. Until then, either one can walk away at no cost.
- **Enough trust to say yes.** Both sides can see identity status and a factual history of past help before deciding.

The taxi comparison is about ease, not dispatch. Handraise does not assign volunteers, broker labor, or decide who must help whom. It gives people the structure to find one another, understand a situation, assess trust and safety, and make their own decision, with as little friction as possible. Nobody is "on shift" and nobody is obligated. Handraise is a place where people help people, not a gig marketplace or a volunteer-operations platform.

---

## 2. Problem and Context

### 2.1 Current state

When someone needs small, concrete help, such as fixing a leaking tap, getting a ride to a clinic, carrying a sofa up three floors, or sorting donations for an hour, there is no quick way to ask for it. Ordering food or a taxi takes a minute. Finding a helping hand can take days, or may never happen. Today people rely on personal networks, neighborhood WhatsApp and Facebook groups, or ad-hoc volunteer coordinators. These channels fail in specific, repeatable ways:

- **Asking is slow and awkward.** You have to know which group to post in, write a message from scratch, and wait for a reply that may never come. Many people never ask at all, especially elderly people and those new to a city.
- **Offering is just as hard.** People who would gladly help have no way to say "I'm free this afternoon and I can fix things." Their willingness is invisible, so help is available but nobody can find it.
- **Needs are unstructured.** A message saying "someone help my mother" carries no scope, timing, location, tools, physical conditions, or risk information. Helpers cannot assess fit without a private negotiation, so most scroll past.
- **Trust is one-directional or absent.** A stranger entering a home is evaluated on a profile photo and a group's implicit vouching. Helpers, symmetrically, have no way to know whether a Requester's previous requests were accurate, respectful, or safe.
- **Commitment is ambiguous.** "I can maybe come Thursday" is not a commitment, but it is often treated as one. No-shows are invisible to everyone except the person who waited.
- **Safety is unmanaged.** Physical risk, unsafe working conditions, and exploitation (unpaid help quietly substituting for paid staff) are handled by group-admin judgment or not at all.

Organizations face the same problem at a larger scale. Shelters, community projects, and local initiatives coordinate helpers through spreadsheets and group chats. They have no fast way to reach new helpers and no reliable record of who confirmed what.

### 2.2 Why this matters

Everyday apps have taught people to expect that what they need can be arranged from a phone in minutes. Help between people hasn't caught up. Israel has a strong volunteering culture and dense civil-society infrastructure, but the way help gets coordinated is informal and only reaches as far as one coordinator's or one group's personal network. The cost of doing nothing is not a dramatic failure. It is a steady loss nobody sees: needs go unmet because the people who could help never saw them, and willing helpers never find a use for their time.

For the pilot, the thing to prove is that a need can reach a confirmed helper **quickly**, and that confirmed help **actually happens** at meaningful rates. Everything else in the roadmap depends on it. If safety and confirmation steps add so much friction that asking through Handraise is slower than posting in a WhatsApp group, the model is wrong, and we should know within three months.

---

## 3. Users and Use Cases

### 3.1 Primary users

Every account is either an **Individual** or an **Organization**, declared at sign-up. Separately from account type, the role in each interaction depends on context: whoever asks for help is the **Requester**, and whoever gives it is the **Helper**.

- **Individual as Requester:** a person with a concrete, bounded need they cannot meet alone, such as an elderly resident, someone with limited mobility, or a parent without a car. **Need:** ask for help in about a minute, in ordinary words, and have the request become something a stranger can quickly and safely evaluate. **Success:** a suitable helper confirms quickly, arrives, and the help happens as described.
- **Individual as Helper:** a person with time, a skill, or a tool who wants to help. **Need:** see nearby requests that genuinely fit their capabilities, availability, and comfort, raise a hand in one tap, and never feel pressured. **Success:** they find a request they can actually do, understand exactly what is expected before committing, and find the situation matches the description on arrival.
- **Organization:** a registered nonprofit (עמותה) or public benefit company (חל"צ) with its own account, operated by people from the organization (details in FR-2). **Need:** publish requests for helpers quickly, including roles, shifts, and capacity, reach helpers beyond its own network, and know who has actually confirmed. An organization may also offer help to others. **Success:** requests fill with helpers who show up, and each helper has an individual record.

Roles attach to interactions, not accounts. One person can be a Requester on Tuesday and a Helper on Thursday with the same profile. The same applies to organizations.

### 3.2 Secondary users

- **Handraise Moderator/Operator:** reviews appeals against automated screening decisions, handles user reports, and reviews escalated exploitation-pattern items. **Success:** every appeal and report is resolved within SLA with a recorded rationale.
- **Pilot Program Lead:** needs visibility into time to match, matching quality, completion rates, and safety incidents to decide whether the model works.

### 3.3 Core use cases

1. **Quick everyday help.** A Requester needs two people to carry a sofa up three floors this evening. They describe it in one sentence by voice. AI drafts the request, and the Requester reviews and publishes it in about a minute. Nearby Helpers who opted in are notified, two of them raise a hand, and the Requester confirms both.
2. **Home repair for an elderly person.** A Requester describes a leaking tap and their limited mobility. AI structures the scope, tools, area, timing, and a cap on materials expenses. A nearby Helper with declared plumbing experience expresses interest, and both agree on terms and confirm.
3. **Safety-sensitive home task.** A Requester needs help replacing a broken outdoor light fixture using a ladder and power tools. The request is flagged safety-sensitive; the Helper may upload relevant supporting evidence (labelled user-provided and unverified), the Requester evaluates it, and both explicitly attest before confirmation.
4. **Animal shelter shift coverage.** A validated shelter's Organization account publishes a request for six Helpers across two shifts, with physical conditions and clothing requirements. Each Helper confirms and completes an individually recorded Engagement.
5. **Community event support.** A mission-led event requests helpers for setup, visitor guidance, accessibility support, and cleanup. Screening confirms the roles serve the mission rather than replacing routine paid staff.
6. **Standing availability.** A person publishes an Availability Offer for minor repairs and event setup on weekends within a 10 km radius. Requesters discover it and open a conversation tied to a specific request. The person may decline every inquiry with no penalty.
7. **Organization offering help.** A community organization publishes an Availability Offer: its volunteers can help elderly residents with grocery runs on Friday mornings. Individual Requesters discover it and ask through a specific Help Request.
8. **Declining without cost.** A Helper reads a request, decides it is not for them, and moves on. Nothing is recorded and nothing is signalled to the Requester.

---

## 4. Goals and Non-Goals

### 4.1 Goals

- **G1. Fast asking.** A Requester can go from describing a need in ordinary words to a published Help Request in about a minute. The request must be clear enough for a stranger to evaluate without a preliminary conversation, and the Requester controls what gets published.
- **G2. Fast offering.** A Helper can find nearby requests that fit them and raise a hand in one tap. Willing helpers can make themselves discoverable through Availability Offers.
- **G3. Clear, lightweight commitment.** An Engagement exists only when both parties explicitly confirm, and confirming must be a quick act, not a negotiation. Declining before confirmation costs nothing and is invisible to others.
- **G4. Trust on both sides.** Both parties get a reciprocal, factual basis for trust: engagement history and structured ratings, never a single opaque score. This applies equally to Individual and Organization accounts.
- **G5. Purpose-bound privacy.** Precise address, phone number, and sensitive details of a need are disclosed only when a specific engagement requires them.
- **G6. Safety without friction for legitimate requests.** Prevent the categories of harm the concept names (prohibited activity, unacknowledged risk, and unpaid help substituting for paid staff) without treating users as suspects or slowing down legitimate requests.
- **G7. Individuals and organizations as first-class accounts.** Both account types can ask for and offer help. Group requests (roles, shifts, capacity) are supported, and one person's cancellation never affects anyone else's record.

### 4.2 Non-goals

- **Not** a payments, barter, time-bank, or reciprocal-credit system. No money moves through Handraise in the MVP.
- **Not** an emergency or crisis response channel. Emergency intent is detected and redirected to national services; it is never routed to volunteers.
- **Not** a credentialing authority. Handraise verifies identity, not professional competence. Professional medical assistance is out of scope for the MVP, and any uploaded supporting evidence is labelled user-provided and unverified.
- **Not** a volunteer program management system of record. Handraise does not compete in v1 on kiosk check-in, volunteer-hours export, CRM sync, waiver administration, or institutional reporting.
- **Not** a general social network. There are no unsolicited direct messages, follows, or feeds unrelated to a request or offer.
- **Not** a childcare or vulnerable-adult care platform. Direct care involving children and vulnerable adults is prohibited in the MVP.
- **Not** multi-city. One pilot city; geographic expansion is out of scope.
- **No** web client in the MVP. Native iOS and Android only.
- **Not** a dispatch service. Handraise never assigns a helper to a request, and no automated matching commits anyone. Recommendations are suggestions, and every commitment is a human act.
- **Not** a guaranteed-response service. Handraise makes help fast to ask for and fast to find, but it cannot promise that someone will say yes, and it never applies pressure to make that happen.
- **Not** a gig or labor marketplace. Help is unpaid and freely given, by individuals and organizations alike.

---

## 5. Product Requirements

### 5.1 Functional requirements

**FR-1 · Sign-up, account type, and tiered verification**

- At sign-up, every account must declare its type: **Individual** or **Organization**. The choice is required and cannot be skipped. Changing the type later is not self-service and goes through support, because an account's trust history belongs to the type it was earned under.
- One Individual account per person. Requester and Helper are contextual roles in an interaction, not account types.
- Sign-up must be quick. An Individual can create a Tier 0 account and publish a first request or raise a hand without completing any optional step first.
- The verification tiers below apply to Individual accounts. Organization accounts follow FR-2.
- **Tier 0 (Registered / identity unverified):** verified email and phone number. May create a profile, publish a Help Request or Availability Offer, browse, express interest, message within a request thread, and confirm an Engagement. Public surfaces must display a clear descriptive label such as **Identity unverified**.
- **Tier 1 (Verified):** government ID document plus liveness check via a third-party identity provider. Not required for individual participation, but intended as a stronger trust signal if a viable Israeli identity vendor is available at acceptable cost and legal posture for the pilot. Public surfaces must display Tier 1 accounts with a clear verification check mark whenever Tier 1 is active.
- Identity verification is **private**. Only the status label is public; the underlying identity data is never shown to other users. The person controls which parts of their identity (display name, photo, city, age band) are public.
- Where identity status is shown, the distinction between verified and unverified accounts must be legible at a glance and must not imply that Handraise has verified anything beyond identity.
- Failed or inconclusive verification produces a clear reason and a retry path; three consecutive failures route to human review rather than a hard lock.

**FR-2 · Organization accounts and validation**

- An Organization account is a full account with its own profile, requests, offers, and trust record. It is created by a named person who becomes its **account owner**. The owner must complete the strongest identity-verification tier available in the pilot and remains accountable for activity on the account.
- The owner may add other named **operators**, such as staff or coordinators. Operators sign in with their own Individual accounts and act on behalf of the Organization, so there is no shared organization password. Every action is recorded against the operator who took it. In the MVP all operators have the same permissions, with no role hierarchy.
- Eligible organization types in the MVP: registered nonprofit associations (עמותה) and public benefit companies (חל"צ), evidenced by active status in the national registry.
- A **purpose-and-beneficiary test** is applied per organization at validation and per request at screening. Legal form alone does not confer eligibility, and an eligible organization may still publish an ineligible request.
- An Organization must be validated before its **first** Help Request or Availability Offer is published. Before validation it can set up its profile and prepare drafts. Validation against registry data should be automated where possible, so it completes within minutes. When manual review is needed, it completes within **2 business days**.
- When an Organization acts as a **Helper**, it must name the person or people who will attend before confirmation. Their display names and identity status are shown to the Requester, who confirms knowing who will arrive.
- A current Certificate of Proper Management (אישור ניהול תקין), when supplied, is displayed as an additional trust badge. It is not required for validation.

**FR-3 · Progressive profile and capability claims**

- Individual profile fields: chosen public identity; interests and causes; self-declared capabilities, experience, tools, languages; availability; travel radius; uploaded supporting documents; user-controlled context about personal needs.
- Organization profile fields: name and logo; mission and areas of activity; registry number and validation status; service area; the kinds of help it asks for or offers.
- A profile only needs the minimum to start. Everything else can be added later, and the app never blocks a first request or a first raised hand on profile completeness.
- Every claim displays its **source and verification status**. User-uploaded evidence is never labelled "verified".
- **Private matching preferences** (excluded activities, personal constraints, comfort limits) improve recommendations and are never exposed to other users.
- AI may _suggest_ profile additions from conversation or repeated activity. The person must confirm or edit a suggestion before it becomes public. AI never edits a public profile directly.
- Factual engagement history is stored and displayed **separately** from self-declared or AI-inferred capability claims. The two must never be visually merged.

**FR-4 · AI-assisted Help Request creation**

- **Speed target:** a typical request can be described, reviewed, and published in about **one minute**, and within about **two minutes** for elderly Requesters (AC-24).
- The Requester describes a need in ordinary language (text or voice-to-text, Hebrew or English).
- Only three things are required to publish: **what** is needed, **where** (area), and **when**. All other fields are optional unless they apply to the request: desired outcome, effort estimate, tools, expenses, relevant capabilities, physical conditions, and roles and capacity for group requests.
- AI proposes a structured Help Request from the description. It asks follow-up questions only for missing required information or a detected safety consideration, never for optional detail.
- The Requester **reviews, edits, and approves** the request on a single review screen before publication. AI never publishes.
- A request may also be created manually without AI assistance, or started from one of the Requester's previous requests.

**FR-5 · Automated screening, blocking, and appeal**

> **Amendment to the concept brief.** The brief states that AI never publishes or rejects a request and assumes a human pre-review queue for flagged requests. The MVP amends this: without a standing moderator queue, legitimate requests would remain invisible. The screener therefore has **two outcomes only** — publish, or block with a stated reason and a human appeal path. Autonomy is preserved through appeal rather than through pre-review.

- On submission, every Help Request is screened against the prohibited-category taxonomy and anti-exploitation heuristics.
- **Outcome A — Published.** The request goes live, with safety-sensitive labelling and required attestations applied where relevant.
- **Outcome B — Blocked.** The request remains a private draft. The Requester receives the specific category matched and a plain-language explanation, then may edit and resubmit or **appeal to a human**.
- There is no "pending review" state. Nothing waits on a human to become visible.
- Screening must feel instant: a decision is returned within **10 seconds** for 95% of submissions.
- Appeals are resolved by a human within **2 business days**, with a recorded rationale. Emergency-intent detections are never appealable; they redirect to national emergency services (Police 100, MDA 101, Fire 102, ERAN 1201).

**FR-6 · Availability Offers**

- An Individual or a validated Organization publishes what kinds of help they are open to, travel area, availability windows, preferences, and relevant capabilities.
- An Availability Offer is explicitly **an invitation to connect, not a standing obligation**. This must be stated in the UI at publication and on the offer itself.
- Requesters discover offers and open a conversation **in the context of a specific Help Request**. There is no free-form messaging from an offer.

**FR-7 · Discovery and matching**

- Fit-ranked feed, keyword and filter search, and opt-in notifications for new matching requests.
- Opt-in notifications for a new matching request are delivered within **1 minute** of publication.
- **Raising a hand** is a single tap from the feed, the request page, or the notification.
- Ranking inputs: declared interests and capabilities, availability overlap, distance, private preferences, and safety requirements.
- **Urgency is displayed as context and never used to boost ranking.** No countdown timers, scarcity language, or pressure mechanics anywhere in the product.
- Requesters can browse Availability Offers and public profiles.
- Requests the user is ineligible for (unmet safety attestation, outside travel radius) are shown with the reason rather than silently hidden.

**FR-8 · Request-scoped communication**

- Unsolicited direct messaging does not exist. Every conversation thread is bound to a Help Request.
- All pre-confirmation communication happens in-app in the request thread.
- Either party may leave a thread at any time before confirmation, with no record and no notification framing it as a rejection.

**FR-9 · Raising a hand and mutual confirmation**

- Raising a hand (expressing interest) is explicitly **not a commitment**, stated in the UI at the point of action.
- The Requester sees everyone who raised a hand, with their trust record, and can chat with any of them in the request thread (FR-8) if something needs clarifying.
- **Confirmation is lightweight.** The Requester picks a Helper and confirms. The Helper confirms the same terms with one tap. Safety-sensitive requests also require attestation (FR-15). No other step is required.
- The terms both parties confirm are limited to **what, when, and where**, plus only the items that apply to the request: expense cap (FR-19), role and shift (FR-12), safety attestation (FR-15), and named attendees when the Helper is an Organization (FR-2). Anything else, such as tools, access, or how to communicate a change, can be agreed in the thread but is never a required step.
- An Engagement is created **only** when both parties have explicitly confirmed the same agreed terms. A single-sided confirmation creates nothing.
- A **material change** to scope, schedule, safety conditions, or expense terms after confirmation requires renewed consent from both parties. Until both re-confirm, the previously agreed terms stand and the change is not in effect.

**FR-10 · Staged disclosure**

- Before confirmation: display name, public profile, general location area (neighbourhood-level, not an address).
- On mutual confirmation: precise address and phone number are disclosed to the counterparty only, with an in-app notice to both parties that this has happened. When the counterparty is an Organization, they are disclosed only to the operator handling the engagement and the named attendees, not to every operator.
- On engagement closure: contact details remain visible to the counterparty for **14 days**, then are hidden from the engagement view. The factual record persists.

**FR-11 · Engagement lifecycle**

States: `interest_expressed` → `confirmed` → `in_progress` → `completed` | `partially_completed` | `not_completed`. Terminal alternatives: `withdrawn` (pre-confirmation), `cancelled` (post-confirmation, by either party, with reason and timestamp), `no_show` (counterparty-reported, subject to dispute), and `expired` (scheduled time passed with no start).

- Every transition records **who acted, when, and relevant context**. History is append-only; no state transition may be deleted or silently rewritten.
- Pre-confirmation withdrawal is **neutral** — not counted, not displayed, and not framed as a rejection.
- Post-confirmation cancellation and no-show are recorded and count toward the public factual record, with the notice period captured.
- The Helper reports the outcome; the Requester confirms or disputes it. An unresolved dispute is recorded as disputed rather than resolved in either party's favor.

**FR-12 · Group requests**

- Any account, Individual or Organization, may create a group request. A Help Request may define multiple **roles**, multiple **shifts**, and a **capacity** per role/shift.
- Each Helper confirms an **individual Engagement** against a specific role and shift, with an independently recorded outcome.
- One person's cancellation or no-show must not affect any other participant's record.
- The Requester sees confirmed-versus-capacity per role and shift, and is notified when a confirmed Helper cancels.

**FR-13 · Structured mutual feedback**

- After closure, both parties may leave structured feedback on fixed dimensions: **clarity, respect, communication, reliability, safety, outcome.**
- Free-text comments are collected but are **private to the counterparty and moderators**. They are never public.
- Feedback is **double-blind**: neither party sees the other's submission until both have submitted or a 7-day window closes.
- Feedback is optional. Non-submission carries no penalty and is not displayed.

**FR-14 · Public trust record**

Publicly visible on a profile:

- Completed engagement count, as Requester and as Helper, separately.
- Post-confirmation cancellation count and no-show count, with tenure context (e.g. "2 cancellations across 31 engagements").
- Per-dimension aggregate ratings, shown only once a **minimum of 3 ratings** on that dimension exists.
- Account type (Individual or Organization), always shown.
- For Individuals: identity-status label (verification check mark for Tier 1; descriptive unverified label for Tier 0), and any organizations they operate for.
- For Organizations: validation status, and the Certificate of Proper Management badge when supplied.

Never public: free-text feedback, private preferences, identity documents, precise location, sensitive need details, uploaded supporting evidence documents (visible only to a counterparty within a specific engagement context).

- No single composite "score" is computed or displayed anywhere.

**FR-15 · Safety-sensitive requests and bilateral attestation**

- A defined subset of permitted requests carries a **safety-sensitive** label. In v1 this list is intentionally narrow and explicit: work at height, electrical or power-tool work, animal handling, and other categories explicitly listed in the product taxonomy for the pilot.
- Safety-sensitive requests display the specific risk prominently and require **both parties to attest** — explicitly, per engagement, not once per account — before confirmation is possible. Each party attests to the specific risk and to understanding the conditions under which the task will be performed.
- Where a request involves elevated risk or specialized capability claims, the Helper may upload relevant supporting evidence. The evidence is shown to the Requester **labelled as user-provided and unverified by Handraise**. The Requester evaluates it and explicitly acknowledges the capability claim before confirming.
- Attestation text is versioned; the record captures which version each party attested to.

**FR-16 · Prohibited categories**

Blocked in the MVP: illegal, deceptive, or deliberately harmful activity; emergency or crisis response; sexual or intimate services; direct care involving children or vulnerable adults; requests violating another person's rights or consent; routine commercial labor presented as volunteering.

**Policy taxonomy for v1**

- **Prohibited-category examples:** emergency medical transport needed now; asking someone to enter another person's property without consent; unpaid routine staffing for a commercial business; intimate or sexual services; direct care for a child or vulnerable adult; illegal purchase, delivery, or deception.
- **Safety-sensitive examples:** changing a high light fixture using a ladder; using power tools for home repair; handling animals in a shelter or rescue setting; lifting or moving heavy items in unsafe physical conditions. These requests may be permitted only with the safety-sensitive label and bilateral attestation.
- **Organization-eligibility pass examples:** a shelter requesting enclosure cleaning support; a community institution requesting donation sorting; a mission-led event requesting accessibility support.
- **Organization-eligibility fail examples:** a commercial festival using volunteers for routine ticketing or cleanup; a nonprofit publishing ordinary unpaid backfill for ongoing staff roles; an organization whose request benefits a private commercial operation more than a public or community purpose.

**FR-17 · Reporting and moderation**

- Any user can report a request, profile, message, or engagement, with a category and free text.
- Reports involving physical safety or an in-progress engagement are routed for human review within **4 hours**; all other reports within **2 business days**.
- Moderators can unpublish a request, restrict an account, or uphold/overturn a screening decision — each with a recorded rationale visible to the affected user.
- Restricted users are told what was restricted and why, and have an appeal path.

**FR-18 · Exploitation-pattern detection**

- Track cumulative engagement count and scheduled duration from agreed `EngagementTerms` per Requester–Helper pair and per organization over rolling windows.
- Above configured thresholds, raise an internal review item for an operator and, where appropriate, prompt the parties to review whether the arrangement remains freely chosen and within scope.
- No automatic blocking, and no hard engagement-count or duration cap. Thresholds are configuration, tunable during the pilot, with initial values set before launch and recorded in the operations runbook.
- Suspected routine staff substitution by an organization is escalated for human review.

**FR-19 · Expense reimbursement**

- Expense **categories**, responsibility, and a maximum amount or calculation method are recorded as part of agreed Engagement terms, and must be agreed before confirmation.
- **No money moves through Handraise.** Settlement is directly between the parties. The platform records the agreed cap only.
- Changing the expense terms is a material change and requires renewed bilateral consent (FR-9).

**FR-20 · Localization**

- Hebrew (full RTL layout) and English, user-selectable, independent of device locale.
- AI request-structuring, screening, and emergency-intent detection must perform to the same standard in both languages. Screening quality in Hebrew is a launch gate, not a follow-up.

### 5.2 Content, data, and integration requirements

**Core entities:** `Account` (type: Individual or Organization), `Profile`, `CapabilityClaim`, `Organization`, `OrganizationOperator`, `NamedAttendee`, `HelpRequest`, `RequestRole`, `Shift`, `AvailabilityOffer`, `Interest`, `Engagement`, `EngagementTerms`, `Attestation`, `EvidenceDocument`, `MessageThread`, `Feedback`, `Report`, `ScreeningDecision`, `Appeal`, `ModerationAction`, `PatternReviewItem`.

**Help Request states:** `draft` → `screening` → `published` | `blocked`; `published` → `filled` | `closed` | `expired` | `withdrawn`. A blocked request may be edited and resubmitted, or appealed.

**Required product-level data properties:**

- Engagement history and all state transitions are **append-only and immutable**. Corrections are new records, never edits.
- `EngagementTerms` are versioned; each version records both parties' confirmation timestamps.
- `ScreeningDecision` records the model/ruleset version, matched category, and confidence, so pilot screening quality can be audited.
- Verification data, identity documents, and evidence documents are stored separately from profile data with distinct access controls and retention.
- Public trust aggregates are derived, never manually editable.

**Integrations:**

- Third-party identity verification provider (ID document + liveness), if Tier 1 verification is active in the pilot.
- National nonprofit registry data source for organization validation.
- Push notification services (APNs, FCM); SMS and email for verification and critical fallbacks.
- Mapping/geocoding for area-level location and distance, with **address-level precision never sent to third parties before confirmation**.
- LLM provider for request structuring, screening, and suggestions.

**Jurisdiction as configuration.** Verification provider, organization registry source, emergency service numbers, prohibited-category taxonomy, retention periods, and pattern thresholds are policy configuration, not hardcoded logic. The pilot ships one configured jurisdiction (Israel); the model must not assume it is the only one.

### 5.3 Quality, safety, and failure requirements

- **Speed:** the core flows have explicit targets measured in the pilot: publishing a typical request takes about one minute of the user's time (about two minutes for elderly Requesters); screening returns within 10 seconds (p95); matching notifications arrive within 1 minute; raising a hand takes one tap; confirmation takes one tap per party. Safety steps such as attestation and evidence appear only on requests that need them and never add steps to ordinary requests.
- **Privacy:** purpose-bound disclosure is enforced server-side, not merely hidden in the UI. Private preferences are never returned by any API a counterparty can reach. Retention periods are defined per data class before launch, with identity documents held for the minimum period the verification flow requires.
- **Data subject rights:** access, correction, and deletion requests are supported. The exact deletion model remains a pre-SRS design decision because it must remove personal data without allowing one user to erase the counterparty's factual history from an append-only record.
- **AI boundaries — hard constraints:** AI must never publish a request, alter a public profile, create or confirm an Engagement, or leave feedback. Every AI output that affects another user passes through explicit human confirmation.
- **AI failure behavior:** if the structuring model is unavailable, manual request creation remains fully functional. If the **screening** model is unavailable, publication is **held**, not permitted — the user is told screening is temporarily unavailable and their draft is saved. Failing open on screening is not acceptable.
- **Emergency redirection** is a distinct, always-on path that takes precedence over every other flow, including screening appeals.
- **Auditability:** every moderation action, screening decision, appeal outcome, attestation, and disclosure event is logged with actor, timestamp, and rationale.
- **Explainability:** a blocked request states which category was matched and why, in plain language. A moderation restriction states its reason. "Violates our policies" alone is not acceptable.
- **Abuse resistance:** rate limits on request creation, interest expression, and reporting. Feedback may only be left by a party to a closed Engagement.
- **Accessibility:** the primary Requester population includes elderly and recovering users. WCAG 2.2 AA equivalent on mobile, full dynamic type support, VoiceOver/TalkBack on all core flows, and no core action that depends on precise gestures.
- **Notification restraint:** no engagement-farming notifications. Notifications are limited to matches the user opted into, direct thread activity, engagement state changes, and safety/moderation events.

---

## 6. Scope

### 6.1 In scope for v1

1. Quick sign-up with a required choice of account type, Individual or Organization, and contextual Requester/Helper roles (FR-1).
2. Tier 0 identity verification, plus Tier 1 verification if vendor feasibility, cost, and legal posture support it for the pilot (FR-1).
3. Organization accounts for עמותות and חל"צ: a verified owner, named operators with equal permissions, registry validation, and named attendees when the Organization helps (FR-2).
4. Progressive profiles for both account types, with capability claims, privacy controls, and user-confirmed AI suggestions (FR-3).
5. AI-assisted Help Request creation with Requester approval, targeting about one minute to publish (FR-4).
6. Automated screening within seconds, with block-and-appeal and no pending-review queue (FR-5).
7. Availability Offers from Individuals and Organizations (FR-6).
8. Fit-based feed, search, prompt matching notifications, one-tap raise a hand, request-scoped communication (FR-7, FR-8).
9. Lightweight bilateral confirmation with material-change re-consent (FR-9).
10. Staged disclosure of address and phone (FR-10).
11. Compact engagement lifecycle with append-only history and neutral pre-commitment refusal (FR-11).
12. Group requests with roles, shifts, capacity, and individual engagement tracking, for both account types (FR-12).
13. Structured double-blind mutual feedback; aggregate-only public trust record (FR-13, FR-14).
14. Narrow, explicit safety-sensitive labelling, evidence upload, bilateral attestation, prohibited categories, reporting, moderation, appeals (FR-15 – FR-17).
15. Exploitation-pattern observation and operator escalation, with optional user-facing prompts where policy calls for them (FR-18).
16. Expense terms recorded, no money movement (FR-19).
17. Hebrew (RTL) and English (FR-20).
18. Native iOS and Android apps.
19. Internal operator console for appeals, reports, organization validation, and pattern review items.

### 6.2 Out of scope for v1

- Volunteer program management as a primary product category: no kiosk check-in, volunteer-hours export, CRM sync, waiver administration, or institutional reporting.
- Web client of any kind.
- Payments, barter, time credits, reciprocal obligations, paid engagement types.
- Professional medical assistance and workflows that depend on platform-validated medical competence, licenses, or certificates.
- Platform-verified professional credentials.
- Childcare, vulnerable-adult care, and safeguarding workflows (DBS-equivalent checks).
- Emergency and crisis response.
- Multi-city or multi-jurisdiction operation.
- Organization role hierarchies and per-operator permissions. In v1 every operator of an Organization account has the same permissions.
- Organization types other than עמותות and חל"צ, such as businesses, municipalities, or informal community groups.
- Dispatch or auto-assignment of any kind, and guaranteed response times for finding a helper.
- Public API, third-party integrations, volunteer-hours export for institutional reporting.
- Explanations of _why_ a specific recommendation was made.
- Free-text public reviews.
- In-app scheduling/calendar sync.

### 6.3 Future considerations

Additional exchange types as explicitly separate engagement models (paid, reciprocal); platform-verified credentials; expanded safeguarding enabling care categories; richer organization administration (roles, permissions, more organization types); transparent matching explanations; multi-city expansion using the jurisdiction configuration layer; recurring and standing engagements. **None of these should be pre-built or partially scaffolded in v1** — the concept's value depends on the MVP model staying small and legible.

---

## 7. Acceptance Criteria

**Autonomy and consent**

- AC-1. An Engagement can only come into existence after two distinct explicit confirmations of the same terms version. No code path creates one otherwise, and individual participation in that flow does not require Tier 1 identity verification.
- AC-2. A user who browses, expresses interest, and withdraws before confirmation leaves no record visible to any other user, and generates no notification framing the withdrawal as a rejection.
- AC-3. Changing scope, schedule, safety conditions, or expense terms on a confirmed Engagement leaves the prior terms in force until both parties re-confirm.

**AI boundaries**

- AC-4. No AI action publishes a Help Request, modifies a public profile field, or creates/confirms an Engagement — verified by code review and an audit of production logs.
- AC-5. Every AI-suggested capability appears as unconfirmed until the user explicitly accepts or edits it.
- AC-6. With the screening service unavailable, publication is blocked with a clear message, and no request reaches the feed unscreened.

**Safety**

- AC-7. Each prohibited category is blocked at publication, in both Hebrew and English, with a stated reason and an appeal path, verified against a labelled test set at ≥95% recall on the prohibited set.
- AC-8. Emergency intent is detected and redirected to national emergency services rather than converted into a Help Request, in both languages.
- AC-9. A safety-sensitive Engagement cannot be confirmed until both parties have attested, and the attestation record identifies the version of the text each party accepted.
- AC-10. Uploaded supporting evidence is displayed with an unambiguous "provided by the user, not verified by Handraise" label on every surface it appears.

**Privacy**

- AC-11. Precise address and phone number are unobtainable, through UI or API, by anyone who is not a confirmed counterparty on that Engagement. When the counterparty is an Organization, only the operator handling the engagement and the named attendees can obtain them.
- AC-12. Private matching preferences are never returned by any endpoint reachable by another user.
- AC-13. Before build begins, the product and technical plan define and test a deletion model that removes personal data without allowing one user to erase the counterparty's factual history.

**Trust record**

- AC-14. No composite trust score exists anywhere in the product.
- AC-15. Public profiles show account type and completed, cancelled, and no-show counts with tenure context; dimension aggregates appear only at ≥3 ratings; free text is never public. Individual profiles always show identity status, either as a Tier 1 verification check mark when Tier 1 is active or a clear unverified label otherwise. Organization profiles always show validation status.
- AC-16. In a group request, one Helper's cancellation changes no other Helper's record.

**Discovery**

- AC-17. Two feeds identical except for urgency produce identical ordering — urgency is display-only.
- AC-18. A request the user is ineligible for shows the reason rather than silently disappearing.

**Operations**

- AC-19. Appeals resolve within 2 business days; safety reports within 4 hours — measured, not asserted.
- AC-20. All core flows are usable with VoiceOver and TalkBack, and in RTL, verified on device.

**Accounts and organizations**

- AC-21. Sign-up cannot be completed without choosing Individual or Organization, and the account type cannot be changed without going through support.
- AC-22. An Organization cannot publish a Help Request or Availability Offer until it is validated. Every action on an Organization account is attributed to a named operator in the audit log.
- AC-23. An Engagement in which an Organization is the Helper cannot be confirmed until at least one attendee is named, and the Requester sees each attendee's display name and identity status before confirming.

**Speed**

- AC-24. In usability testing with pilot-representative users, the median time from starting a new request to publishing it is ≤60 seconds, and ≥80% of participants publish within 2 minutes. For elderly Requesters, tested as a separate group, the median is ≤2 minutes.
- AC-25. Measured in production: screening returns a decision within 10 seconds for ≥95% of submissions, and opt-in matching notifications are delivered within 1 minute of publication for ≥95% of requests.
- AC-26. Raising a hand takes exactly one tap. Confirming a non-safety-sensitive request takes one tap per party. Nothing beyond what, where, and when, plus the items that apply under FR-9, is required to publish or confirm.

---

## 8. Metrics and Signals

**Primary (does the model work?)**

- **Time to a confirmed helper:** median time from publication to the first mutual confirmation, by category and by account type. This is the headline metric for "quickly find a helping hand."
- **Completed engagements:** ≥100 over the 3-month pilot.
- **Confirmation rate:** raised hand → mutual confirmation. A low rate means confirmation is still too heavy, or Requesters don't trust who is raising a hand.
- **Completion rate:** confirmed → completed. Target ≥85%.

**Speed**

- **Time to publish:** time from starting a new request to publishing it, as measured in the app (target median ≤60 seconds overall and ≤2 minutes for elderly Requesters, AC-24; the elderly split uses the optional age band where users provide it). Also the drop-off rate between starting a request and publishing it.
- **Time to first raised hand:** median time from publication to the first raised hand.
- **Unanswered requests:** share of requests with no raised hand within **24 hours**. **A rate above ~30% means matching or liquidity is failing.**
- **Sign-up completion:** share of people who start sign-up and finish it, split by account type.
- Technical targets from AC-25: screening latency and notification delivery time.

**Adoption**

- Total registered and active accounts, split by **Individual** and **Organization**. If Tier 1 is active in the pilot, also Tier 1 verified users and the Tier 1 verification rate among active users.
- Share of accounts that act in **both** roles, which tests the core hypothesis that roles belong to interactions rather than accounts.
- Published requests and Availability Offers, split by account type.
- Organizations: time from sign-up to validation, number of active Organizations, and how many both ask for and offer help.

**Quality**

- Request structuring: share of AI-proposed requests published with no field edited (too high may mean rubber-stamping; too low means the structuring is not useful).
- Feedback submission rate and per-dimension distributions, especially **clarity** (did the request as described match reality), which is the strongest signal that fast structuring hasn't cost accuracy.
- Post-confirmation cancellation and no-show rates, split by account type.
- Group requests: share of capacity filled by the scheduled time.

**Safety**

- Screening precision and recall against a labelled Hebrew/English set, tracked per ruleset version.
- Appeal volume and overturn rate (**a high overturn rate means the screener is over-blocking and suppressing legitimate needs**).
- Safety reports per 100 engagements; time to resolution.
- Any incident of physical harm, exploitation, or privacy breach — target zero, each one individually reviewed regardless of rate.
- Exploitation pattern items raised, and how many were judged genuine on review.

**Qualitative**

- Structured interviews with ≥20 Requesters, ≥20 Helpers, and the operators of at least 5 Organizations, focused on: did asking or offering help feel as easy as ordering food or a taxi; did the situation match the description; did anyone feel pressured to accept; did the trust record actually inform the decision.

---

## 9. Alternatives Considered

- **A single reputation score.** Rejected: reduces a person to one opaque number, invites gaming, and hides _why_ a rating is what it is. Structured dimensions with visible counts preserve the information a decision actually needs.
- **Public free-text reviews.** Rejected: invites retaliation and harassment, and free text about a health or home situation is a serious privacy hazard given who the Requesters are. Free text is collected but kept private to the counterparty and moderators.
- **Human pre-review queue for flagged requests** (the concept brief's original design). Rejected for the MVP: with automated-screening-only staffing, a pending queue would leave legitimate requests invisible for days. Replaced by block-with-appeal (FR-5), which preserves human recourse without a blocking dependency.
- **Automatic hour/frequency caps to prevent exploitation.** Rejected per the concept: repetition alone does not indicate coercion, and a cap would penalize the healthiest ongoing relationships. Replaced by observation and escalation, with user prompts only where policy explicitly calls for them.
- **In-app payments for expense reimbursement.** Rejected: introduces money-transmission obligations and dispute adjudication, and blurs the unpaid-volunteering line the product is built on. Recording an agreed cap achieves the transparency goal at a fraction of the cost.
- **Responsive web MVP.** Rejected: notification reliability, location context, and identity/liveness capture are materially better native, and the product's credibility depends on all three.
- **Urgency-weighted ranking.** Rejected: it converts a coordination tool into a pressure mechanism and directly contradicts voluntary participation. Urgency is context only.
- **Deferring Availability Offers to v2.** Considered — it is the least proven half of the marketplace. Kept, because without it the pilot cannot test whether willing helpers become discoverable, which is half the stated problem.
- **Individuals-only pilot (no organizations).** Rejected: shelters, institutions, and events are where group volunteering, capacity, and staff-substitution risk actually appear, and those are among the riskiest things to leave untested.
- **Build on or partner with an existing volunteer management platform.** Rejected: POINT-class products are built around organization-owned events with unilateral registration. They cannot express an individual's negotiated need, bilateral confirmation, staged disclosure, or reciprocal trust without changing the product primitive.

---

## 10. Open Questions and Assumptions

**Open questions**

| #   | Question                                                                                                                                                                       | Owner / resolution path                                     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| Q1  | Which city is the pilot in, which partner organization is the acquisition channel, and what month-one request/helper volume will it seed?                                      | Pilot Program Lead — before build starts                    |
| Q2  | Who pays for the pilot, and what is the acceptable per-user identity-verification cost ceiling?                                                                                | Pilot Program Lead — before FR-1 build                      |
| Q3  | Which identity verification vendor supports Israeli ID documents with liveness at acceptable cost, data residency, and coverage?                                               | Vendor evaluation — before FR-1 build                       |
| Q4  | Legal review of volunteer-liability exposure and whether insurance is required for safety-sensitive engagements.                                                               | External counsel — before pilot launch                      |
| Q5  | Legal review of Israeli Privacy Protection Law obligations: lawful basis, retention, DPO requirement, security classification for identity and sensitive personal-need data.   | External counsel — before pilot launch                      |
| Q6  | Exact prohibited/safety-sensitive taxonomy for the MVP, excluding professional medical assistance, as an enumerated, testable list with Hebrew and English examples.           | Product + Trust & Safety — before FR-5 build                |
| Q7  | What kinds of user-provided supporting evidence are acceptable for safety-sensitive tasks in the MVP, and how are they described to the Requester without implying validation? | Product + counsel                                           |
| Q8  | What retention period applies to each data class, and how will deletion requests be reconciled with append-only engagement history?                                            | Counsel + Product — before SRS                              |
| Q9  | Initial threshold values for exploitation-pattern observation and escalation.                                                                                                  | Pilot Program Lead — before launch, recorded in ops runbook |
| Q10 | Who staffs appeals and reports during the pilot, and what is the escalation path outside business hours for safety reports?                                                    | Pilot Program Lead                                          |
| Q11 | Can LLM screening meet the 10-second p95 latency target (AC-25) at the AC-7 quality bar in Hebrew, and what is the fallback if it can't?                                      | Engineering — before FR-5 build                             |
| Q12 | Can the registry data source support automated Organization validation within minutes, or will most validations need manual review?                                           | Engineering + Pilot Program Lead — before FR-2 build        |
| Q13 | What is the support process and policy for changing an account's type (Individual ↔ Organization), and what happens to its trust history?                                      | Product — before launch                                     |

**Resolved decisions**

| #   | Decision                                                                                                                                                                       | Where reflected                     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------- |
| D1  | Every account must declare at sign-up whether it is an Individual or an Organization.                                                                                          | §1, FR-1, AC-21                     |
| D2  | Organization accounts can both ask for and offer help. When an Organization helps, it names the attendees before confirmation.                                                | §3, FR-2, FR-6, AC-23               |
| D3  | v1 Organizations are limited to registered עמותות and חל"צ. Informal community groups join as Individuals for now.                                                             | FR-2, §6.2                          |
| D4  | Speed targets: about 60 seconds median to publish (about 2 minutes for elderly Requesters), screening ≤10 seconds p95, matching notifications ≤1 minute.                       | FR-4, FR-5, FR-7, §5.3, AC-24, AC-25 |
| D5  | A request with no raised hand within **24 hours** counts as unanswered.                                                                                                        | §8                                  |

**Assumptions (to be corrected if wrong)**

- **A1.** Active registry status is the bar for organization validation; אישור ניהול תקין is a strengthening badge, not a requirement. _Assumed, not confirmed._
- **A2.** Organization operators sign in with their own Individual accounts and act on behalf of the Organization account. There is no shared organization password, and every action is attributed to a named operator. _Assumed._
- **A3.** A third-party verification vendor with adequate Israeli ID coverage exists at viable pilot cost; if not, Tier 1 verification is deferred and the pilot launches with the strongest feasible alternative identity signal.
- **A4.** LLM screening quality in Hebrew reaches the AC-7 bar; if not, Hebrew screening needs a rules-based backstop and the timeline changes.
- **A5.** Lightweight confirmation (what, when, and where, plus only the items that apply) gives both parties enough clarity to show up and do the right thing. If clarity ratings or completion rates are low, the required terms in FR-9 need to grow, and speed is traded for accuracy.
- **A6.** Pilot liquidity is achievable only if a launch partner and community outreach seed enough early requests and helpers. This is unproven and should be measured explicitly. Organizations that offer help are expected to be an important early source of helpers.
- **A7.** Requesters will accept the extra step of seeing named attendees before confirming an Organization as Helper, and Organizations will be able to name attendees in advance.

---

## 11. AI Implementation Readiness Notes

**Use this PRD for:** scoping the MVP, deciding what ships in v1, writing the technical plan, and settling scope disputes. Sections 5 and 7 are the contract — functional requirements and acceptance criteria are written to be individually testable.

**Do not use this PRD for:** database schema, API design, service boundaries, mobile architecture, model/prompt selection, or test plans. Entity names in §5.2 are product vocabulary, not a schema.

**Belongs in the technical plan (`srs.md`):** account model for Individual and Organization accounts, including operators acting on behalf of an Organization and named attendees; data model and access-control implementation of purpose-bound disclosure; append-only engagement history and terms versioning; screening pipeline design, ruleset versioning, latency budget, and the evaluation harness for AC-7/AC-8/AC-25; registry integration and Organization validation flow; matching, ranking, and near-real-time notification delivery; identity vendor integration and PII segregation; RTL and localization approach; operator console; observability for the §8 metrics, including the speed metrics.

**Highest-risk areas — plan these first:**

1. Server-side enforcement of staged disclosure and purpose-bound privacy (AC-11, AC-12). A leak here does reputational damage that can't be undone. Organization accounts make this harder, because disclosure must reach only the handling operator and named attendees, not every operator.
2. Bilateral confirmation and terms versioning (AC-1, AC-3, AC-26). This is the conceptual core: getting the state machine wrong invalidates the pilot. It must stay at one tap per party without weakening the guarantee that both parties confirmed the same terms version.
3. Screening quality **and speed** in Hebrew (AC-7, AC-8, AC-25). This is a launch gate with a real chance of failing, and the 10-second budget may conflict with the quality bar (Q11).
4. The account model (AC-21 – AC-23). Individual vs. Organization accounts, operators acting on an Organization's behalf, and attributing every action to a named person all shape identity, permissions, and audit throughout the system. Retrofitting this later is expensive.
5. Liability-sensitive flows for safety-sensitive engagements. The product is brokering help between strangers, not just scheduling events.
6. Append-only history with deletion rights (AC-13). This remains a genuine pre-SRS design conflict.

**Suggested first implementation-planning prompt:**

> Using `docs/2026-09-07-handraise-mvp/prd.md`, produce a technical plan for the Handraise MVP. Start with the account model: Individual and Organization accounts, how operators act on behalf of an Organization, and how every action is attributed to a named person. Then define the domain model and state machines for `HelpRequest`, `Engagement`, and `EngagementTerms`, showing every transition, the actor permitted to trigger it, and how bilateral confirmation and material-change re-consent are enforced server-side while keeping confirmation to one tap per party. Then design the purpose-bound disclosure model: what each viewer role can read at each engagement state, including Organization operators and named attendees, enforced at the data-access layer rather than the UI. Include the policy hooks required for the prohibited/safety-sensitive taxonomy and organization-eligibility checks, and a latency budget showing how screening (≤10 s p95) and matching notifications (≤1 min) meet their targets. Call out where AC-13 (deletion) conflicts with the append-only history requirement and propose a resolution. Do not design the mobile UI yet.

---

**Reviewer notes:**

- **Screening amendment.** §5, FR-5 records a deliberate amendment to the concept brief's "AI never rejects a request" principle. If that principle is non-negotiable, FR-5, AC-7, and the moderation staffing assumption all need to be reopened together.
- **Speed as the mission.** This revision reframes the product around speed: quickly find a helping hand, or quickly give one. Confirmation is now lightweight (FR-9) instead of a full clarification checklist, and the speed targets are acceptance criteria (AC-24 – AC-26). If pilot clarity ratings or completion rates fall, FR-9, A5, and AC-26 need to be reopened together. If screening cannot meet its latency target (Q11), FR-5 and AC-25 need to be reopened together.
- **Account types.** Every account now declares at sign-up whether it is an Individual or an Organization (D1). Organizations are full accounts that can ask for and offer help, operated by named operators (FR-2, A2). This replaces the earlier model in which organization representatives acted only through personal accounts. Changes to it affect FR-1, FR-2, FR-10, FR-14, AC-21 – AC-23, and the disclosure model together.
