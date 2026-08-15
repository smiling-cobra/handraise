# TaskLink — A Trusted, Transparent Labor Exchange

## The Pitch

Finding reliable help for a task — or finding trustworthy work — is still messier than it should be. Businesses post vague job ads and wade through unqualified applicants. Workers apply blind, with no clarity on what's really expected, whether they'll get paid on time, or whether the task matches what they're actually good at and want to do.

**TaskLink** is a client-server marketplace that connects businesses needing a specific task done with nearby, verified workers whose skills, availability, and preferences genuinely fit that task — combining the immediacy of **Wolt/Bolt** (see it, request it, get it fast), the structured trust-building of **Upwork** (verified skills, evidence levels, track record), and the frictionless task-posting of **Fiverr** (clear scope, fast matching) — but purpose-built for real-world, often physical or on-site work, not just digital gigs.

The core belief: **matching quality beats matching speed**, but speed still matters. So the system leans on AI to do the tedious structuring work (turning a messy task description into a standardized, publishable order) while keeping every human decision — accepting a task, approving completion, verifying a license — squarely in human hands.

### What makes it different

| | Wolt / Bolt-Uber | Fiverr | Upwork | **TaskLink** |
|---|---|---|---|---|
| Speed of match | Near-instant | Days | Days–weeks | Instant offers for simple tasks, feed-based for complex ones |
| Task scope | Fixed, narrow (ride, delivery) | Seller-defined packages | Long-form proposals | AI-standardized structured orders |
| Trust model | Ratings only | Ratings + portfolio | Ratings + verified history | Layered evidence model (L0–L5), separate from ratings |
| Physical/on-site work | Yes (delivery/rides only) | No | No | Yes — built for it (PPE, worksite conditions, physical requirements) |
| Messaging before commitment | N/A | Open | Open | Gated — no DMs until an order exists, reducing spam and off-platform leakage |
| Profile use | Static | Static, self-managed | Static, self-managed | **Dynamic** — profile evolves from real task outcomes, worker can always see and correct it |

### Design principles

- **Transparent by default** — order status is always one of a small, well-defined set (SUBMITTED, PENDING, ACCEPTED, APPROVED, REJECTED/DECLINED, WITHDRAWN, EXPIRED). No ambiguous limbo states.
- **AI proposes, humans decide** — AI drafts the structured task (category, required skills, risk class, missing info) but the business must approve it before it goes live, and AI can never fabricate evidence or waive a required license.
- **Progressive, conversational onboarding** — no 40-field forms up front; the system learns the worker over time, the same way a good staffing agency would.
- **Two-layer profile privacy** — a public professional profile (what businesses need to decide) and a private matching profile (preferences used only for recommendation, never exposed).
- **Fairness in tracking** — declining an offer is neutral; only real failures (no-shows, last-minute cancellations, mismatched execution) are tracked as reliability signals.

---

## Core Entities & Roles

- **Order** — the central unit of work, short- or long-term, moving through a fixed lifecycle.
- **Customer** — posts orders, reviews and hires workers, approves completed work.
- **Worker** — discovers and accepts orders, executes them, builds a verified track record over time.

Role is chosen once at account creation (customer or worker), each with its own profile, settings, and feed.

---

## User Stories

### Epic 1 — Getting Started

**As a new user**, I want to choose whether I'm a customer or a worker when I create my account, so that the app tailors onboarding, my profile, and my feed to what I actually need.

**As a new customer**, I want a guided onboarding that captures my identity, billing status, saved locations, typical tasks, and worksite conditions, so that future orders are faster to create and better matched.

**As a new worker**, I want a conversational, step-by-step onboarding (not one long form) covering my identity, location, availability, CV, portfolio, certificates, tools, languages, and preferences, so that I'm not overwhelmed and can complete it in stages.

**As a new worker**, I want to be asked practical questions like which environments I'm comfortable in, whether I prefer exact instructions or outcome-based freedom, and what work I don't want to receive, so that the system avoids sending me mismatched tasks from day one.

**As a new customer**, I want to browse and search worker profiles before committing, so that I can gauge fit — but I understand I can't message a worker directly until I actually create an order, so that the platform stays focused and spam-free.

### Epic 2 — Creating & Publishing an Order

**As a customer**, I want to describe my task in my own words and have AI propose a standardized title, category, required and optional skills, minimum proficiency, risk class, and safety flags, so that I don't have to know the "correct" taxonomy myself.

**As a customer**, I want to review and approve (or edit) the AI-structured requirements before the order is published, so that I stay in control of exactly what gets published under my name.

**As a customer**, I want the AI to flag missing information (e.g., unclear physical requirements or safety needs) before publishing, so that I catch gaps before a worker does.

**As a customer with a simple, clearly-specified task**, I want to send an instant offer directly to eligible nearby workers, so that urgent, well-defined jobs (in the spirit of Wolt/Bolt) get filled in minutes, not days.

**As a worker**, I want an instant offer to expire automatically and reserve my capacity the moment I accept it, so that I never lose a job to a double-booking or a stale request.

### Epic 3 — Discovery & Matching

**As a worker**, I want to see relevant published orders on a job feed filtered to my skills, location, and preferences, so that I'm not scrolling through irrelevant listings.

**As a worker**, I want to receive a notification when an order matching my capabilities is published, so that I can act on good-fit opportunities quickly.

**As a worker**, I want to place a standing service offer visible in the feed, so that customers looking for my exact skill set can find and approach me proactively.

**As a customer**, I want to search for workers by skill, evidence level, and rating, so that I can shortlist candidates before deciding whether to create an order.

### Epic 4 — The Order Lifecycle

**As a Worker, I want to accept or ignore an order I've been notified about, so that I retain control over what I take on — and I want declining to carry no penalty, so that I'm not pressured into bad-fit work:**

* I can receive and view notifications once a relevant order was placed, with the order description
* I can receive direct offers from a Customer
* I can view orders myself via the job feed
* I can search for orders via search or job feed
* I can accept or ignore a notified order at my own discretion, with no penalty for ignoring
* I can decline an order after having accepted it, with the reason recorded separately from a no-show or last-minute cancellation

**As a customer or worker**, I want the ability to decline an order even after it's been accepted, so that genuine changes in circumstance don't trap either side.

**As a worker**, I want to explicitly click "Start" when I begin the job, so that there's an unambiguous, timestamped record of when execution began.

**As a worker**, I want to mark a job as completed, postponed, or failed, so that the true outcome is always reflected accurately, not forced into a binary done/not-done.

**As a customer**, I want to approve or decline a worker's completed job, so that I have the final say on whether the work meets what was agreed.

**As a customer**, I want to see a small, fixed set of order statuses (submitted, pending, accepted, approved, rejected/declined, withdrawn, expired) at every stage, so that I always know exactly where things stand without guessing.

### Epic 5 — Trust, Skills & Verification

**As a customer**, I want to see each worker's skill evidence level — from self-declared up to certificate/license-verified — so that I can judge how much to trust a claimed skill, especially for regulated or risky work.

**As a worker**, I want to build up my evidence level over time through AI-inferred confirmation, portfolio uploads, completed platform tasks, and verified certificates, so that my profile becomes more credible without repeated manual re-entry.

**As a worker**, I want confidence that AI will never invent evidence on my behalf or wave through a mandatory license requirement, so that my professional credibility stays real and defensible.

**As a customer**, I want to see category-specific ratings, completed-task metrics, and any safety-incident or task-mismatch reports, so that I can make an informed hiring decision beyond a single overall star rating.

### Epic 6 — The Living Worker Profile

**As a worker**, I want my profile to automatically evolve based on completed assignments, punctuality, business feedback, and repeated task categories, so that it stays accurate without me manually updating it constantly.

**As a worker**, I want a dedicated "How the app understands your work profile" view, so that I can see exactly what the system has inferred about me.

**As a worker**, I want to correct any inference the system has made, so that I stay in control of how I'm represented, even as the profile updates automatically.

**As a worker**, I want my private matching preferences (excluded task types, compensation thresholds, decline reasons) kept separate from my public professional profile, so that businesses see only what they need to make a hiring decision — not my personal filtering logic.

**As a worker**, I want last-minute cancellations and no-shows tracked separately from simple declines, so that the system distinguishes "I said no upfront" (neutral) from "I let someone down" (a real reliability signal).

---

## Suggested MVP Slice

To ship something coherent fast, an initial release could focus on:

1. Account creation with role selection + progressive worker onboarding.
2. AI-assisted order creation with mandatory customer approval before publishing.
3. Job feed + notifications (skip instant offers for v1).
4. Core lifecycle: accept → start → complete → approve, with the fixed status set.
5. Basic evidence model (L0–L2 only) — defer certificate/license verification (L4) and specialist verification (L5) to a later phase.

Instant offers, the full L0–L5 evidence ladder, and the dynamic self-correcting profile view are natural fast-follows once the core loop is validated.
