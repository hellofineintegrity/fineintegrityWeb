---
title: "Brief-to-PRD Reconciliation — Prachar Marketing Website"
created: 2026-06-15
source_brief: "_bmad-output/planning-artifacts/briefs/brief-Campaign Promotion Automation-2026-06-15/brief.md"
source_prd: "_bmad-output/planning-artifacts/prds/prd-Prachar-Marketing-Website-2026-06-15/prd.md"
---

# Brief-to-PRD Reconciliation

## Status: 5 gaps identified

---

### Gap 1 — Urgency / Seasonal Calendar not in Features copy requirement

**Brief says:** Urgency is the third conversion hurdle. The website must connect Prachar's value to specific calendar moments (Navratri, Diwali, Eid, monsoon) the visitor already has circled. This is positioned as a distinct psychological lever alongside Comprehension and Trust.

**PRD handles it:** Only partially. Navratri/Diwali appear in UJ-2 (Meena's journey) and in the How It Works section narrative. The Features section (FR-9) names "Indian Festival Calendar" as a feature card, which addresses comprehension — but no functional requirement explicitly demands that the hero or How It Works copy create urgency by referencing the visitor's current/upcoming season. The brief frames urgency as equally important to comprehension and trust; the PRD treats it as incidental copy detail.

**Recommendation:** Add a functional requirement (or a testable consequence to FR-4 or FR-7) that the hero sub-headline or How It Works section reference at least one named, imminent festival season — not just a generic "Indian festivals" phrase.

---

### Gap 2 — Differentiation section / Competitor framing not required anywhere

**Brief says:** Three specific differentiators must be communicated — and specifically, *not as a generic feature list*. The brief names the direct competitors (Zoko, Interakt, WATI, manual WhatsApp Business) and requires the site to address why Prachar is different from those alternatives.

**PRD handles it:** The PRD's five feature cards (FR-9) cover the same capabilities, but no functional requirement demands competitor-differentiated framing, a "vs. alternatives" callout, or any copy that names what the visitor is currently using. The "reason-to-believe" framing in §4.4 is noted but not enforced by any testable consequence.

**Recommendation:** Add a testable consequence to FR-9 or FR-4 that at least one hero/feature element explicitly contrasts Prachar with manual WhatsApp broadcasts (without necessarily naming paid competitors), satisfying the brief's "not a generic feature list" directive.

---

### Gap 3 — Secondary persona journey incompletely specified for "forwarded link" use case

**Brief says:** The Business Executive (secondary) "will share the URL before a decision is made — the website must work as a forwarded link, not just a search landing page." The brief treats this as a distinct site design constraint.

**PRD handles it:** UJ-2 (Meena) covers the forwarding scenario, and FR-12 includes a testable consequence that `#pricing` deep-linking works on mobile Chrome. However, no functional requirement addresses Open Graph / WhatsApp link preview specifically for the *forwarded WhatsApp message* rendering experience (i.e., the og:title and og:image that appear in the WhatsApp chat before the recipient taps). FR-19 covers OG tags generally, but the testable consequence only checks that "WhatsApp link preview renders og:title and og:image correctly" — it does not specify that the preview image must show recognisable INR pricing or a campaign visual that makes a boss want to tap without being told to. This is a copy/content requirement, not just a technical one.

**Recommendation:** Add a note or consequence to FR-19 that the og:image and og:title are optimised for the WhatsApp-forward discovery path (boss sees the preview and understands the value before tapping), consistent with the brief's forwarded-link constraint.

---

### Gap 4 — "No AI moats" copy guardrail not reflected in the PRD

**Brief says (explicitly):** "The website should not claim AI moats or proprietary technology. Execution speed and Indian-market specificity are the honest advantages at this stage."

**PRD handles it:** Not at all. No functional requirement, consequence, or assumption restricts feature copy from overclaiming AI capabilities. FR-9's suggested copy is appropriately grounded ("Describe your promotion... Get a professionally designed... campaign in under 15 seconds"), but there is no enforceable guardrail in the PRD preventing a copywriter or developer from adding language like "proprietary AI engine" or "best-in-class ML model."

**Recommendation:** Add a single testable consequence to FR-9 (or a site-wide copy constraint in §4.1 or Appendix A) stating that no copy on the site claims proprietary AI technology, algorithmic superiority, or AI moats — and that "execution speed and Indian-market specificity" are the approved framing for differentiation.

---

### Gap 5 — Success metric SM-1 definition gap: "conversion rate" measurement methodology

**Brief says:** "Free trial sign-up conversion rate ≥3% of unique visitors" is measured by "Account creation events."

**PRD says (SM-1):** "≥3% of unique homepage visitors click a CTA and complete registration at app.fineintegrity.com/register." The PRD measures CTA *click* + registration — which is cross-domain and requires the GA4 cross-domain measurement to be working (FR-26, Open Question §8.3). If cross-domain measurement is not configured at launch, the PRD's SM-1 is unmeasurable from day one.

**Inconsistency:** The brief's measurement method ("Account creation events") implies the metric is owned by the product side (app.fineintegrity.com). The PRD assigns it to the marketing site analytics without a confirmed data path. This creates a measurement gap: the marketing site can confirm CTA clicks (FR-24) but cannot confirm completed registrations without cross-domain GA4 or a shared event from the product.

**Recommendation:** SM-1 should explicitly state the fallback measurement approach if GA4 cross-domain is not ready at launch (e.g., CTA click rate as a proxy), and Open Question §8.3 should be elevated to a launch blocker, not merely an open question.

---

## Summary

| # | Area | Severity |
|---|---|---|
| 1 | Urgency / seasonal calendar not enforced in copy requirements | Medium |
| 2 | Competitor differentiation framing not a testable requirement | Medium |
| 3 | Forwarded WhatsApp link preview not specified for secondary persona path | Low |
| 4 | "No AI moats" copy guardrail absent from PRD | Medium |
| 5 | SM-1 conversion rate has a measurement dependency gap vs. brief | High |
