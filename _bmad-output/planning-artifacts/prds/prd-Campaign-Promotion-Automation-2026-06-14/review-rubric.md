# PRD Quality Review — Campaign Promotion Automation

---

## Overall verdict

This is a well-above-average PRD for an Indian SMB SaaS pilot. The functional requirements are largely testable, the glossary is rigorous, the counter-metrics are a genuine rarity, and the UJs feature named protagonists with real context. The main weaknesses are: (1) several NFRs still carry implicit gaps (scalability limit unstated, WhatsApp opt-in source not specified for email channel, data-deletion SLA absent); (2) two Open Questions are duplicates of each other (OQ-1 and OQ-7 are both re-opt-in), and some questions have already been answered in the Assumptions Index, obscuring which are genuinely unresolved; (3) the approval workflow has a gap — there is no consequence for what happens when a WhatsApp Template is Meta-rejected after the Promotion is in Approved state (the flow described in FR-34 is incomplete); and (4) scope-for-pilot vs scope-for-product is fused in §6, making it hard for an architect or PM to know which requirements are firm commitments vs. nice-to-haves for the 3-business pilot.

---

## 1. Decision-readiness — adequate

The PRD surfaces most of the hard calls honestly. The pricing model (Appendix C) is unusually transparent, including the cost basis and margin arithmetic. The Non-Goals list (§5) is doing real work — each exclusion carries a rationale. The pilot scope is named (3 specific businesses, Navratri–Diwali window), which is exactly the kind of commitment a decision-maker needs.

However, two structural issues undercut decision-readiness:

**Open Questions contain resolved and duplicate items.** OQ-1 and OQ-7 ask the same question ("what is the re-consent mechanism when a customer opts out via STOP?") and the Assumptions Index (§9) already answers it provisionally ("Re-opt-in flow via WhatsApp reply 'START' is in scope for v1"). Neither OQ-1 nor OQ-7 cite that assumption or flag its contradiction. A decision-maker scanning §8 cannot tell whether this is resolved or not.

OQ-6 ("Is manual invoicing acceptable for the 3-business pilot?") is also answered in §9 with "manual invoicing or Razorpay payment link after free trial" — yet §8 lists it as unresolved. This pattern (assumption resolves the question, but question doesn't cross-reference the assumption) is a consistent reliability problem across the document.

**Trade-off between self-registration and pilot control is not surfaced.** §4.1 states "Business Admin — not System Admin — performs all registration steps." But §6 names exactly 3 pilot businesses. Is the platform actually open for any business to self-register during the pilot, or is registration gateable to invited businesses only? This is a real product decision — if a fourth business discovers and registers during the pilot window, is that desired? There is no NOTE FOR PM or ASSUMPTION flagging this tension.

### Findings
- **high** Duplicate open questions OQ-1 and OQ-7 (§8) — Both ask the re-opt-in question already partially addressed in §9. *Fix:* Collapse into one question, cross-reference the assumption, and explicitly state what residual uncertainty remains (specifically Meta's policy restriction on contacting opted-out users, which OQ-7 identifies but OQ-1 does not).
- **medium** OQ-6 appears unresolved but is answered in §9 (§8 vs §9) — Creates confusion about which questions are genuinely open. *Fix:* Remove OQ-6 from §8 or reframe it as "Confirm timeline: when does manual invoicing need to be replaced?" and reference the assumption.
- **medium** Self-registration open-for-all vs. pilot-gated not decided (§4.1, §6.1) — *Fix:* Add `[ASSUMPTION: Self-registration is open to any business; the 3-business pilot list is illustrative, not gated]` or add a NOTE FOR PM flagging this decision.

---

## 2. Substance over theater — strong

This PRD is substantially free of theater. Vision (§1) cites specific competitors (Zoko, Interakt, WATI) with specific differentiators (Indian festival calendar, AI generation, cross-channel analytics). The "Why Now" paragraph names a concrete product window (Navratri–Diwali 2026) and a specific cost enabler (Claude Haiku pricing). These are earned claims, not generic aspiration.

The industry list (FR-46) is seeded with 25 named categories — a measurable commitment. The layout library commitment (FR-17: "minimum of 50 templates at launch, covering all 25 Industries and at least the 10 most common festival/seasonal occasions") is testable and real.

One area of mild theater: the "Why Now" sentence "Competition is intensifying for Indian SMBs" is generic and could appear in any B2B SaaS PRD. The rest of the paragraph earns it, but the opening clause is filler.

The Appendix D infrastructure table is a decision record, not a spec — it names the actual Azure tier (App Service B1, PostgreSQL B1ms) and the rationale for each. This is excellent for a PRD aimed at a solo-operator product owner.

### Findings
- **low** "Competition is intensifying" opener in §1 Why Now — Swappable into any PRD. *Fix:* Cut the first sentence; open with the named competitor comparison.

---

## 3. Strategic coherence — strong

The thesis is clear and consistent throughout: *Indian SMBs lack professional, compliant, culturally-relevant promotional tooling that a non-marketer can operate end-to-end.* Every feature serves this arc:

- AI generation with festival calendar → removes design and copywriting barrier
- WhatsApp-first delivery → meets Indian SMB customers where they are
- Approval workflow → preserves brand control without IT overhead
- Pre-built layouts (not generative images) → zero-cost professional output
- ₹3/message pricing vs. manual alternative → accessible to SMBs

Success metrics map cleanly to the thesis. SM-1 (onboarding completion) validates the "any business exec can operate this" claim. SM-3 (70% of promotions need ≤2 iterations) validates AI quality. SM-7 (free-trial conversion) validates product-market fit. Counter-metrics (SM-C1 through SM-C3) are genuinely protective — opt-out rate, revision overuse, and Meta rejection rate are the three failure modes that would sink the pilot specifically.

One coherence gap: the Viewer role (§2 and Glossary) has no success metric and no UJ. The thesis says Viewers ("Business Owners") need to "understand whether the investment in promotions is driving results." No SM measures whether they actually access analytics or find them useful. This is a low-severity gap for a pilot, but means the Viewer role is not validated.

### Findings
- **medium** Viewer role has no success metric or UJ (§2.1, §7) — Business Owner is named as a target user but success is unmeasured. *Fix:* Add SM-8: "At least 1 Viewer per pilot business accesses analytics dashboard within 2 weeks of first campaign completion." Or explicitly note that Viewer satisfaction is deferred to v2 measurement.
- **low** No counter-metric for onboarding drop-off — If Meta WhatsApp Business verification is the blocker (OQ-3), the pilot could fail before a single campaign is sent. *Fix:* Add SM-C4: "Meta Business verification completion rate must be 100% for pilot businesses before Navratri launch."

---

## 4. Done-ness clarity — strong

The "Consequences (testable)" pattern applied to every FR is the strongest structural feature of this document. Most FRs have crisp, binary acceptance criteria. Sampling:

- FR-2: "active and routed to the correct Business tenant within 60 seconds" — testable
- FR-7: "creates or updates the corresponding Customer Profile within 5 minutes" — testable
- FR-12: "STOP reply on WhatsApp is processed within 60 seconds of receipt" — testable
- FR-37: "throttled to stay within the Branch's Meta free tier (1,000 marketing conversations/month per number)" — testable

There are three "done-ness" gaps worth naming:

**FR-34 has an incomplete consequence path.** When Meta rejects a template after it has been in the "Pending Meta Approval" state, the FR states "Promotion is returned to Approved state for editing." But the Approval Workflow state machine (§4.5) has no "Approved but awaiting Meta rejection remediation" state. An engineer needs to know: can the Executive re-edit directly, or does it go back to Draft and require re-approval? UJ-2 edge case describes the rejection flow but implies the Promotion "returns to editing state" without specifying who can edit (Executive? Admin?) and whether re-approval is needed.

**FR-26 auto-archive has an ambiguous consequence.** "Campaigns whose end date has passed are automatically moved to Archived state, regardless of their current state (including Draft or Submitted)." If a Submitted Promotion is auto-archived, its in-progress approval is cancelled. This is the right behavior but the consequence listed says only "Admin is notified" — it does not say the notification explains why (end-date expiry), nor does it say the Executive is notified. The "done" test for this edge case is incomplete.

**FR-10 auto-segment rules are undefined at the group level.** The consequence states "auto-segment Groups update dynamically as Customer Profiles change." But the FR description says rules are "e.g., 'all Gold tier customers in Pune'" — and the only filterable Customer Profile fields documented (FR-9) are: loyalty tier and city/branch tag. Whether an engineer can build *any* attribute-based rule or only these two is left open. The "done" test for auto-segment is incomplete.

### Findings
- **high** FR-34 incomplete state machine for Meta rejection remediation (§4.6) — After Meta rejects, who can edit and must re-approval happen? *Fix:* Add consequence: "Meta-rejected WhatsApp Promotion reverts to Draft state; Executive must re-edit and resubmit; Admin must re-approve; Meta re-submission is triggered on second approval."
- **medium** FR-26 auto-archive notification gap (§4.4) — Executive is not notified; notification reason ("end-date expiry") is unspecified. *Fix:* Add consequence row: "Executive whose Submitted Promotion is auto-archived receives an in-app + email notification with reason 'Campaign end date passed.'"
- **medium** FR-10 auto-segment rule scope unclear (§4.2) — "e.g." example implies extensible rules; only 2 profile attributes are suitable. *Fix:* List the exact fields on which auto-segment rules can be built, or add `[ASSUMPTION: Auto-segment supports filtering on: Loyalty Tier, City/Branch Tag, and WhatsApp opt-in status only for v1.]`

---

## 5. Scope honesty — adequate

The Non-Goals section (§5) is doing substantive work: each exclusion names the specific reason it was cut (e.g., SMS → "TRAI DLT registration complexity"; AI images → "zero per-image cost" rationale for the alternative). This is well above average.

The two NOTE FOR PM flags in §6.2 (SMS and regional languages) are correctly placed and actionable.

However, three honest scope gaps are not acknowledged:

**Email opt-in is unaddressed.** FR-9 defines `WhatsApp opt-in status` and `Email opt-out status` but never defines how email opt-in is obtained. FR-12 covers email unsubscribe. FR-37 mentions "only customers with whatsapp_opt_in = true... receive the message" — but there is no equivalent FR-XX for email. Under Indian data protection law and the PRD's own TRAI compliance section, sending promotional email to a customer without recorded consent is non-compliant. The absence of `email_opt_in` as a Customer Profile field and of an enforcement check at email send time (analogous to the WhatsApp check in FR-37) is a real compliance gap, not a scope cut, and it is not explicitly non-goaled.

**Unsplash rate limit is an Open Question (OQ-5) but has no ASSUMPTION tag and no fallback specified in FR-17.** FR-17 states "if the Unsplash API is unavailable, the Platform falls back to the layout's default placeholder imagery." This is a service-down fallback, not a rate-limit fallback. At 50 requests/hour, even moderate concurrent usage hits the ceiling. This is a scope decision (pay for Unsplash Pro? Cache images? Use Azure CDN?) that has no `[ASSUMPTION]` tag and no `NOTE FOR PM` in FR-17.

**Campaign modification after scheduling is partially addressed.** FR-24 says "Admin can reschedule or cancel a scheduled Campaign at any time before the send time." But what happens if the Admin modifies the Customer Group after the Campaign is scheduled — does the delivery list freeze at schedule time or resolve at send time? This edge case is not specified.

### Findings
- **high** Email opt-in not defined (§4.2 FR-9, §4.6 FR-36, Appendix B) — `email_opt_in` is absent from Customer Profile; email send-time enforcement check is absent. Compliance gap under Indian IT Act. *Fix:* Add `email_opt_in` and `email_opt_in_date` to FR-9. Add consequence to FR-36: "Customers with `email_opt_out = true` or no recorded `email_opt_in` are excluded from email campaigns." Or explicitly non-goal email opt-in enforcement with a NOTE FOR PM.
- **medium** Unsplash rate-limit fallback not specified in FR-17 (§4.3) — OQ-5 identifies the risk but no assumption or decision is recorded in the FR. *Fix:* Add `[ASSUMPTION: Unsplash API responses for common festival/industry combinations will be cached server-side for 24 hours to stay within the 50 req/hour free tier limit. Pro API upgrade is a v1.1 consideration.]` to FR-17.
- **low** Customer Group list resolution time not defined (§4.4 FR-24) — Campaign delivery list: frozen at schedule-creation time or recomputed at send time? *Fix:* Add consequence: "The recipient list is computed from Customer Group membership at send time (not at schedule creation time); opted-out customers added between scheduling and sending are excluded."

---

## 6. Downstream usability — strong

**Glossary:** §3 defines all 16 key terms with precision. The Promotion vs. Template vs. Campaign distinction is a common source of confusion in campaign-management tools, and this PRD resolves it cleanly. The WhatsApp Template (Meta-approved format) vs. Template (reusable Promotion layout) distinction is correctly named and differentiated in the glossary. No glossary drift detected across the document.

**FR ID continuity:** FR-1 through FR-48 are present and contiguous. No gaps detected. IDs are stable (they are not reorganized by section — they run sequentially across all 8 feature sections), which means cross-references in the Assumptions Index and SM mappings resolve cleanly.

**Cross-references:** SM-1 through SM-7 cite FR IDs explicitly. SM-1 ("Validates FR-1 through FR-7") is correct. SM-2 and SM-3 cite specific FRs correctly. Appendix B compliance table references FR-11 and FR-37, which are the correct FRs. No broken cross-references found.

**UJ protagonist naming:** All three UJs use named protagonists (Rahul, Priya, Vikram's customer). The entry state specifies authentication context and subdomain. UJ-2 introduces a second named actor (Meena, the executive) in the path, which correctly shows the notification loop.

**One downstream usability gap:** The Approval Workflow state machine is described in prose (§4.5 description: "Draft → Submitted → Approved or Rejected → Published") but not as a formal state diagram or table. Given that FR-34 (Meta rejection) and FR-26 (auto-archive) create branching states not captured in the linear description, an engineer building the state machine will need to infer states from across multiple FRs. A state transition table would make this unambiguous.

**ID continuity for SMs:** SM-1 through SM-7 and SM-C1 through SM-C3 are present and contiguous. No gaps.

### Findings
- **medium** Promotion state machine not consolidated (§4.5 description, FR-34, FR-26) — States are scattered across three sections. An engineer must infer the complete FSM from prose. *Fix:* Add a state transition table to §4.5 header listing all states (Draft, Submitted, Approved, Rejected, Archived, and any new "Meta Rejected" branch) and the transitions between them with the actor who triggers each.
- **low** "shreegaments.fineintegrity.com" typo in UJ-2 (§2.3) — Should be "shreegarments" or the correct business subdomain. *Fix:* Correct the subdomain to match the business name or the actual pilot business subdomain.

---

## 7. Shape fit — adequate

**Right level for the product type?** This is a B2B multi-tenant SaaS for Indian SMBs, pilot launch, with a named 3-business cohort. The formalization level is calibrated correctly in most areas:

- FR depth is appropriate for a product with a single developer/owner — consequences are testable without over-specifying implementation
- Appendix D infrastructure choices are right for this document (a solo-operator product owner who makes infrastructure decisions personally)
- Pricing in Appendix C belongs in this PRD because it's a product decision, not just a monetization detail

**Areas where shape fit is off:**

**Multi-tenancy isolation is under-specified for a SaaS product.** The glossary defines Business → Branch → Customer hierarchy, but no FR specifies tenant isolation guarantees. An engineer building on this PRD has no documented requirement that Business A cannot access Business B's data. For a multi-tenant B2B SaaS, this is not an implementation detail — it is a product requirement. There is no NFR in Appendix A for tenant data isolation.

**The "50+ Business tenants without rewrite" scalability NFR (Appendix A) is too vague for a SaaS product.** 50 tenants is essentially no scalability requirement — it rules out only a static-site approach. For a product targeting Indian SMBs, the real question is: what is the concurrent-user or messages-per-day capacity? Without this, an architect cannot make meaningful sizing decisions. Compare: the AI generation SLA (< 15 seconds P95) is well-specified; the concurrency requirement for delivery is absent.

**Billing model shape mismatch:** The per-message pricing (₹3/WhatsApp, ₹0.50/email) with no stated metering mechanism is a gap for a v1 product. The assumption that billing is handled externally (§9) defers this cleanly — but the PRD does not specify what data the platform must log to enable accurate per-send billing (a metering/ledger requirement). For a product billing per message, the metering requirement is as critical as the delivery requirement.

### Findings
- **high** Tenant data isolation not required anywhere in the PRD (§4.1, Appendix A) — Multi-tenant SaaS without isolation NFR is an architectural risk. *Fix:* Add to Appendix A NFR table: "Tenant isolation: Business A's data (customers, promotions, campaigns, analytics) must be inaccessible to Business B's users at all application layers. Violated isolation is a critical security defect."
- **medium** Scalability NFR "50+ tenants without rewrite" is not a real ceiling (Appendix A) — *Fix:* Replace with meaningful concurrent-load target: e.g., "System supports 500 concurrent authenticated users and 10,000 WhatsApp messages dispatched per hour across all tenants without degradation."
- **medium** Per-message billing has no metering requirement (§6.1, Appendix C) — The platform bills per send but no FR specifies a billing-event log. *Fix:* Add FR-49: "For each message dispatched, the Platform records a billing event: Business ID, Branch ID, Channel, Customer ID, send timestamp, and delivery status. Billing events are immutable and retained for 12 months."

---

## Mechanical notes

**Glossary drift:** None detected. All terms defined in §3 are used consistently. "WhatsApp Template" (Meta-approved format, §3) and "Template" (reusable Promotion layout, §3) are consistently distinguished throughout, including in FR-34, FR-37, and Appendix B.

**FR ID continuity:** FR-1 through FR-48 are contiguous with no gaps. FR-49 is the next available ID (used in the fix recommendation above).

**Assumptions Index roundtrip:** 5 assumptions in §9; all 5 have matching `[ASSUMPTION]` tags in the body. However, the tag placement in FR-11 cites "Re-opt-in flow via WhatsApp reply 'START' is in scope for v1" while OQ-1 asks whether this is sufficient — and OQ-7 asks essentially the same question again. The roundtrip from tag → assumption → open question is broken by the duplication.

**UJ protagonist naming:** Rahul (UJ-1), Priya and Meena (UJ-2), unnamed customer and "Fresh Basket Admin" (UJ-3). UJ-3 uses two anonymous roles where names would improve traceability; low severity.

**Subdomain typo in UJ-2:** `shreegaments.fineintegrity.com` should be `shreegarments.fineintegrity.com` (or the actual business name).

**"4 revisions" inconsistency:** FR-18 says "maximum 4 revision iterations per generation session" and FR-31 says "Resubmission uses the same 4-iteration AI revision budget (iterations already used are counted)." This means if a Promotion is rejected and returned to Draft, the Executive may have 0 revisions remaining. This is a deliberate design choice but there is no UJ edge case or NOTE FOR PM acknowledging it. An Executive who used all 4 revisions before submission and then receives a rejection has no AI revision path — they must start a new generation session (FR-18 mentions this). This needs to be explicit in FR-31 consequences.
