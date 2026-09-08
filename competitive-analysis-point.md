# Competitive Analysis: POINT (pointapp.org) vs. Handraise

**Date:** 2026-09-07
**Prepared against:** `docs/2026-09-07-handraise-mvp/prd.md`, `handraise-concept-brief.md`
**Sources:** pointapp.org (marketing, pricing, feature comparison, privacy policy, support KB), Apple App Store, Google Play, G2/Capterra/Software Advice, Crunchbase (blocked), press coverage (Forbes, TechOhio, The Metropreneur)

> **Confidence note.** Everything below about POINT is derived from public marketing, its privacy policy, its support knowledge base, and third-party reviews. I did not create an account or see the admin dashboard. Where POINT's own numbers contradict each other, I say so rather than picking one.

---

# Part 1 — POINT, in depth

## 1.1 What it actually is

POINT sells itself as an **"All-in-one Volunteer Management Platform and App"** with the tagline *"Easiest Volunteer App, Everrr."* Stripped of marketing, it is:

- a **B2B SaaS volunteer-operations system of record** for nonprofits (the product that earns money), plus
- a **free consumer mobile app** that acts as the fulfilment and discovery surface for those nonprofits' events (the product that makes the SaaS worth buying).

The centre of gravity is unambiguous. The positioning line *"Donor CRMs are not built for volunteering, we are"* tells you who they think they are displacing: Bloomerang/Kindful/Bonterra-style donor CRMs being misused for volunteer scheduling, and legacy volunteer tools (VolunteerHub, Volunteer Scheduler Pro, Galaxy Digital).

**The unit of work is an Event with Shifts, owned by an Organization.** Everything else in the product is downstream of that primitive.

## 1.2 Origin, entity structure, and business model

| Item | Detail |
|---|---|
| Founder/CEO | Madison Mikhail Bush |
| Origin | Started 2012 as a nonprofit giving website (OneLife, ~$20k raised); pivoted to a volunteer app ~2013; added a nonprofit management dashboard ~2015. Some press dates the app launch to 2016 — POINT's own About page uses 2012 as the founding date. |
| Home market | Columbus, Ohio; early expansion to Cincinnati (press cited ~88,500 users across those two cities in 2019) |
| Structure | Dual entity — **POINT Global** (nonprofit, runs giving programs) and **POINT Tech** (social enterprise, est. 2020, builds product) |
| Model | **"Buy + Give"** — *"When your company buys a subscription to POINT, we gift the nonprofit POINT platform to registered 501(c)3 nonprofits for free"* |
| Stated values | *"Creating tech equity"* and *"Doing more good"*; describes itself as **"impact-first"** |
| Funding | Sparse public record. $36,900 from the Sea Change Columbus 2018 pitch competition is the only round I could confirm; Crunchbase blocked scraping. No evidence of a priced venture round. |

The cross-subsidy is the strategic core: **corporates and multi-org Networks pay so that nonprofits don't have to.** That decision sets the market price of nonprofit-side volunteer-ops tooling at approximately zero, which matters enormously for Handraise (see §2.6).

## 1.3 The four customer surfaces

POINT is not two-sided; it is **four-sided**, and three of the four sides are institutions.

1. **Nonprofits** — the free/Pro/Teams SaaS. 501(c)(3) status is the gate for free access.
2. **Volunteers** — the free consumer app (iOS/Android/web portal). Not monetized.
3. **Companies** — employee volunteering / CSR. Targets *"five to 50,000 employees."* Private employee-only events or co-hosted public events with nonprofits, corporate branding, impact goals, automatic reporting on volunteering **plus donations and in-kind gifts**. Pricing is demo-gated.
4. **Networks** — the enterprise wedge. *"national corporations, city governments, volunteer centers, and large nonprofits."* A network admin invites member orgs (*"corporate branches, separate companies, nonprofit chapters, community organizations, or schools"*), creates events auto-recommended to members, curates charity partnerships, and sets network visibility to **closed / open / secret**. Members get *"network-wide and organization overview stats on volunteer hours, economic impact, causes supported, volunteer trends."* Data-sharing level is a condition of joining.

Networks is the smartest structural piece in the product: a single United Way, city government, or foundation deal drags dozens of orgs onto the platform at once, and those orgs bring their volunteers.

## 1.4 Feature inventory

POINT organizes marketing around six pillars — **Recruit, Onboard, Manage, Mobilize, Track, Engage.** The actual feature surface, from the plan-comparison page:

**Recruit / distribution**
Website widget for opportunities and events (grid, list, calendar views); a hosted "POINT Page"/Volunteer Hub; volunteer mobile app (iOS + Android); volunteer web platform; social sharing (Facebook, X, LinkedIn); email invites; shareable event links.

**Onboard / compliance**
Custom applications; e-waivers; **Checkr background-check integration**; background-check status tracking for both admins and volunteers; document library; custom questions and fields; progress tracking.

**Manage — database**
Unlimited volunteers; dynamic activity profiles (bio, volunteer history, **attendance rating**, hour tracking, background-check status); contact import; search filters; private admin notes; attached documents and submissions; public/private volunteer access levels; unlimited groups and subgroups; account merge; document storage.

**Manage — admins**
Unlimited admins; owner/admin permissions; admin email preferences. Role-based access control is a **Teams**-tier feature.

**Manage — opportunities**
Unlimited events; ongoing events; public, private, and **secret "unlisted"** events; co-hosted events; group-only access; program events; **shifts**; registration management and self-registration; assign-spot and reserve-spot; **group reservations**; shift printouts; registration and cancellation deadlines; event templates and copy-event; volunteer and admin list + calendar views; integrated background-check and waiver requirements per event; drafts; event contact info; **youth and guest registrations** (added in v10.6).

**Manage — programs** (Pro/Network)
Unlimited programs and sub-programs; per-program admin permissions, reporting, events, website integration, and calendar views.

**Track / report**
Automatic reports; granular hour reporting; PDF export; **volunteer hour self-submission and admin verification**; volunteer self-export/print of their own hours; admin-added hours; **check-in stations — kiosk on an org iPad/computer, or a custom QR code scanned from the volunteer's phone** (Pro).

**Engage / messaging**
Email, SMS, and push; auto confirmations, reminders, and thank-yous; sent box and team sent box; volunteer email preference settings; calendar integrations (Apple, Google, Yahoo, iCal).

**Integrations**
SSO (Okta, Apple, Google); donor CRM (Kindful); fundraising (PayPal, Venmo, Givebutter, DonorBox, Donately); calendar; website embeds. **No public API is advertised.**

## 1.5 The volunteer-side experience

The consumer flow is deliberately thin: *"Pick the causes you care about, find opportunities to volunteer in your feed, and sign up by tapping 'go'."* You set a location and causes, get a personalized feed of upcoming events in your city, filter by available times, and register. Coverage is claimed *"across all 50 states — whether you're looking to sign up for a one-time event or apply for an ongoing role."* If nothing is nearby, you can **request that a nonprofit join** — a demand-signal capture mechanic worth stealing.

Two details reveal the real design intent:

- *"Volunteers associated with your organization will see a personalized feed that keeps your nonprofit's events pinned at the top,"* and private events are *"highlighted at the top of the volunteer's feed."* **Discovery is biased toward orgs you already belong to.** This is a retention tool sold to admins, not a neutral marketplace ranking.
- The app consolidates admin burden — *"Register, apply, sign waivers, track hours … the whole shebang on POINT."*

Impact tracking shows **hours and dollars** accumulating on the volunteer profile. POINT claims a **97% attendance rate** across its events.

**App-store reality check.** iOS: version 10.6.0, updated August 9 2026, **4.0 stars from 88 ratings**, 4+ age rating, developer POINT Global. The v10.6 release notes cite smoother event sign-up, registration questions, signing up with kids or guests, redesigned check-in, and **full Ukrainian language support**. Reviews are split: one calls it *"the absolute MOST convenient way to do more good in your city"*; another calls the experience *"hostile to the user"* over an inability to browse multiple opportunities at once. Other requests: privacy transparency, calendar integration, multi-cause selection.

**88 ratings against a claimed 7,000+ nonprofits is the single most informative number in this analysis.** The consumer app is a fulfilment surface for org-driven signups, not a destination people seek out. POINT's growth is org-led, top-down, and the volunteer side is essentially a compliance-and-scheduling client.

## 1.6 Trust and safety model

POINT's safety posture is **delegation, not adjudication.** It ships compliance *tooling*; the nonprofit makes every judgment.

- **Background checks via Checkr**, in three packages: *Basic Plus* (low-risk roles), *Essential* (most programs), *Complete* (schools and vulnerable populations). POINT shares only the volunteer's **email address** with Checkr to send an invitation; it **does not receive or store report content**, only status. Reports live in Checkr and are visible only to the requesting nonprofit and the volunteer.
- Its own KB is explicit that checks are one input: orgs *"may also use applications, references, interviews, training, certifications, waivers, role qualifications, or other requirements."*
- **Under-18:** Checkr will not screen minors; orgs can manually mark a minor as cleared and set the clearance to expire on their 18th birthday. Minors' **name and profile photo are not shown publicly or to other volunteers.**
- **Waivers** are e-signed and can be set as a per-event requirement, as can background-check status.
- **Reputation is one-directional.** Volunteer profiles carry an **attendance rating** visible to admins. There is no reciprocal record about the organization — no rating of whether the shift was as described, whether the site was safe, or whether the coordinator showed up.
- **Location:** POINT states it does not collect *"precise geographic location"* — only *"approximate location"* to suggest addresses and find nearby opportunities.

## 1.7 Privacy posture

Worth reading closely, because it is nearly the inverse of Handraise's design.

POINT collects name, email, phone, date of birth, address, interests, activity records, profile photos, payment information, documents, emergency contacts, plus IP/device/cookie telemetry.

**Organizations receive, for volunteers they work with:** *"name and contact information; date of birth; address; emergency contact information; application and form responses; volunteer registrations; attendance and volunteer hours."* Organizations may also pipe this into their own CRM or donor platform — transmitted *"at the organization's direction and for the organization's use."*

That last clause is the crux: **once a volunteer registers for an event, their personal data leaves POINT's governance and enters the nonprofit's, by design.** There is no staged disclosure, no purpose-binding, and no post-engagement retraction. The model assumes the org is the trusted party.

Other points: POINT *"does not sell volunteer personal information."* Retention is *"for as long as reasonably necessary."* Users can request access, correction, deletion, and opt-out — but *"Deactivating your account does not necessarily delete all information associated with it."* App Store privacy labels: contact info (address, email, name, phone) and user/device ID **linked to identity**; analytics and diagnostics not linked.

## 1.8 Commercial terms

| Plan | Price | Notable contents |
|---|---|---|
| **Free** | **$0, "Free Forever"** | Unlimited volunteers, admins, events, groups; scheduling, automated messaging, time tracking, reporting, website widgets, integrations, volunteer portal + mobile app, e-waivers, Checkr integration, document library. 20-min onboarding, office hours, chat support. |
| **Pro** | **$99/mo billed annually** (list $140/mo) | Applications, programs/sub-programs, **check-in kiosk + QR**, recurring opportunities, waiver management, document storage, custom questions/fields, custom reports. 1-hour onboarding, priority support, SMS add-on bundles. |
| **Teams** (beta) | **$199/mo billed annually** (list $299/mo) | Role-based access control, admin task management, program segmentation, branded communication, multi-session onboarding and team training. |
| **Network** | Custom / contact sales | Multi-org aggregation, network-wide reporting, partnerships, visibility controls. |
| **Companies** | Demo-gated | Corporate branding, private employee events, impact goals, donation and in-kind reporting. |

No contracts on Free/Pro/Teams, no setup fees, 14-day trial without a credit card.

**Numbers that don't reconcile.** The homepage claims *"Trusted by 7000+ Nonprofits"*; review-site copy still says *1,500+*. The VolunteerHub comparison page prices Pro at **$80/mo** while the pricing page says **$99/mo**. Both are probably stale-marketing artifacts rather than deception, but treat any single POINT figure as approximate.

## 1.9 Go-to-market

- **Bottom-up freemium** into nonprofits, with a free tier generous enough that evaluation friction is near zero.
- **Embedded distribution**: the website widget puts POINT's registration flow on thousands of nonprofit sites, which is how volunteers arrive.
- **Top-down Networks** deals (United Way, city governments, foundations) that onboard many orgs at once.
- **Corporate CSR** as the revenue engine and the moral story simultaneously.
- **SEO comparison pages** (`/point-vs-volunteerhub/`, `/compare-features/`) and heavy review-site badge farming (G2, Capterra, Software Advice, GetApp, *"Top 25 Nonprofit Tool of 2026"*).
- **Logo credibility**: Special Olympics, Salvation Army, Alzheimer's Association, American Red Cross, YMCA, United Way, Meals on Wheels.

## 1.10 Assessment

**Genuine strengths**

1. The free tier is a strategic weapon, not a trial. It removes the budget conversation from a sector that has no budget.
2. Operational depth where it counts: shifts, capacity, kiosk/QR check-in, hour verification, per-program reporting, exports. This is the daily grind of a volunteer coordinator, and it is solved.
3. Compliance is packaged (Checkr, e-waivers, applications, per-event requirements) without POINT assuming liability for the judgment.
4. Support is the most consistently praised thing in reviews — free one-on-one help including evenings and weekends.
5. Networks solves org-side cold-start structurally rather than through marketing spend.
6. Cross-subsidy alignment: the paying customer (corporate) genuinely wants the free customer (nonprofit) to succeed.

**Structural limitations**

1. **Individuals cannot create demand.** There is no way for a person to say "my tap is leaking." Every opportunity descends from an organization. This is not a missing feature; it is the shape of the business.
2. **Only event-shaped work exists.** Scheduled, group, repeatable. A bespoke, negotiated, one-off task has no representation.
3. **No negotiation layer.** Signup is unilateral — the volunteer takes a slot. There is no bilateral confirmation, no agreed scope, no material-change re-consent. Cancellation is a scheduling event, not a broken agreement.
4. **Asymmetric trust.** The org vets the volunteer; nothing vets the org for the volunteer.
5. **No AI layer** is visible in any public material — no need structuring, no screening, no matching intelligence.
6. **Discovery is filtering, not matching.** Cause + city + time window, with your own org's events pinned on top. No capability, no fit, no constraint modelling.
7. **Consumer-side weakness.** 4.0/88 ratings; a top review complaint about not being able to browse multiple opportunities; a request for multi-cause selection. Volunteers arrive through orgs, not through the app.
8. **US-shaped.** 501(c)(3) eligibility, Checkr (US-only), all-50-states framing, English-first (Ukrainian recently added). Porting to Israel would mean replacing the org-eligibility test, the background-check vendor, and the compliance assumptions wholesale.
9. **Reviewer-reported gaps** — inability to publish one event across multiple days in a batch (Pro's "recurring opportunities" may since have addressed this; the review and the feature list conflict).

**What POINT proves, and what it doesn't**

*Proves:* nonprofits will adopt free volunteer-ops software at real scale; corporate CSR budgets will fund it; the recurring pain is check-in, hours, and compliance reporting; anchoring supply to organizations solves cold-start.

*Does not prove:* that a peer-to-peer help network works; that consumer-side volunteer demand exists independently of an organization pushing it; that anyone will pay for coordination as opposed to administration.

---

# Part 2 — POINT vs. Handraise

## 2.1 The headline

**These are not competitors in the ordinary sense. They are different products that share a vocabulary.**

POINT is a **system of record for volunteer program administration**, sold to organizations. Handraise is a **trust and coordination network for bilateral help**, including help between two private individuals — a category POINT structurally cannot enter without changing who its customer is.

The overlap is roughly one use case out of six: *a validated nonprofit publishes a shift-based group opportunity that individuals sign up for* (Handraise UC-3 animal shelter, UC-4 community event, FR-12 group volunteering). In that overlap, **POINT is substantially more capable and free.**

## 2.2 Dimension-by-dimension

| Dimension | POINT | Handraise (per PRD) |
|---|---|---|
| **Primary customer** | Nonprofit admin; corporate CSR lead | Individual Requester and Individual Helper; orgs secondary |
| **Who pays** | Companies and Networks, cross-subsidizing free nonprofits | **Unspecified — the PRD contains no business model** |
| **Unit of work** | `Event` → `Shift` → registration | `HelpRequest` → `Interest` → `Engagement` (+ versioned `EngagementTerms`) |
| **Who can create demand** | Validated 501(c)(3)s and companies only | Individuals **and** validated עמותות / חל"צ (FR-2) |
| **Commitment model** | Unilateral: volunteer takes a slot | **Bilateral explicit confirmation of the same terms version** (FR-9, AC-1) |
| **Scope negotiation** | None | Mandatory clarification of scope, timing, conditions, safety, expenses (FR-9) |
| **Change after commitment** | Admin edits the event | **Material change requires renewed bilateral consent; prior terms stand until both re-confirm** (FR-9, AC-3) |
| **Declining** | Unregister — a scheduling action | Explicitly **neutral, unrecorded, un-notified** pre-confirmation (FR-11, AC-2) |
| **Matching** | Filter: cause + city + time; own-org events pinned first | Fit ranking on interests, capabilities, availability overlap, distance, private preferences, safety requirements (FR-7) |
| **Urgency** | Not a modelled concept | Displayed as context, **never used to rank**; no scarcity or countdown mechanics (FR-7, AC-17) |
| **Identity verification** | None at platform level; orgs run Checkr | **Tiered: Tier 0 email+phone, Tier 1 gov ID + liveness. Verification data private; only status is public** (FR-1) |
| **Trust record** | One-directional; attendance rating visible to admins | **Reciprocal, structured, double-blind** across clarity/respect/communication/reliability/safety/outcome; aggregates only at ≥3 ratings; **no composite score anywhere** (FR-13, FR-14, AC-14) |
| **Public free text** | N/A | Deliberately **never public** (FR-13) |
| **Safety adjudication** | Delegated to the organization; POINT supplies waivers + Checkr | **Platform-level**: prohibited-category taxonomy, AI screening with block + human appeal, safety-sensitive labelling, per-engagement bilateral attestation with versioned text (FR-5, FR-15, FR-16) |
| **Evidence of competence** | Checkr status; org's own applications/references | User-uploaded, **labelled user-provided and unverified on every surface** (FR-15, AC-10) |
| **Privacy model** | Org receives full contact data on registration and may export to its own CRM | **Staged disclosure**: area-level pre-confirmation; address + phone only on confirmation; hidden 14 days after closure; **enforced server-side** (FR-10, AC-11) |
| **Private preferences** | N/A | Never returned by any endpoint another user can reach (FR-3, AC-12) |
| **AI** | None visible | Central: request structuring, screening, profile suggestions — with hard boundaries (never publishes, never edits a public profile, never confirms) (FR-4, §5.3, AC-4) |
| **Money** | None; links out to donation tools | None; **records agreed expense cap only** (FR-19) |
| **Exploitation / staff substitution** | Not a concept — the orgs are the customer | First-class: cumulative pattern tracking, bilateral nudges, operator escalation, no hard caps (FR-18) |
| **Group volunteering** | Deep: shifts, capacity, group reservations, kiosk/QR check-in, shift printouts, templates | Basic: roles, shifts, capacity, individual Engagement per Helper (FR-12) |
| **Hours & reporting** | Core: self-submission, admin verification, granular reports, PDF/CSV export, network aggregates | **Explicitly out of scope** (§6.2: no volunteer-hours export for institutional reporting) |
| **Integrations** | SSO, Kindful CRM, five fundraising tools, calendars, website widgets | **Explicitly out of scope**; no public API in v1 |
| **Jurisdiction** | US-shaped and US-locked in practice | Israel pilot, **jurisdiction-as-configuration** (§5.2) |
| **Languages** | English; Ukrainian added 2026 | Hebrew (full RTL) + English, with **Hebrew screening quality as a launch gate** (FR-20, AC-7) |
| **Platforms** | iOS, Android, volunteer web portal, admin web dashboard | **Native iOS + Android only**; no web client; internal operator console |
| **Accessibility** | Not publicly stated | WCAG 2.2 AA equivalent, VoiceOver/TalkBack, dynamic type — explicit, because the Requester population skews elderly and recovering (§5.3, AC-20) |
| **Scale today** | 7,000+ nonprofits claimed, all 50 states, 4.0/88 iOS ratings | Zero. Target: ~500–1,000 verified users, ≥100 completed engagements in one city over 3 months |

## 2.3 Where Handraise is genuinely differentiated

Four things POINT cannot copy without becoming a different company:

1. **Individual-to-individual demand.** POINT's customer is the organization; its free tier, its compliance model, and its data-sharing policy all assume an institutional intermediary. Serving a private person's leaking tap has no revenue path and no liability shelter for them.
2. **Bilateral confirmation with versioned terms.** POINT's primitive is a slot. Handraise's is an agreement. Retrofitting negotiation onto event registration would break every existing report.
3. **Reciprocal trust.** POINT's asymmetry is not laziness — its customer *is* the org, so rating the org is rating the customer. Handraise has no such conflict.
4. **Purpose-bound privacy.** POINT's privacy policy exists to authorize org access to volunteer data. Handraise's exists to restrict it. These are opposite designs and cannot be reconciled inside one product.

Add to that AI-structured requests, urgency-never-ranks, and exploitation-pattern detection as concept-level commitments POINT has no reason to make.

## 2.4 Where Handraise is weaker, and will be told so

If Handraise pitches a nonprofit on group volunteering, the org will compare it to POINT and find Handraise missing: **hour tracking and export, kiosk/QR check-in, applications, waivers, program hierarchies, CRM integration, admin web dashboard, custom reports, group reservations, event templates.** All of that is explicitly out of scope in §6.2 — correctly, for validating the model — but it means **orgs are a liquidity strategy, not a customer segment,** and the PRD should say so.

The blunt version: *POINT is free and does org volunteer ops better. Handraise should not try to win that comparison; it should decline to enter it.*

## 2.5 Three uncomfortable findings

**Finding 1 — POINT's consumer traction validates Handraise's stated liquidity risk, and makes it worse.**
POINT has 7,000+ organizations pushing volunteers into its app and still shows 88 iOS ratings. Consumer-side pull in this category is weak; supply arrives because an org sent it. Handraise assumption **A6** (pilot liquidity without paid acquisition, via a partner org and community outreach) is therefore not merely optimistic — it is betting on the harder half of a mechanism that even a subsidized incumbent only makes work top-down. **The partner organization in Q1 is not a nice-to-have; it is the acquisition channel.**

**Finding 2 — the PRD has no business model, and POINT has already set the price to zero.**
The PRD's metrics measure whether the model works, never whether it can pay for itself. That is defensible for a 3-month pilot, but two things make it urgent: Tier 1 verification carries a **hard per-check unit cost** (Q2/A3), and POINT has established that nonprofits will not pay for volunteer coordination. Whatever funds Handraise — corporate CSR, municipal contract, philanthropic grant, individual-side premium — needs an owner before Q2's vendor negotiation, because the acceptable per-verification price depends entirely on the answer.

**Finding 3 — Handraise carries liability POINT deliberately never touches.**
POINT never enters the help relationship: the org recruits, screens, waives, and supervises; POINT provides software. Handraise puts a verified stranger inside an elderly person's home under platform-brokered terms, with platform-run screening and platform-defined attestations. That is a materially different exposure. **Q4 (volunteer-liability and insurance review) is currently ordered below threshold-setting and taxonomy work; it belongs at or near the top.**

## 2.6 Worth borrowing from POINT

- **Kiosk / QR check-in** for group shifts. Cheap, high-value, and the thing coordinators actually use. Reasonable to keep out of v1, but note it as the first org-side follow-up.
- **"Request that an organization join."** Free demand-signal capture during cold-start, and it converts a dead-end empty feed into a data point.
- **Volunteer hour self-submission + counterparty verification.** See the defect in §2.7 — Handraise needs some version of this already.
- **Free-tier-as-weapon.** If Handraise ever does court orgs, it cannot charge them. Plan around that now.
- **Networks as a structural analogue** to Handraise's jurisdiction-as-configuration layer — one umbrella entity onboarding many orgs is a cleaner expansion mechanic than city-by-city marketing.
- **Ukrainian localization** in a US-first product is a useful signal: volunteering demand concentrates in displaced and minority-language populations. Relevant to the Israel pilot beyond Hebrew/English.

## 2.7 Concrete PRD changes this analysis suggests

1. **Defect — FR-18 references data no requirement captures.** FR-18 says *"Track cumulative engagement count and reported hours per Requester–Helper pair,"* but no FR defines hour capture. FR-11 records outcomes, not durations, and §6.2 excludes hours reporting. Either add duration capture to FR-11's outcome report, or restate FR-18 in terms of engagement counts and scheduled duration from `EngagementTerms`. **This blocks implementation as written.**
2. **Add a non-goal:** *"Not a volunteer program management system of record. Handraise does not compete with POINT-class volunteer-ops platforms on hours, compliance, or institutional reporting; organizations are a liquidity source for the pilot, not a customer segment."*
3. **Add Q10 — funding and unit economics.** Who pays, and what is the ceiling per Tier 1 verification? Owner: Pilot Program Lead, before Q2 vendor selection.
4. **Re-rank Q4 (liability/insurance) upward,** per Finding 3.
5. **Add to §9 Alternatives Considered:** *"Build on, or partner with, an existing volunteer management platform (POINT, Golden, VolunteerHub). Rejected: their primitive is an organization-owned event with unilateral registration, which cannot express an individual's negotiated need, bilateral confirmation, staged disclosure, or reciprocal trust. The overlap is one use case; the mismatch is the other five."*
6. **Reconsider one line in §6.2.** Excluding volunteer-hours export is right for individuals and wrong for orgs — it is the single most requested org feature in this category. Not a v1 change, but decide deliberately whether org adoption in the pilot survives its absence, and record the decision.
7. **Strengthen Q1.** Reframe from *"is there a launch partner?"* to *"which partner organization is the acquisition channel, and what volume of requests will it seed in month one?"* — per Finding 1.

## 2.8 Bottom line

POINT is a well-run, sensibly financed, operationally deep **volunteer administration platform** whose free tier and cross-subsidy make it very hard to displace on its own ground — and whose entire architecture presumes an organization stands between the helper and the helped.

Handraise's thesis lives precisely in the space that presumption excludes: **help between two people who do not share an institution.** That space has no incumbent in this analysis, which is either the opportunity or the warning. POINT's own numbers suggest the reason: consumer-side volunteering demand is hard to activate without an organization pushing it, and Handraise has chosen to test exactly that — with a heavier consent flow, real per-user verification cost, and no revenue model yet named.

The comparison does not invalidate the PRD. It sharpens three things it currently underweights: **who seeds liquidity, who pays, and who carries the liability when a stranger enters a home.**

---

## Sources

- [POINT homepage](https://pointapp.org/) · [Volunteers](https://pointapp.org/volunteers/) · [Plan & feature comparison](https://pointapp.org/your-volunteer-command-center/) · [Pricing](https://pointapp.org/pricing/) · [About / mission](https://pointapp.org/about-our-mission/) · [Companies](https://pointapp.org/companies/) · [Networks](https://pointapp.org/network/) · [Mobile app](https://pointapp.org/nonprofit-features/point-mobile-app/) · [vs. VolunteerHub](https://pointapp.org/point-vs-volunteerhub/) · [Privacy policy](https://pointapp.org/privacy-policy/)
- [POINT support KB — background checks](https://support.pointapp.org/knowledge/how-background-checks-work-on-point) · [packages](https://support.pointapp.org/knowledge/background-checks-packages) · [report access](https://support.pointapp.org/knowledge/who-has-access-to-my-background-check-report)
- [App Store — POINT: Volunteer near you](https://apps.apple.com/us/app/point-volunteer-near-you/id1173737657) · [Google Play](https://play.google.com/store/apps/details?id=org.pointapp.point)
- [G2 reviews](https://www.g2.com/products/point-point/reviews) · [Capterra reviews](https://www.capterra.com/p/208936/POINT/reviews/) · [Software Advice](https://www.softwareadvice.com/nonprofit/point-profile/) · [GetApp](https://www.getapp.com/nonprofit-software/a/point/)
- [Crunchbase — POINT](https://www.crunchbase.com/organization/point-app) (access blocked) · [Forbes, 2019](https://www.forbes.com/sites/lizengel/2019/12/24/this-app-wants-to-make-volunteering-as-easy-as-getting-an-uber/) · [TechOhio, 2019](https://weare.techohio.ohio.gov/2019/03/06/point-app-makes-volunteering-as-easy-as-ordering-food/) · [POINT blog — Sea Change](https://pointapp.org/blog/point-sea-change/)
