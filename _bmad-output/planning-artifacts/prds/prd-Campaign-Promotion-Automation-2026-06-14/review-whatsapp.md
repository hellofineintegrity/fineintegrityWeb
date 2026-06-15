# WhatsApp / Meta Compliance Review
## PRD: Campaign Promotion Automation
**Reviewer:** WhatsApp Business API & Meta Compliance Specialist
**Review date:** 2026-06-15
**PRD version:** Draft 2026-06-14
**Verdict:** CONDITIONAL PASS — The PRD is directionally sound but contains four HIGH-severity inaccuracies that could block the pilot or create legal exposure. Twelve issues total are documented below.

---

## Severity Key

| Level | Meaning |
|---|---|
| HIGH | Factually incorrect or missing; will cause pilot failure or policy violation if not corrected before build. |
| MEDIUM | Partially correct or ambiguous; needs clarification to avoid rework during implementation. |
| LOW | Minor imprecision or missing nuance; unlikely to block the pilot but should be tightened for accuracy. |

---

## Finding 1 — Opt-in for synced WhatsApp contacts is LEGALLY insufficient
**Severity: HIGH**
**PRD location:** FR-7, FR-11, FR-37, Appendix B (WhatsApp Opt-in row), UJ-2 "sent automatically on October 1 at 8:00 AM to 142 opted-in customers"

### What the PRD says
FR-7 describes a WhatsApp Business contact sync that automatically creates Customer Profiles, and FR-11 sets `opt-in date` when the status is first marked `true`. FR-37 and UJ-2 gate sending on `whatsapp_opt_in = true`. FR-9 exposes `WhatsApp opt-in status` as a boolean settable by Admin.

### What Meta's policy actually requires
Meta's WhatsApp Business Messaging Policy (updated 2024) mandates that **opt-in must be actively given by the end customer** — it cannot be inferred from the fact that the customer's number exists in the business's WhatsApp contact list or was shared in any offline context. Specifically:

- A WhatsApp contact sync (FR-7) gives you a phone number. It does **not** give you marketing opt-in for WhatsApp template messages.
- Opt-in must meet Meta's requirements: the customer must actively indicate their intent to receive messages from the business **on WhatsApp specifically**, via a channel the business controls (website, physical sign-in sheet with explicit WhatsApp consent wording, SMS, etc.). The medium used to collect opt-in must clearly state the business name and that messages will be sent on WhatsApp.
- An Admin manually flipping `whatsapp_opt_in = true` in the Platform (FR-8) is **not** valid consent under Meta's policy — this does not represent a customer action.
- India's Digital Personal Data Protection Act 2023 (DPDPA) adds a parallel requirement: consent must be free, specific, informed, unconditional, and unambiguous. Importing a phone number from a WhatsApp contact list does not satisfy any of these.

### Risk
Sending marketing template messages to contacts whose opt-in was established by Admin toggle rather than genuine customer consent exposes the business to:
1. Meta account suspension (common trigger for spam complaints and Quality Rating drops).
2. DPDPA enforcement (regulators are activating).
3. High opt-out / block rates invalidating SM-C1 counter-metric.

### Required PRD changes
- FR-7 must state explicitly: "Syncing a WhatsApp contact creates a Customer Profile with `whatsapp_opt_in = false` by default. Opt-in is **never** set by the sync process."
- FR-8 must remove Admin ability to directly set `whatsapp_opt_in = true` manually. Opt-in can only be recorded when the customer has taken a documented opt-in action.
- A new FR is needed: **Opt-in collection flow** — the Platform must provide (or document for the business) a mechanism by which customers actively consent (e.g., a web opt-in form per branch, a QR code to a consent page, or an offline sign-up sheet whose completion is manually logged with the consent source). The consent source and date must be stored per customer.
- Appendix B must be updated to state consent source requirements.
- The pilot onboarding plan (Open Question 3 / Meta Business verification) must address how the 3 pilot businesses will legitimately collect opt-in from their existing customer base before the Diwali send.

---

## Finding 2 — "1,000 free marketing conversations/month" is WRONG
**Severity: HIGH**
**PRD location:** FR-37 consequences ("throttled to stay within the Branch's Meta free tier of 1,000 marketing conversations/month"), Appendix D ("1,000 free service conversations/month per number"), §6.1 MVP scope ("WhatsApp delivery via Meta Cloud API (within free tier, 1,000 messages/month/number)"), Appendix C cost basis

### What the PRD says
FR-37 calls it "1,000 marketing conversations/month." Appendix D calls it "1,000 free **service** conversations/month." The two labels are inconsistent with each other and both are inaccurate.

### What Meta's pricing actually says (as of 2024–2025)
Meta's WhatsApp Business Platform pricing has two dimensions:

1. **Free tier:** 1,000 **free conversations per month** — but this applies to **user-initiated (service) conversations only**. These are conversations opened when a customer messages the business first. Business-initiated marketing template messages are in a **separate category and are NOT included in the free 1,000**.

2. **Marketing templates are charged from the first message.** There is no free allocation for business-initiated marketing or utility messages. Meta charges per 24-hour conversation window:
   - Marketing conversations: approximately USD 0.0121 per conversation (India pricing tier, as of mid-2025).
   - Utility conversations: approximately USD 0.0042 per conversation.
   - Authentication conversations: approximately USD 0.0082 per conversation.
   - Service (user-initiated) conversations: 1,000 free/month; ~USD 0.0042 above that.

3. The PRD's Appendix C cost basis of "Meta API ~₹1.50/WhatsApp msg" is actually in the right ballpark for marketing conversations at current exchange rates (~USD 0.012 × ~82 INR = ~₹1.00, closer to ₹1.00–₹1.25 at current rates). However it is reached by accident, not by accurate pricing reasoning.

### Risk
The PRD's free tier assumption is factually wrong. The pilot businesses will be charged for **every single marketing message they send** from day one. The cost basis in Appendix C will be slightly wrong. Budget planning, margin calculations, and the "1,000 messages/month" throttle in FR-37 are all built on a false premise. FR-37's throttle logic will never trigger (there is no free marketing tier to protect), yet the Platform will still incur costs the PRD has not accounted for.

### Required PRD changes
- Correct Appendix D to: "WhatsApp Cloud API: 1,000 free **user-initiated (service)** conversations/month per number. **Business-initiated marketing messages are charged per conversation from the first message**; no free marketing tier exists."
- Correct FR-37 consequences: remove the "1,000 marketing conversations/month free tier" throttle. Replace with: "Admin is warned when monthly WhatsApp spend (marketing conversations) exceeds a configurable threshold; a hard spend cap can optionally be set per Branch."
- Update Appendix C cost basis with accurate India-tier marketing conversation pricing.
- Confirm with Nilesh whether the free trial plan subsidises Meta costs or passes them to the pilot business.

---

## Finding 3 — Meta template auto-submission timing is WRONG and missing required fields
**Severity: HIGH**
**PRD location:** FR-34, UJ-2 step 5, §6.1 MVP scope, Appendix B

### What the PRD says
FR-34: "When a Promotion is approved for a WhatsApp Campaign, the Platform automatically formats the Promotion as a Meta-compliant WhatsApp Template and submits it to Meta for approval via the WhatsApp Cloud API. Triggered automatically on Promotion approval."

UJ-2 step 5: "System checks Meta template approval status — shows 'Pending Meta Approval' (submitted automatically on first approval). Priya clicks 'Submit to Meta.'" (Note: UJ-2 contradicts FR-34 — it shows a manual submit button.)

### What the Meta template submission process actually requires

**Missing required fields for template submission:**
The WhatsApp Cloud API template create endpoint (`POST /v1/WHATSAPP_BUSINESS_ACCOUNT_ID/message_templates`) requires fields the PRD does not mention:
- `name` — a unique, lowercase, alphanumeric+underscore template name (not the Promotion title). Must be unique per WABA. The PRD does not address how the Platform generates unique template names or what happens on name collision.
- `category` — must be one of `MARKETING`, `UTILITY`, or `AUTHENTICATION`. The PRD never specifies that campaigns are categorised as `MARKETING`. This matters for pricing (finding 2) and content rules.
- `language` — a BCP-47 language code (`en`, `hi`, etc.). The PRD mentions language selection but does not confirm language codes are passed to the template submission.
- `components` array — structured JSON with `HEADER` (image), `BODY` (text with `{{1}}`-style variables), `FOOTER`, and `BUTTONS`. The PRD treats the WhatsApp template as a rendered creative but Meta requires it to be parameterised. The body text must use `{{1}}`, `{{2}}` variable placeholders for any customer-specific content, not literal names/values baked in.
- `allow_category_change` — optional but recommended so Meta can reclassify to avoid rejection.

**Timing issues:**
- Template approval typically takes **minutes to 24 hours**, not "within 3 hours" (UJ-2). Peak periods (festival seasons, when every Indian SMB is submitting templates simultaneously) can see delays of 24–48 hours. The PRD's "within 3 hours" in UJ-2 is optimistic and will mislead pilot users.
- Meta's review SLA is not guaranteed. The PRD must communicate to users that approval timing is variable.

**Template name uniqueness per WABA:**
- Each WhatsApp Business Account (WABA) has a flat namespace for template names. If a business clones a campaign and resubmits, the same template name already exists — Meta will reject it as a duplicate. The PRD has no strategy for template name management.

**Per-language template submissions:**
- Each language variant of a template is a separate submission. An English and a Hindi version of the same promotion require two separate template submissions and approvals.

### Required PRD changes
- FR-34 must be expanded to specify: template `name` generation strategy (e.g., `{branch_slug}_{promotion_id}_{lang}`), `category` field (always `MARKETING` for this use case), `language` field, and the parameterised `components` structure.
- FR-16 consequences must clarify that the WhatsApp body text is parameterised (with `{{1}}` etc.) for any variable content, not rendered with literal customer data at generation time (personalisation is applied at send time via message parameter injection).
- UJ-2 should reflect that approval can take up to 24–48 hours, not 3 hours.
- FR-34 must address template name collision on clone/resubmit scenarios.
- The architecture must handle multi-language template submissions as separate API calls per language.

---

## Finding 4 — WhatsApp contact sync (FR-7) is NOT possible as described
**Severity: HIGH**
**PRD location:** FR-7, §6.1 MVP scope

### What the PRD says
FR-7: "When a new contact is added to the Branch's connected WhatsApp Business account, the Platform automatically creates or updates the corresponding Customer Profile within 5 minutes."

### What the Meta Cloud API actually supports
The WhatsApp Cloud API (and the WhatsApp Business API in any form) **does not expose a contact list**. Specifically:
- There is no API endpoint to retrieve contacts stored in a WhatsApp Business account.
- There is no webhook event that fires when a new contact is added to a WhatsApp Business account's address book.
- The Cloud API is message-sending and webhook-receiving only. It has no CRM-style contact management surface.

The only way a phone number enters the Platform's awareness via the API is when that number **sends a message to the business number first** (generating an inbound message webhook). At that point the sender's `wa_id` (phone number) is available.

**What FR-7 might be confusing this with:**
- WhatsApp Business App (the consumer/SMB mobile app) has a local phone contact book sync — but this is a local phone feature, not accessible via any API.
- Importing contacts manually into a WhatsApp Business account's broadcast list is a UI-only action in the mobile app; it is not surfaced via the Cloud API.
- Some BSPs (business solution providers like Interakt, WATI) offer their own CRM layer on top of the API that stores contacts from inbound messages, but this is the BSP's own database — not a Meta API feature.

### Risk
FR-7 as written is technically impossible to implement using the WhatsApp Cloud API. Attempting to build this feature will either fail or require a manual import workflow that contradicts the PRD's "automatic" and "5 minutes" language.

### Required PRD changes
- FR-7 must be rewritten. Two valid alternatives:
  1. **Inbound message sync:** "When a customer sends any message to the Branch's WhatsApp number, the Platform creates or updates a Customer Profile with the customer's `wa_id` (phone number) and any profile name provided in the webhook." This is technically correct and is the real-world mechanism.
  2. **Manual CSV import:** "Business Admin can import Customer Profiles via CSV upload (Name, Mobile Number, Email columns). Imported contacts have `whatsapp_opt_in = false` by default until a valid opt-in action is recorded." This is independent of WhatsApp and is the primary onboarding path for existing customer lists.
- Both alternatives should be present. The automatic "contact sync" framing must be removed.

---

## Finding 5 — STOP reply opt-out handling is partially correct but missing webhook constraint
**Severity: MEDIUM**
**PRD location:** FR-12, FR-43, UJ-3

### What the PRD says
FR-12: "STOP reply on WhatsApp is processed within 60 seconds of receipt." FR-43: "STOP replies are automatically processed as opt-outs and not shown as conversational replies."

### What the Cloud API actually does
The WhatsApp Cloud API does **not** provide a built-in keyword opt-out mechanism. When a customer replies "STOP", it arrives as an ordinary inbound message webhook (`messages` webhook of type `text`). The Platform must:
1. Receive the webhook.
2. Parse the message body and match it (case-insensitive) against opt-out keywords.
3. Record the opt-out in the database.
4. Optionally send a confirmation template message back to the customer ("You have been unsubscribed...").

This is what the PRD implies is happening, so FR-12 is directionally correct. The gaps are:

- **Meta does not enforce STOP**. The responsibility is entirely on the Platform. If the webhook processing queue is delayed or the webhook handler has a bug, opt-outs are silently missed. The "60 seconds" SLA in FR-12 is Platform-internal; Meta places no requirement on how fast the business processes opt-outs.
- **Confirmation message requirement (India):** India's TRAI requires that opt-out confirmations be sent. A WhatsApp confirmation is not a regulated channel under TRAI (TRAI governs SMS), but best practice for Meta compliance is to send an acknowledgment. The PRD does not address this.
- **Post-STOP messaging restriction:** After a customer sends STOP, Meta's policy states the business **must not** send further marketing messages to that number. This is actually stricter than the PRD implies — even a re-opt-in template message to the opted-out number may violate Meta policy (this is the same issue flagged in Open Question 7, which the PRD correctly identifies but leaves unresolved). The PRD cannot leave Open Question 7 open through to pilot; it must be resolved at design time.
- **24-hour window interaction:** If the customer sends "STOP" within an active 24-hour service conversation window, the reply is a service message. The business replying with a confirmation uses the service conversation window (free). If the window is expired, any reply requires a template. The Platform needs to handle this correctly.

### Required PRD changes
- FR-12 and FR-43 are acceptable as-is, but add: "The Platform sends a WhatsApp confirmation template message to the customer acknowledging the opt-out. This confirmation template must be separately Meta-approved."
- Open Question 7 must be resolved in the PRD: "Meta policy prohibits sending any message (including re-opt-in requests) to numbers that have sent STOP. Re-opt-in for opted-out customers must be collected via a non-WhatsApp channel (web form, in-person, email). FR-11's [ASSUMPTION] about 'Reply START' is INVALID — remove it."
- FR-11 assumption must be corrected: "Re-opt-in via WhatsApp reply 'START' is in scope for v1" — this contradicts Meta policy and must be removed.

---

## Finding 6 — Message category usage is incomplete and pricing implications are missed
**Severity: MEDIUM**
**PRD location:** FR-37 ("1,000 marketing conversations/month"), Appendix B (template policy row), Appendix C (cost basis)

### What the PRD says
The PRD references "marketing conversations" inconsistently (sometimes "marketing conversations," sometimes "service conversations") and never explicitly states which Meta category the Platform will use.

### What Meta's categories actually require
Meta's three conversation categories have distinct rules:

| Category | When used | Body text rules | Free tier |
|---|---|---|---|
| MARKETING | Business-initiated promotions, offers | Can include offers, CTAs, promotional copy | None |
| UTILITY | Post-purchase notifications, order updates, reminders | Must relate to a prior transaction or interaction | None |
| AUTHENTICATION | OTPs, login verification | Highly restricted format | None |
| SERVICE | User sends message first; business responds within 24h | Any content allowed | 1,000 free/month |

All campaign messages in this Platform are `MARKETING` (business-initiated promotional offers). The PRD must:
1. Explicitly state this in FR-34 and the Glossary.
2. Note that using `UTILITY` category for what is functionally marketing content is a Meta policy violation that triggers rejection or account suspension.
3. Workflow notifications (FR-33) sent to business users (executives, admins) are also `MARKETING` or `UTILITY` — they are not service conversations. If Fine Integrity's notification WhatsApp number sends these messages without approved templates, that also violates policy.

### Required PRD changes
- FR-34 must specify `category: MARKETING` in the template submission.
- Appendix B must clarify: all campaign messages = MARKETING category; service conversations = user-initiated only.
- FR-33 workflow notifications via WhatsApp must also use Meta-approved UTILITY or MARKETING templates, not free-form messages. Add this constraint.

---

## Finding 7 — AI content guardrails do not fully match Meta rejection patterns
**Severity: MEDIUM**
**PRD location:** Appendix B ("Meta WhatsApp Template policy" row: "AI content generation guided to avoid common rejection triggers: excessive urgency, false claims, excessive caps")

### What the PRD says
Three guardrails: excessive urgency, false claims, excessive caps.

### What Meta's actual rejection patterns include
Meta's template review rejects for a significantly broader set of triggers than the three listed. Common rejection triggers not addressed in the PRD:

1. **Variable placeholders in headers** — Image header templates cannot have text overlays with variable content in them (common AI generation mistake).
2. **Shortened URLs / URL shorteners** — Templates containing bit.ly, t.co, or other link shorteners in button URLs are often rejected. The Platform's tracking redirect (FR-42) may trigger this if the redirect domain is not pre-registered with Meta. Meta requires the business to have verified ownership of any URL domain used in templates.
3. **Prohibited content categories** — Alcohol, tobacco, gambling, adult content, weapons, healthcare claims, financial products without proper disclaimers. For the 3 pilot businesses these are unlikely, but the PRD's guardrail list should acknowledge the full prohibited list.
4. **"Guaranteed" language** — Templates using words like "guaranteed," "100% certain," "no risk" are frequently rejected.
5. **Missing opt-out in footer** — Templates without a clear opt-out instruction in the footer are rejected. The PRD mentions this footer (FR-16 consequences), but the AI guardrails in Appendix B do not mention it as a required element.
6. **Phone numbers or email addresses in body text** — Meta sometimes rejects templates with contact info embedded in the body (they prefer it in CTAs). The PRD auto-injects phone and email into generated content (FR-20), which may trigger rejections.
7. **Emoji overuse** — Not consistently enforced but noted in Meta's best practices.
8. **CTA button URL must be a registered domain** — The business must pre-register the domain of any URL used in template buttons with their WABA. The PRD does not mention domain registration as part of onboarding.

### Required PRD changes
- Appendix B guardrails list must be expanded to include at minimum: URL shortener prohibition, prohibited content categories, domain pre-registration requirement for CTA URLs, phone/email injection risk in body text.
- FR-34 or a new FR must add: CTA button URL domain must be registered with the business's WABA before template submission.
- FR-5 (WhatsApp connection) onboarding steps should include domain registration.

---

## Finding 8 — Fine Integrity's own WhatsApp notification number faces the same policy constraints
**Severity: MEDIUM**
**PRD location:** FR-33, Open Question 4, §9 Assumptions Index

### What the PRD says
FR-33: "WhatsApp workflow notifications are sent via the Platform's own WhatsApp integration. [ASSUMPTION: Fine Integrity will maintain a dedicated WhatsApp Business number for platform notifications.]"

### What is actually required
The PRD treats this as an operational/resourcing question (Open Question 4). It is also a technical and policy question:

1. **Templates required:** Fine Integrity's notification number must use Meta-approved templates for all business-initiated notification messages (approval alerts, rejection alerts, etc.). These are utility templates, not free-form messages. Fine Integrity must submit and get approved a suite of notification templates (at least: "Your promotion has been approved," "Your promotion has been rejected: {{1}}," "Campaign published," "Opt-out received from {{1}}"). These approvals take time and must be done before pilot launch.

2. **WABA required:** Fine Integrity needs its own WhatsApp Business Account (separate from pilot businesses' WABAs) with a verified phone number. This requires:
   - Meta Business verification for Fine Integrity's own business.
   - Phone number registration.
   - Template approval for all notification types.
   - This is non-trivial and typically takes 1–2 weeks minimum.

3. **Messaging limit at launch:** New WABAs start at Tier 1 (1,000 unique business-initiated conversations per 24 hours). With 3 pilot businesses and workflow notifications, this is not a constraint during pilot. But it should be documented.

4. **Alternative:** The PRD already notes that "notification delivery failure does not block the workflow action." Given the complexity, it is worth explicitly stating in the PRD that WhatsApp workflow notifications are a "nice-to-have" enhancement and that email notifications are the primary guaranteed channel. WhatsApp notifications can be deferred post-pilot.

### Required PRD changes
- FR-33 assumption must acknowledge the technical requirements: Fine Integrity must obtain its own WABA, get Meta Business verification, register a number, and submit notification templates for approval before pilot launch.
- Open Question 4 should be elevated from "operational" to a pilot-blocking dependency with a resolution deadline.
- Recommend: defer WhatsApp workflow notifications from pilot v1; use email as primary notification channel and add WhatsApp notifications as a v2 enhancement. Update FR-33 accordingly.

---

## Finding 9 — Re-opt-in via "Reply START" contradicts Meta policy
**Severity: MEDIUM**
**PRD location:** FR-11 [ASSUMPTION], Open Question 1, Open Question 7, §9 Assumptions Index

### What the PRD says
FR-11 [ASSUMPTION]: "Re-opt-in flow via WhatsApp reply 'START' is in scope for v1." Open Question 7: "Meta's policy may restrict the Business from sending any message (including a re-opt-in request) to a user who has sent STOP."

### What Meta's policy says
Open Question 7 answers its own question: the PRD is correct to flag this as a risk. Meta's WhatsApp Business Messaging Policy explicitly states that businesses **must not** message users who have opted out. This includes sending a re-opt-in invitation. There is no "START" keyword mechanism in WhatsApp Cloud API.

A customer who has sent STOP can only re-opt-in via:
- Initiating a new conversation by messaging the business themselves (user-initiated; business can then ask for re-consent within the 24-hour service window).
- Completing an opt-in form on a non-WhatsApp channel (web, in-store, email).

The PRD assumption that "Reply START" is an in-scope v1 feature is directly contradicted by Meta policy. Building it would risk account suspension.

### Required PRD changes
- Remove the FR-11 [ASSUMPTION] about "Reply START."
- Add an explicit note: "Re-opt-in after STOP can only occur via user-initiated WhatsApp contact or via a non-WhatsApp opt-in channel. The Platform records re-opt-in when: (a) a customer sends any inbound message after opting out (setting `whatsapp_opt_in = true`, `whatsapp_opt_out = false`, with timestamp), or (b) an Admin records re-consent from a non-WhatsApp channel with consent source documentation."
- Close Open Questions 1 and 7 with this resolution.

---

## Finding 10 — WhatsApp read receipts require the customer to have read receipts enabled
**Severity: LOW**
**PRD location:** FR-40, FR-37, SM-4

### What the PRD says
FR-40 lists "Read" as a WhatsApp delivery status. SM-4 targets "90% delivered." FR-41 ranks campaigns by "Read count / Delivered count."

### What Meta actually delivers
WhatsApp "Read" status (double blue ticks) is only reported in the Cloud API webhook if the recipient has read receipts enabled in their WhatsApp privacy settings. A significant proportion of Indian users disable read receipts. In practice, read rate from Meta webhooks is materially lower than true read rate. The PRD should note this so pilot businesses are not confused by the analytics.

### Required PRD changes
- Add a footnote to FR-40 or Analytics section: "WhatsApp 'Read' status is only reported when the recipient has read receipts enabled. Actual message reads may be higher than the Platform's reported read count."
- SM-4 should be "Delivered" rate target (90%); "Read" rate tracking is useful but cannot be used as a success metric due to read receipt privacy settings.

---

## Finding 11 — Meta Business verification is a pilot-blocking prerequisite not adequately flagged
**Severity: MEDIUM**
**PRD location:** Open Question 3, §9 Assumptions Index (absent from assumptions), FR-5

### What the PRD says
Open Question 3: "How will the 3 pilot businesses be guided through Meta's WhatsApp Business Platform verification? Will Fine Integrity provide hands-on onboarding support?"

### What verification actually entails
Meta Business verification is a prerequisite for accessing the WhatsApp Cloud API (specifically for the Display Name to show on messages and for messaging limits above Tier 1). The process:

1. The business must have a verified Meta Business Manager account.
2. Business verification requires: legal business name matching government documents, business phone number, website (optional but helps), registered address. Takes 1–10 business days; sometimes requires resubmission.
3. **Phone number registration:** The phone number used for WhatsApp must not be registered on WhatsApp consumer app or WhatsApp Business App. If a pilot business currently uses their phone number on WhatsApp Business App, it must be migrated (a separate process that can take hours to days).
4. **Display name approval:** Even after WABA verification, the display name shown to customers must be approved separately. Unapproved names show as the phone number until approved.

For 3 pilot businesses targeting Diwali (October 2026), if Meta Business verification has not started, it needs to start by August 2026 at the latest to allow buffer for re-submissions.

### Required PRD changes
- Open Question 3 must be elevated to a pilot-blocking dependency with a specific action owner and deadline.
- FR-5 (WhatsApp Business account connection) must note: "Meta Business verification for the business's WABA must be completed before the WhatsApp number can be connected. This is a prerequisite the Admin must complete outside the Platform; the Platform's onboarding flow should link to Meta's verification documentation."
- A new assumption: "All 3 pilot businesses will complete Meta Business verification and phone number registration by [date] to enable pilot onboarding."

---

## Finding 12 — Glossary "WhatsApp Template" definition omits required structural constraints
**Severity: LOW**
**PRD location:** §3 Glossary, "WhatsApp Template" entry

### What the PRD says
"A Meta-approved message format (header image + body text + CTA button) required by the WhatsApp Cloud API before any marketing message can be sent."

### What is missing
- **Footer** is a distinct component in Meta's template structure (separate from body) and is where the opt-out instruction lives. The PRD's AI generation spec (FR-16) mentions footer but the Glossary definition omits it.
- **Variable parameters** (`{{1}}`, `{{2}}`) are a fundamental structural requirement of templates. Templates are not rendered messages — they are parameterised skeletons. The Glossary should mention this.
- **Template name uniqueness** within a WABA is a key operational constraint.
- Body text limit is 1,024 characters (FR-16 correctly states this; Glossary should reference or be consistent).

### Required PRD changes
- Glossary entry update: "A Meta-approved parameterised message structure (header image, body text with `{{n}}` variable placeholders, footer, CTA button) required for all business-initiated WhatsApp messages. Each template has a unique name within the business's WhatsApp Business Account and must be approved before use."

---

## Summary of Required Changes

| # | Finding | Severity | Action Required |
|---|---|---|---|
| 1 | Opt-in for synced contacts is legally insufficient | HIGH | Rewrite FR-7; restrict Admin opt-in toggle in FR-8; add opt-in collection FR |
| 2 | Free tier is "service conversations," not "marketing" — marketing is paid from message 1 | HIGH | Correct FR-37, Appendix D, §6.1, Appendix C cost basis |
| 3 | Template submission missing required fields; approval timing overstated; no name strategy | HIGH | Expand FR-34; correct UJ-2 timing; add name generation strategy |
| 4 | WhatsApp contact sync (FR-7) is technically impossible via Cloud API | HIGH | Rewrite FR-7 as inbound-message-triggered or CSV import |
| 5 | STOP handling correct but "Reply START" re-opt-in is invalid per Meta policy | MEDIUM | Remove FR-11 [ASSUMPTION]; correct re-opt-in flow description |
| 6 | Message category (MARKETING) never stated; workflow notifications also need templates | MEDIUM | Add MARKETING category to FR-34; address FR-33 notification templates |
| 7 | AI guardrails incomplete vs actual Meta rejection patterns | MEDIUM | Expand Appendix B guardrails; add domain registration requirement |
| 8 | Fine Integrity's own WhatsApp notification number underestimates setup complexity | MEDIUM | Expand FR-33; flag as pilot-blocking; consider deferring to v2 |
| 9 | Reply START re-opt-in contradicts Meta policy (same as Finding 5 but in FR-11) | MEDIUM | Close Open Questions 1 and 7 with correct resolution |
| 10 | Read receipts are privacy-gated; read rate metric will be understated | LOW | Add footnote to FR-40; soften read rate in SM-4 |
| 11 | Meta Business verification is pilot-blocking; not tracked as dependency | MEDIUM | Elevate Open Question 3; add to FR-5 and assumptions |
| 12 | Glossary WhatsApp Template definition incomplete | LOW | Update Glossary entry to include footer, variables, name uniqueness |

---

## Pilot-Blocking Items (Must Resolve Before Architecture Design)

These findings must be resolved in the PRD before the architect can design the WhatsApp integration:

1. **Finding 1** — How will opt-in be legitimately collected for pilot customers? Design the opt-in collection flow.
2. **Finding 2** — Confirm pilot businesses accept that all marketing WhatsApp messages are paid from day one. Update cost model.
3. **Finding 3** — Define template name strategy and acknowledge parameterised template structure.
4. **Finding 4** — Replace the impossible contact sync with the correct inbound-message + CSV import model.
5. **Finding 11** — Set a hard deadline for Meta Business verification for all 3 pilot businesses.

---

*Review completed: 2026-06-15. This document covers the WhatsApp/Meta-specific aspects of the PRD only. Email delivery, TRAI compliance, Azure infrastructure, and billing are not in scope for this review.*
