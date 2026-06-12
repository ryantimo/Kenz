# Product Requirements Document (PRD)
## Beirut Premium Fashion Resale Marketplace — MVP

**Version:** v1 (MVP)
**Owner:** Founder
**Audience:** Product & Engineering team
**Status:** Draft for build planning
**Last updated:** June 2026

> **How to read this doc.** Sections 1–18 are the PRD. Every feature is tagged
> **[MVP]** (build now), **[LATER]** (roadmap, do not build), or **[ADMIN]**
> (internal manual tool needed to operate at launch). Section 19 onward gives the
> screen-by-screen breakdown, the phased build sequence, the tech recommendation,
> and the exact follow-up prompts your developer should send next.

---

## 1. Product Summary

A peer-to-peer marketplace for pre-owned premium and luxury fashion (bags, shoes,
accessories, jewelry, selected clothing), launching in Beirut and Lebanon-first.
Inventory stays with the seller — no consignment, no warehouse. At launch only
**verified professional sellers** (boutiques, established resellers, curated
Instagram accounts) may list.

The transaction model is **offer-then-pay**: buyers do not buy instantly. They
submit a USD offer; the seller accepts, counters, or rejects; only after acceptance
does the buyer pay through the platform. Funds are held by the platform via a local
payment partner (Whish / MontyPay) and released to the seller only after the buyer
confirms delivery and the inspection window passes. The platform earns a commission
per completed sale.

This is a **trust-and-transaction product, not a beauty product**. The MVP's job is
to make one loop work reliably: **list → offer → accept → pay → ship → deliver →
release payout**, with a narrow, well-defined claims path for fakes and major
mismatches.

---

## 2. Problem Statement

Premium resale in Lebanon already happens at scale, but it is **fragmented and
trust-poor**:

- Supply and demand are scattered across Instagram, WhatsApp, and small physical
  shops. There is no centralized, searchable, trusted place to buy or sell.
- Buyers have **no protection**: they pay first (often by cash/transfer), with no
  recourse if the item is fake, wrong, or badly misdescribed.
- Authenticity is the dominant fear in luxury resale, and informal channels offer
  no structured verification.
- Payment is messy. Lebanon runs on **fresh USD** and a strong cash-on-delivery
  habit; people are reluctant to pay before holding the goods, and sellers struggle
  to collect reliably and chase buyers for money.
- Negotiation is cultural and expected, but there is no structured way to do it.

The opportunity: centralize fragmented supply behind a **trusted, payment-protected,
negotiation-native** layer, starting with sellers whose quality we can vouch for.

---

## 3. Target Users

**Buyers (demand side)**
- Fashion-conscious consumers in Lebanon (Beirut metro first) who buy premium/luxury
  pre-owned items.
- Hold and transact in fresh USD; accustomed to negotiation.
- Primary fear: authenticity and "will I actually get what I paid for."

**Sellers (supply side) — launch cohort**
- Professional resellers, boutiques, and curated Instagram resale accounts.
- Already move inventory informally; pain points are reach, trust signaling to new
  buyers, and getting paid reliably without chasing.
- Must pass KYC + manual vetting before they can list.
- *Individual sellers are explicitly out of scope for v1 (see §16).*

**Internal operators (admin side)**
- Small ops team that manually approves sellers, reviews high-risk listings, manages
  claims, and triggers payout releases. The MVP assumes humans in the loop.

---

## 4. Value Proposition

**For buyers**
- One trusted place to browse curated premium resale, with real photos, condition
  grades, and verified-seller badges.
- Payment protection: money is held and only released after delivery; a real claims
  path exists for counterfeit / wrong / badly misdescribed items.
- Negotiation built in (offer-then-pay), matching how people already buy.

**For sellers**
- Access to buyers beyond their own follower base.
- A trust credential (verified badge) that converts hesitant buyers.
- **They get paid reliably, in fresh USD, without chasing buyers** — the platform
  collects, holds, and pays out. This is the single strongest seller hook; lead with it.

**For the platform**
- A commission on every completed sale.
- A defensible trust position (verification + protected payments + dispute handling)
  that informal channels cannot match.

---

## 5. Business Model

- **Revenue:** commission on each completed sale (released funds), e.g. a take rate
  applied to the agreed price. *Exact % is an open question (§18).*
- **Fee structure decision (open):** whether the take is charged to the seller, the
  buyer (as a buyer-protection fee), or split. Recommendation for launch: keep the
  **seller take low** and consider a **buyer protection fee**, because the launch
  cohort can list for ~0% commission on Instagram and will resist a high seller cut.
- **Payment processing fees** from the partner (gateway/wallet) are a cost line and
  must be modeled into the take rate so sellers net more than they would informally.
- USD only. No LBP pricing or settlement in v1.

---

## 6. Core MVP Features (Must-Have Only)

| # | Feature | Tag |
|---|---------|-----|
| 1 | Seller application + KYC upload + manual approval | [MVP] |
| 2 | Verified-seller badge | [MVP] |
| 3 | Listing creation with **mandatory** photos + condition grade + brand/size/details | [MVP] |
| 4 | Admin listing review queue (manual, at least for high-value items) | [MVP][ADMIN] |
| 5 | Browse + search + filters (category, brand, size, price) | [MVP] |
| 6 | Listing detail with evidence (photos, grade, badge, details) | [MVP] |
| 7 | Make-offer (USD) + offer thread (accept / counter / reject) | [MVP] |
| 8 | Pay-after-acceptance via payment partner | [MVP] |
| 9 | Platform-held funds with manual/triggered release (secure hold, not legal escrow) | [MVP] |
| 10 | Shipping confirmation + delivery proof (chain-of-custody capture) | [MVP] |
| 11 | Delivery confirmation + inspection window → payout release | [MVP] |
| 12 | Claims flow (counterfeit / wrong item / major misdescription only) | [MVP] |
| 13 | Seller payout (fresh USD to bank/approved wallet) | [MVP][ADMIN] |
| 14 | Ratings/reviews (basic) | [MVP] |
| 15 | Admin dashboards: seller approval, listing review, claims, payout release | [MVP][ADMIN] |

Everything not on this list is **[LATER]** (see §16).

---

## 7. Buyer Flows

**7.1 Browse & discover**
Home/discovery → filter by category, brand, size, price → open listing detail.

**7.2 Evaluate listing**
Listing detail shows: real photos, condition grade, brand/size/details, seller badge,
seller rating, authenticity notes, and an **Make Offer** button. *No instant "Buy now"
by default.*

**7.3 Make an offer**
Buyer submits a USD offer (optionally a message). Offer enters `PENDING`.

**7.4 Negotiate**
Seller accepts, counters, or rejects. Counters loop in an offer thread until accepted,
rejected, or expired. Buyer is notified of each state change.

**7.5 Pay (only after acceptance)**
On `ACCEPTED`, buyer gets a time-boxed payment window. Buyer pays via the payment
partner. Funds move to platform-held status (`PAID_HELD`). Item is locked/marked
reserved so it cannot be sold twice.

**7.6 Fulfillment & confirmation**
Buyer sees order tracking: awaiting shipment → shipped → delivered. On delivery, an
**inspection window** opens (e.g. 24–72h). Buyer confirms receipt or opens a claim.

**7.7 Completion**
If buyer confirms (or window passes with no claim) → order `COMPLETED`, funds released
to seller. Buyer can rate the seller.

**7.8 Claim (exception path only)**
Within the inspection window, buyer may open a claim for **counterfeit / wrong item /
major misdescription** only. No "change of mind" returns. Claim freezes the held funds
and routes to admin review.

---

## 8. Seller Flows

**8.1 Apply & verify**
Apply as seller → upload KYC (ID, business proof where relevant) → status `PENDING_REVIEW`.
Admin approves/rejects. Approved sellers get the verified badge.

**8.2 List**
Create listing with **mandatory** fields: photos (min count), condition grade, brand,
size, category, description, authenticity details. High-value listings (above a
configurable threshold) route to admin review before going live; others may go live and
be reviewed asynchronously.

**8.3 Receive & handle offers**
Offers arrive in an Offers inbox. Seller accepts, counters, or rejects. Acceptance
triggers the buyer payment window.

**8.4 Ship & prove**
After payment is held, seller ships and **uploads shipping proof** (courier, tracking,
pickup photo / sealed-package evidence). This is the seller's side of chain-of-custody
(see §10).

**8.5 Get paid**
After delivery confirmation + inspection window with no upheld claim → payout released
in fresh USD to the seller's bank or approved wallet, minus commission and fees. Seller
sees payout status.

**8.6 Reputation**
Seller accrues ratings; repeated claims or violations affect badge/standing
(admin-controlled in v1).

---

## 9. Admin / Back-Office Flows

The MVP is **operated by humans**. These tools are not optional polish — they are how
the business runs on day one.

- **[ADMIN] Seller approval queue:** review KYC, approve/reject, assign badge.
- **[ADMIN] Listing review queue:** review high-value / high-risk listings before or
  shortly after they go live; flag, request edits, or remove.
- **[ADMIN] Offers/orders monitor:** read-only view of live transactions and their state.
- **[ADMIN] Claims/disputes dashboard:** view claim, evidence (photos, unboxing video,
  shipping proof), decide refund-to-buyer vs release-to-seller, log resolution.
- **[ADMIN] Payout release dashboard:** see orders eligible for release, confirm/trigger
  payout, track payout status and failures.
- **[ADMIN] Audit log:** every admin action recorded (who, what, when).

---

## 10. Trust & Safety Logic

Trust is the product. The MVP enforces it at four control points:

1. **Seller gating.** Only KYC-verified, manually-approved professional sellers can
   list. Verified badge is the buyer-facing signal.

2. **Listing quality enforcement.** Mandatory photos + condition grade + structured
   fields. Manual admin review for high-value items. Listings cannot go live missing
   required evidence.

3. **Authenticity responsibility + optional verification step.**
   - Sellers are **contractually responsible** for authenticity, ownership, and accurate
     description (captured at onboarding).
   - **[MVP, configurable]** Items above a value threshold (e.g. $500–800) may require an
     extra **authentication checkpoint** before payout release — e.g. a trusted in-house
     authenticator, a partner service, or a verification photo protocol. This is the
     cheap insurance against the one fake that damages the brand. Keep it threshold-gated
     so it doesn't slow low-value flow.

4. **Chain of custody at the handoff** *(the riskiest gap in a no-warehouse model)*.
   Because the platform never holds the item, disputes are otherwise he-said-she-said.
   Mitigate by capturing neutral evidence:
   - Seller uploads **shipping/pickup proof** (sealed package photo, courier + tracking).
   - Recommend **tamper-evident packaging** for high-value items.
   - Buyer is prompted to record an **unboxing video** as condition for a valid claim.
   - Use a **known courier** as the neutral witness where possible.
   These artifacts are what the claims process adjudicates against.

---

## 11. Payment & Payout Logic

**Important framing:** v1 uses **platform-managed secure hold**, *not* true legal escrow.
The platform collects via a payment partner, holds funds, and releases on a trigger.
(The regulatory implications of holding third-party funds are a real open risk — see §18.)

**Flow**
1. Buyer pays only **after** seller accepts the offer.
2. Funds are collected via partner (Whish Pay / MontyPay) and moved to **platform-held**
   status (`PAID_HELD`). Item is locked.
3. Funds are **released to the seller** only when: buyer confirms delivery **OR** the
   inspection window elapses with no claim **AND** (if applicable) the authentication
   checkpoint passes.
4. If a claim is opened and **upheld**, funds are **refunded to the buyer** (full or
   partial per resolution). If **rejected**, funds release to the seller.
5. Payouts to sellers are in **fresh USD** to bank or approved wallet, minus commission
   and processing fees.

**Hard constraints**
- USD only end to end. No LBP.
- Inflows and outflows must stay in the **fresh-dollar** rail (post-Oct-2019 accounts/
  wallets), never routed through frozen/legacy deposits.
- Payout reliability is a launch-critical promise; instrument it and treat payout
  failures as P0 incidents.

---

## 12. Dispute & Claims Logic

**Eligible claim reasons (only these):**
1. Counterfeit / not authentic.
2. Wrong item received.
3. Major misdescription (materially different from listing).

**Not eligible:** change of mind, minor wear consistent with grade, buyer's remorse.
**Final sale** is the default.

**Process**
- Claim must be opened within the **inspection window** after delivery.
- Opening a claim **freezes the held funds**.
- Buyer submits evidence (photos, unboxing video, description of issue).
- Admin reviews against listing, shipping proof, and authentication record.
- Resolution: **refund buyer** (return logistics defined per case) or **release to seller**.
- Seller carries **primary liability** for counterfeit / serious misrepresentation per
  their contract; repeated upheld claims affect standing/badge.
- Every claim and decision is logged for audit and pattern detection (anti-abuse).

**Anti-abuse note:** the claim rules must be tight enough that they can't be farmed
(e.g. buyer swapping in their own fake). The unboxing-video + shipping-proof requirements
are the evidentiary backbone.

---

## 13. Information Architecture / Sitemap

```
PUBLIC / BUYER
  Home (Discovery)
    └─ Search + Filters
        └─ Listing Detail
            ├─ Make Offer (modal)
            └─ Offer Thread / Negotiation
                └─ Payment
                    └─ Order Tracking
                        ├─ Confirm Delivery
                        └─ Claim / Report Issue
  Account
    ├─ My Offers
    ├─ My Orders
    └─ Profile / Auth

SELLER
  Seller Application
    └─ KYC Verification
  Seller Dashboard
    ├─ Create / Edit Listing
    ├─ My Listings
    ├─ Offers Inbox
    │   └─ Offer Detail (accept / counter / reject)
    ├─ Orders → Shipping Confirmation (+ proof upload)
    └─ Payout Status

ADMIN (back-office)
  Seller Approval Queue
  Listing Review Queue
  Orders / Offers Monitor
  Claims / Disputes Dashboard
  Payout Release Dashboard
  Audit Log
```

---

## 14. Required Screens

**Buyer (8)**
1. Home / discovery
2. Search + filters
3. Listing detail
4. Make-offer modal
5. Offer thread / negotiation
6. Payment screen
7. Order tracking (+ confirm delivery)
8. Claim / report issue

**Seller (7)**
9. Seller application
10. KYC verification
11. Seller dashboard
12. Create / edit listing
13. Offers inbox (+ offer detail)
14. Shipping confirmation (+ proof upload)
15. Payout status

**Admin (5–6)**
16. Seller approval queue
17. Listing review queue
18. Claims / disputes dashboard
19. Payout release dashboard
20. (Orders/offers monitor + audit log — can share one shell)

Plus shared/auth: login/register, profile. **~16 primary screens** for v1.

---

## 15. Data Model / Main Entities

| Entity | Key fields | Notes |
|--------|-----------|-------|
| **User** | id, role (buyer/seller/admin), name, email, phone, auth, fresh-USD payout details | Role-based access |
| **SellerProfile** | user_id, status (pending/approved/rejected), kyc_docs, badge, business_info, standing | Gated by admin |
| **Listing** | id, seller_id, category, brand, size, condition_grade, price_usd, photos[], description, authenticity_details, status (draft/in_review/live/reserved/sold/removed) | Mandatory media + grade |
| **Offer** | id, listing_id, buyer_id, amount_usd, status (pending/accepted/countered/rejected/expired), parent_offer_id, messages[] | Thread via parent_offer_id |
| **Order** | id, listing_id, buyer_id, seller_id, accepted_offer_id, amount_usd, commission, status (see state machine) | Created on payment |
| **Payment** | id, order_id, gateway_ref, amount_usd, hold_status (held/released/refunded) | Partner integration |
| **Payout** | id, order_id, seller_id, amount_usd, method, status (pending/sent/failed) | Fresh USD only |
| **Shipment** | id, order_id, courier, tracking, pickup_proof, delivery_proof, status | Chain of custody |
| **Claim** | id, order_id, reason (counterfeit/wrong/misdescription), evidence[], status (open/under_review/upheld/rejected), resolution | Inspection-window gated |
| **Review** | id, order_id, rater_id, ratee_id, rating, comment | Basic |
| **Category / Brand** | id, name | Reference data for filters |
| **AdminAction / AuditLog** | id, admin_id, action, target, timestamp | Every admin op |

**Order state machine (the heart of the system):**
```
OFFER_ACCEPTED → AWAITING_PAYMENT → PAID_HELD → AWAITING_SHIPMENT
   → SHIPPED → DELIVERED → INSPECTION_WINDOW
        → COMPLETED (funds released → payout)
        → CLAIM_OPENED → UNDER_REVIEW → { REFUNDED | RELEASED }
```

---

## 16. Out of Scope for v1 (Roadmap / [LATER])

- Individual (non-professional) sellers.
- Instant "Buy Now" by default.
- AI try-on / virtual fitting.
- Auctions, live bidding, live shopping.
- Complex social features (feeds, following, comments, social graph).
- In-house warehouse / consignment operations.
- Cross-border / regional MENA expansion and multi-currency.
- LBP pricing or settlement.
- Native mobile apps if a responsive web/PWA is faster to ship (revisit post-launch).
- Automated/AI authentication at scale (start manual + threshold-gated).
- Loyalty, referrals, promotions engine.

---

## 17. Recommended Launch Metrics

**Loop health (primary)**
- Listings created / week (supply).
- Offer rate (offers per listing view).
- Offer → acceptance rate.
- Acceptance → payment conversion.
- **Successful loop completion rate** (paid → delivered → released).
- Time-to-payout (seller-facing trust metric).

**Trust & quality**
- Claim rate (claims / completed orders).
- Claim upheld rate.
- Payout failure rate (treat as P0).
- Seller approval funnel (applied → approved).

**Growth**
- Active buyers / active sellers.
- Repeat purchase & repeat-listing rates.
- GMV and net revenue (commission realized).

Target for "is the loop working": a small but steady set of **clean end-to-end
completions** with low claim and zero payout-failure, before scaling supply.

---

## 18. Open Questions / Risks

1. **Secure hold vs regulation (highest).** Holding buyer funds and releasing on a
   trigger may constitute regulated activity. Confirm with Whish/MontyPay whether they
   offer a conditional hold/sub-account primitive, or whether the platform is acting as
   the holder (and what license/structure that requires).
2. **Payout reliability.** Can we reliably pay sellers in fresh USD, fast, every time?
   This is the make-or-break operational promise.
3. **Take rate & who pays.** Final commission %, and seller vs buyer split, must leave
   sellers netting more than free Instagram.
4. **Authentication threshold.** What value triggers the extra authentication step, and
   what's the actual mechanism (in-house, partner, photo protocol)?
5. **Chain-of-custody enforceability.** Is unboxing video + shipping proof enough to
   adjudicate disputes fairly and resist abuse?
6. **Delivery/courier partner.** Who delivers, and can they act as neutral witness?
7. **Seller cold-start.** Will the launch cohort actually list and pay commission?
   Validate with ~5 real sellers before heavy build.
8. **Listing volume vs review capacity.** Manual review must scale with supply; define
   the high-value threshold that forces pre-publish review.
9. **Double-sale prevention.** Locking on acceptance/payment must be airtight since
   inventory lives with sellers across channels.

---

# 19. Screen-by-Screen Breakdown

### Buyer

| Screen | Purpose | Key elements | Primary action |
|--------|---------|--------------|----------------|
| Home / discovery | Surface inventory | Featured/new listings, categories, search entry | Tap listing / search |
| Search + filters | Find items | Filters: category, brand, size, price; results grid | Apply filters → open listing |
| Listing detail | Build trust + convert | Photos, condition grade, brand/size/details, seller badge + rating, authenticity notes | **Make Offer** |
| Make-offer modal | Submit offer | USD amount input, optional message, terms reminder | Submit offer |
| Offer thread | Negotiate | Offer history, counter values, status, notifications | Counter / accept / withdraw |
| Payment | Pay after acceptance | Order summary, amount, partner payment widget, payment window timer | Pay |
| Order tracking | Track + confirm | Status timeline (held→shipped→delivered), tracking, inspection countdown | Confirm delivery / open claim |
| Claim / report | Exception path | Reason picker (3 only), evidence upload (photos, unboxing video), notes | Submit claim |

### Seller

| Screen | Purpose | Key elements | Primary action |
|--------|---------|--------------|----------------|
| Seller application | Onboard | Business info, contact, agreement to authenticity/ownership terms | Submit application |
| KYC verification | Verify identity | ID/business doc upload, status indicator | Upload & submit |
| Seller dashboard | Operate | Listings summary, open offers, orders to ship, payout snapshot | Navigate |
| Create / edit listing | Add supply | **Mandatory** photos, condition grade, brand, size, category, description, authenticity details, price USD | Publish / submit for review |
| Offers inbox | Handle demand | Incoming offers, amounts, buyer, status; offer detail | Accept / counter / reject |
| Shipping confirmation | Fulfill + prove | Courier/tracking input, pickup/sealed-package proof upload | Confirm shipped |
| Payout status | Get paid | Per-order payout state, amounts net of fees, history | View |

### Admin

| Screen | Purpose | Key elements | Primary action |
|--------|---------|--------------|----------------|
| Seller approval queue | Gate supply | Pending applications, KYC docs, history | Approve / reject / assign badge |
| Listing review queue | Quality control | High-value/flagged listings, media, fields | Approve / request edit / remove |
| Claims / disputes | Adjudicate | Claim details, all evidence, listing, shipping proof, auth record | Uphold (refund) / reject (release) |
| Payout release | Move money | Orders eligible for release, payout status, failures | Confirm / trigger / retry payout |
| Orders/offers monitor + audit log | Oversight | Live transaction states, admin action history | Inspect |

---

# 20. Suggested MVP Development Sequence (Phased)

**Phase 0 — Foundations**
Auth + roles (buyer/seller/admin), data model, image storage, base admin shell,
environments. *Outcome: skeleton you can build on.*

**Phase 1 — Supply**
Seller application → KYC → admin approval → badge; listing creation with mandatory
fields; admin listing review queue. *Outcome: verified sellers can publish listings.*

**Phase 2 — Demand**
Home/discovery, search + filters, listing detail. *Outcome: buyers can browse real
inventory.*

**Phase 3 — Transaction core (the hard part)**
Make-offer → offer thread (accept/counter/reject) → payment after acceptance →
platform-held funds → order created + item locked. *Outcome: money can change hands
safely. Build the order state machine here, carefully.*

**Phase 4 — Fulfillment & payout**
Shipping confirmation + proof upload, order tracking, delivery confirmation, inspection
window, payout release dashboard. *Outcome: full loop closes and sellers get paid.*

**Phase 5 — Trust & disputes**
Claims flow (3 reasons), evidence capture (unboxing/shipping proof), admin
claims dashboard, ratings, badge standing, audit log. *Outcome: exceptions are handled
and abuse is contained.*

**Phase 6 — Instrumentation & launch hardening**
Launch metrics (§17), payout-failure alerting, double-sale locks, edge-case QA,
soft launch with a handful of vetted sellers. *Outcome: ready for a controlled launch.*

> Build order rationale: supply before demand (empty marketplaces fail), transaction
> before trust-polish (the loop must close), disputes last (they're the exception, not
> the path). Resist building Phase 5+ niceties before Phase 3–4 works end to end.

---

# 21. Short Technical Recommendation (Fast MVP Build)

Optimize for **speed, small team, and humans-in-the-loop ops** — not scale.

**Recommended stack**
- **Frontend:** Next.js (React) as a responsive web app / PWA. Mobile-first layout;
  skip native apps for v1 (Lebanon is mobile-heavy, but a PWA ships far faster).
- **Backend + DB + auth + storage:** a batteries-included platform like **Supabase**
  (Postgres + auth + file storage + row-level security) or Firebase. This collapses
  weeks of plumbing. Postgres is the right call given the relational order/offer/claim
  model.
- **Payments:** integrate the local partner's gateway (**Whish Pay / MontyPay**) for
  collection; implement the **hold-and-release** as platform-side order state + a
  controlled payout step. Do not assume the gateway gives you escrow — confirm early
  (§18.1).
- **Admin back-office:** don't hand-build it. Use an internal-tool framework
  (**Retool / Refine**) or a thin admin section in the same Next.js app, wired to the
  same DB. This is where manual ops live.
- **Media:** Supabase storage or Cloudinary for listing photos / unboxing videos.
- **Notifications:** WhatsApp / SMS for offer and order events (Lebanon lives on
  WhatsApp) — even a manual/semi-automated start is fine.

**Principle:** every "should we automate this?" question in v1 answers to **"no — give
admin a button."** Automate only after the manual version proves the rule.

---

# 22. What Your Developer Should Ask Claude Next (in order)

Send these as **separate** follow-up prompts, one at a time, so each output is focused:

1. *"Turn this PRD into a screen-by-screen UX sitemap."*
2. *"Turn the sitemap into user stories with acceptance criteria."*
3. *"Turn the user stories into a phased development backlog."*
4. *"List the simplest backend schema for this MVP."* (entities, fields, relations, the
   order state machine)
5. *"Suggest the leanest tech stack for fast implementation."* (confirm/adjust §21)
6. *"Write admin rules for seller verification, offer handling, claims, and payout
   release."* (the operating manual for the humans-in-the-loop)

Plus one I'd add before writing code:
7. *"Draft the exact order/offer/claim state machine with every status, transition, and
   guard condition."* — this is the spec the whole transaction core is built from.

---

*End of PRD v1.*
