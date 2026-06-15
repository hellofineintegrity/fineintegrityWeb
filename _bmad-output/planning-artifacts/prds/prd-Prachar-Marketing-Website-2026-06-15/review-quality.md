# PRD Quality Review: Prachar Marketing Website
**Reviewed:** 2026-06-15  
**PRD version:** draft (2026-06-15)  
**Reviewer role:** Senior Product Manager

---

## Gate Verdict

**NEEDS WORK — minor gaps; not a blocker to starting design, but must be resolved before developer handoff.**

The PRD is well above average for a v1 marketing site document. Structure is professional, FRs are largely testable, and the decision trail is transparent. However, five categories of gaps — detailed below — would create ambiguity or rework risk during implementation if left unresolved.

---

## Dimension-by-Dimension Assessment

### 1. Decision-Readiness

**Rating: High (with caveats)**

A competent web developer and UX designer could start work immediately on: page structure, section order, responsive layout breakpoints, UTM scheme, analytics event taxonomy, hosting platform, image constraints, and performance targets. The glossary (§3) prevents most terminology disputes.

**Caveats that create ambiguity:**

- The PRD refers the reader to "product PRD Appendix C" for pricing/volumes (FR-12) but does not include those figures inline. If the product PRD is not co-located or its Appendix C changes, pricing values on the marketing site could fall out of sync with no detection mechanism. At minimum, the current pricing/volume figures should be reproduced verbatim in this PRD (or a formal dependency note added stating that FR-12 is blocked until product PRD Appendix C is frozen).
- The hero visual (FR-6) is specified as "a static image of a sample Prachar-generated campaign" but there is no requirement for who produces this asset, by when, or what the acceptance criteria are beyond file format and weight. This is a common critical-path blocker on marketing sites — visual design often depends on a real campaign screenshot that doesn't exist until the product is functional. The PRD should either add a content-readiness assumption or an explicit dependency on the product being far enough along to generate a sample campaign.
- FR-4 specifies headline copy constraints (≤10 words) and font size minimums but defers final copy to "a content/UX decision." That is acceptable, but there is no stated process or owner for finalising copy before implementation. The developer needs final copy before build, not after. An assumption or open question should capture this dependency.

---

### 2. Substance — Are FRs Testable? Are Consequences Specific?

**Rating: High**

Most FRs are among the best-written in this document class:

- Pixel-level specificity (44×44px touch targets, 375px minimum viewport, 64px max header height, 150KB hero image, 200KB OG image).
- GA4 event names, parameter names, and expected values are exact (FR-24: `cta_click`, `cta_location`, `plan_tier`).
- UTM matrix in Appendix C is complete and unambiguous.
- Performance targets specify the measurement method (P75 mobile, simulated 4G, Google Search Console).
- Redirect chains are specified (HTTP → HTTPS, www → non-www, both 301).

**Issues:**

- **FR-13 (Pricing FAQ accordion):** "No API call or JavaScript framework required for accordion behaviour (CSS-only or minimal JS acceptable)" is a guidance note, not a testable consequence. The consequence should state what is being tested (e.g., "Accordion expand/collapse works without loading any JavaScript larger than 5KB" or simply remove the implementation guidance and leave the behaviour consequence).
- **FR-11 (Social Proof):** The consequence "at least one proof element is present" is testable, but "renders correctly on mobile and desktop" is not — it needs a definition of "correctly" (e.g., no overflow, font size ≥14px, image does not clip). This is a minor gap.
- **FR-25 (Scroll depth):** "Fire and appear in GA4 reports within 24 hours" — the 24-hour latency is a GA4 platform characteristic, not something the developer can control or test. The testable consequence should be "Scroll depth events fire and appear in GA4 DebugView during a live test session" (consistent with how FR-24 is specified).
- **FR-7 (How It Works):** "Each step has a distinct icon or illustration" is present, but there is no specification for who creates these icons/illustrations, or whether they are sourced from an icon library (if so, which?), commissioned, or reused from the product. This is a content-dependency gap that blocks visual design.

---

### 3. Scope Clarity

**Rating: High**

The non-goals (§5) and out-of-scope list (§6.2) are consistent with each other and with the FRs. No FR reaches into out-of-scope territory. The site boundary (fineintegrity.com vs. app.fineintegrity.com vs. businessname.fineintegrity.com) is clearly defined in §3 and maintained throughout.

**One inconsistency found:**

- §4.8 (Contact) states the contact link appears in "both the header navigation and the footer." FR-1 (sticky header) lists navigation links as "Features, Pricing, Contact" — this is consistent. However, FR-15 consequences test for the link "in both the header navigation and the footer" without checking the mobile nav drawer. Given that FR-2 specifies the mobile drawer "contains the navigation links and the 'Start Free Trial' CTA," the contact link should also appear in the mobile drawer by implication. This should be made explicit in FR-15's consequences to avoid the developer excluding it from the drawer.

- The Appendix B page table includes `/* (all other paths) → 404 or redirect to /` but does not specify which behaviour is required. This matters for SEO (a redirect to / creates a soft-404 risk in Google Search Console; a real 404 page is correct for static sites). The PRD should commit to one approach.

---

### 4. Strategic Coherence

**Rating: High**

The success metrics (§7) trace cleanly to FRs:

- SM-1 (sign-up conversion) → FR-5, FR-8, FR-12, FR-14, FR-24, FR-26. Correct.
- SM-2 (time-to-first-CTA-click) → FR-4, FR-5, FR-7. Correct.
- SM-6 (Core Web Vitals) → Appendix A, FR-6. Correct.
- Counter-metrics (SM-C1, SM-C2) are rare and valuable — they prevent common scope creep patterns.

**Issues:**

- **SM-1 measurement gap:** The metric is "click a CTA AND complete registration." The click is measurable via FR-24 (cta_click event on the marketing site). But completed registration happens on app.fineintegrity.com/register — the marketing site PRD does not own that measurement. FR-26 covers UTM passthrough, and the assumption covers GA4 cross-domain measurement, but the open question (§8.3) notes ownership is unresolved. If that open question is not resolved before launch, SM-1 is unmeasurable and the primary success metric fails from day one. This risk is identified in the PRD but should be elevated — it is a launch blocker, not just an open question.

- **SM-3 (Android bounce rate ≤50%):** There is no FR that directly targets bounce reduction beyond general performance and headline clarity. The bounce rate target is reasonable but is not driven by any specific requirement. If bounce rate underperforms, there is no actionable lever in the PRD. Consider whether a "scroll past hero" event (a variant of FR-25) would give more diagnostic value than raw bounce rate.

- **Open questions (§8):** The seven open questions cover the real risks well (social proof permission, legal content, GA4 ownership, payment gateway naming, domain control, highlighted tier, app subdomain readiness). These are exactly the right questions. No critical risk appears missing from this list.

---

### 5. What's Missing

**Rating: Medium — five gaps identified**

**Gap 1: 404 page requirement (minor blocker)**  
Appendix B notes "404 or redirect to /" for unknown paths but does not specify which. A proper static 404 page (`/404` or equivalent) with site navigation, a brief message, and a CTA back to the homepage is standard practice and avoids soft-404 SEO penalties. This page is currently neither in scope nor out of scope — it falls through the cracks.

**Gap 2: Cookie / consent banner (deferred but underspecified)**  
§5 Non-Goals defers "full PDPB/GDPR cookie consent" to post-launch. But GA4 tracking (FR-23 through FR-26) is active from day one and collects visitor data. The PRD does not specify what, if anything, is shown to visitors at launch regarding data collection — even a minimal "we use analytics" notice. Given that India's DPDP Act (Digital Personal Data Protection Act, 2023) imposes consent requirements for personal data processing, and GA4 collects IP addresses and identifiers, the decision to ship with no consent mechanism at all should be made explicitly by Nilesh, documented as an assumption with acknowledged risk, not simply deferred. At minimum this should become an open question.

**Gap 3: Favicon and brand asset requirements**  
There is no requirement for a favicon (16×16, 32×32, Apple Touch Icon at 180×180, Web App Manifest icon). These are table stakes for a launch-ready site and affect how the site appears when bookmarked, shared on iOS home screens, or opened in browser tabs. Given the emphasis on the WhatsApp-forwarded-link use case (UJ-2), the og:image requirement (FR-19) is covered, but the bookmark/homescreen icon path is not.

**Gap 4: Font and typography specification**  
Appendix A NFR specifies "System font stack or preloaded web font; no FOUT on mobile" but does not name the font(s). If Prachar has a brand font (or if Fine Integrity has decided on one), this should be specified here so that the developer and UX designer use the same typeface. If the font is genuinely undecided, an assumption tag should flag it. Font choice has a direct impact on perceived brand quality and LCP (web fonts can delay render).

**Gap 5: Content freeze / handoff process**  
The PRD defines copy constraints (word limits, headline length) and outcome-language tone, but does not specify when final copy must be ready for the developer to implement, or who owns final copy approval (Nilesh? a copywriter?). For a static build with no CMS, late-stage copy changes require code edits. The document would benefit from a single sentence or assumption in §4.2/§4.3/§4.4 stating that final copy must be delivered to the developer by a specific milestone (e.g., 30 days before the go-live target of October 1, 2026).

---

## Summary of Required Actions Before Developer Handoff

| Priority | Item | Action |
|---|---|---|
| Blocker | SM-1 measurement depends on GA4 cross-domain ownership (§8.3) | Resolve §8.3 and document which team owns the configuration before handoff |
| Blocker | Pricing values are deferred to product PRD Appendix C | Reproduce pricing/volume figures inline in FR-12, or freeze the product PRD before this site enters development |
| High | 404 page | Add FR or explicit out-of-scope decision for static 404 page |
| High | Cookie/consent at launch | Elevate §5 deferral to an open question; Nilesh must make an explicit decision |
| High | Hero visual production path | Add assumption or open question naming who produces the hero image and by when |
| Medium | Favicon/brand icon assets | Add a brief FR or NFR row in Appendix A |
| Medium | Font specification | Add font name(s) to Appendix A NFR or tag as assumption |
| Medium | FR-25 test condition | Change "within 24 hours" to "in GA4 DebugView during live test session" |
| Low | FR-15 mobile drawer | Add mobile nav drawer to FR-15 consequences |
| Low | Appendix B 404 path | Commit to "static 404 page" (recommended) vs. "redirect to /" |
| Low | Content freeze date | Add one assumption or note in §4.2 naming copy handoff owner and date |

---

## What the PRD Does Well (Do Not Change)

- Single-scroll homepage architecture with section order driven by conversion logic is clearly and correctly specified.
- UTM matrix (Appendix C) is complete, consistent across all FRs, and machine-readable.
- GA4 event taxonomy (FR-24) is precise enough to hand directly to a developer without further spec.
- User journeys (UJ-1, UJ-2) are written at the right level of detail — concrete enough to drive design decisions, specific enough to expose the 4G / WhatsApp-forward edge cases.
- Non-goals (§5) are unusually disciplined — explicitly naming Hindi website, live chat, CMS, and A/B testing prevents scope creep.
- Counter-metrics (SM-C1, SM-C2) are a genuine best practice rarely seen in v1 PRDs.
- Assumptions Index (§9) is complete and cross-referenced; no inline assumption tag is orphaned.
- The open questions are the right questions and are sequenced by downstream dependency (social proof → legal → analytics → payment → domain → pricing → product readiness).
