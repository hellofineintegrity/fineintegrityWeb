# SMB Market Fit Review: Campaign Promotion Automation PRD
**Reviewer role:** Indian SMB market and product pricing specialist
**Review date:** 2026-06-15
**PRD version:** Draft, 2026-06-14
**Pilot businesses:** AutoCare Services (automotive), Shree Ladies Garments (ladies garments retail), Fresh Basket (grocery)

---

## Overall Verdict

**CONDITIONAL FIT — proceed with caution on pricing and customer data assumptions.**

The product vision is sound and well-targeted. The festival calendar angle, AI copy generation in Hindi, and WhatsApp-first delivery align tightly with how Indian SMBs actually promote. However, four structural risks — pricing that is 2–6x above what pilot businesses will intuitively accept, a Free Trial design that is too thin to demonstrate value for at least two of the three pilot businesses, unrealistic assumptions about pre-existing structured customer lists, and the WhatsApp free tier being a hard ceiling that will be hit within weeks for a grocery store — need resolution before pilot launch. None of these are fatal. All are fixable now, before build, at zero engineering cost.

---

## Finding 1 — CRITICAL: Pricing is misaligned with Indian SMB willingness to pay

**Severity: CRITICAL**
**Affects:** All three pilot businesses

### The numbers as stated
- ₹3.00 per WhatsApp message sent
- ₹0.50 per email sent
- ₹500/branch/month minimum

### Realistic monthly send volumes for each pilot business

| Business | Typical customer WhatsApp list size | Expected monthly sends (campaigns × contacts) | Monthly bill at ₹3/msg |
|---|---|---|---|
| AutoCare Services | 200–400 active customers | 2 campaigns × 300 = 600 msgs | ₹1,800/month |
| Shree Ladies Garments | 300–600 loyal customers | 3 campaigns × 400 = 1,200 msgs | ₹3,600/month |
| Fresh Basket | 500–1,500 customers | 4–6 campaigns × 800 = 4,800 msgs | ₹14,400/month |

### Why this is a problem

**For AutoCare Services:** ₹1,800/month for a garage with 2–4 staff is roughly 20–35% of what they pay for a part-time helper. An auto services owner in Pune will balk at this. Their frame of reference is WhatsApp Business (free, unlimited). The comparison they will make is zero vs ₹1,800. Getting them to see value will require very strong demonstrated ROI — a single car PPF job (₹15,000–₹30,000) pays for many months, but they will not naturally make that calculation without hand-holding.

**For Shree Ladies Garments:** ₹3,600/month for a standalone garments shop is significant. The owner's instinct will be to compare this to what she pays for printing 500 festival offer pamphlets (₹800–₹1,200 per run). She is already using Instagram to post saree photos for free. The value story needs to be "you reach your specific list of buyers who already bought from you" — but she needs to be educated on why that is worth ₹3,600.

**For Fresh Basket:** At any reasonable grocery send frequency (weekly offers are normal for grocery), the bill spirals to ₹10,000–₹20,000/month. This is completely out of range for a standalone grocery store with 5–8% net margins. Fresh Basket is the worst-fit use case for per-message pricing and risks becoming a case study in why the product doesn't work.

### Competitive reference pricing
- **Zoko:** ₹2,999–₹7,999/month flat, unlimited contacts, per-conversation pricing layered on top. But Zoko is positioned at businesses with dedicated marketing staff.
- **Interakt:** ₹999–₹2,499/month for starter plans.
- **WATI:** USD 49/month (≈ ₹4,100/month) with 5 agents, 1,000 contacts.
- **WhatsApp Business broadcast (free):** The true competition — zero cost, no platform needed, limited to 256 per broadcast.

The closest Indian SMB comparison is Mailchimp's free-to-entry model or Meesho's zero-fee seller model. Indian SMBs have been trained to expect very low or zero cost for digital tools.

### Recommendation

Consider a flat monthly plan model with WhatsApp API cost passed through at near-actual cost (Meta charges ~₹0.72–₹1.12 per marketing conversation in India; the PRD notes ₹1.50/msg cost basis). Suggested restructure:

| Plan | Price | What's included |
|---|---|---|
| Starter | ₹999/month/branch | Up to 500 WhatsApp messages/month, unlimited emails, all features |
| Growth | ₹1,999/month/branch | Up to 1,500 WhatsApp messages/month |
| Pro | ₹3,499/month/branch | Up to 5,000 WhatsApp messages/month |
| Overage | ₹1.50/WhatsApp msg | Above plan limit |

This makes costs predictable, reduces sticker shock, and lets the owner calculate the maximum they'll ever pay. Flat plans are dramatically easier to sell to Indian SMBs than per-unit pricing. The ₹500/branch minimum in the PRD is too low to mean anything as a floor — it doesn't protect revenue and doesn't help the customer understand what they're buying.

**Action required before build:** Validate willingness to pay with at least one of the three pilot owners before designing the billing system. Ask: "If this tool saved you 5 hours a month and got 20% better response to your Diwali offer, what would you pay per month?" You will likely hear ₹500–₹1,500.

---

## Finding 2 — HIGH: Free trial is too thin for grocery and automotive; may not reach the "aha moment"

**Severity: HIGH**
**Affects:** Fresh Basket (high), AutoCare Services (medium), Shree Ladies Garments (low)

### The stated free trial
- 1 month
- Maximum 3 campaigns published per branch
- All features unlocked

### Why 3 campaigns may not be enough

**Fresh Basket (grocery):** Grocery businesses live on weekly offers — "Weekend Vegetable Special," "Mango Season Offer," "Bakri Eid Meat Discount." Three campaigns in a month means one campaign every 10 days — well below the natural cadence a grocery owner would run if the tool made it easy. The aha moment for Fresh Basket is: "I sent the Friday vegetable offer to 400 customers at 6pm and got 30 walk-ins by Saturday morning." You need at least 4–6 sends to see that pattern. Three sends gives you three data points, none of which is statistically meaningful to a small business owner.

**AutoCare Services (automotive):** Automotive is low-frequency — a customer services their car 2–4 times per year. Three campaigns in a month is actually above the natural run rate. The problem here is different: with a list of 200–400 contacts and low purchase frequency, a single campaign may show a read rate of 80%+ and 0 immediate conversions. This can make the product look ineffective even if it worked. The aha moment for AutoCare is: "3 customers called about the monsoon wash package after I sent the message." That may or may not happen in just 3 campaigns.

**Shree Ladies Garments:** Three campaigns is actually appropriate if all three are timed to the Navratri–Diwali–Christmas window (the PRD correctly identifies this as the pilot launch window). Festival retail is high-spend and customers respond. This is the best-fit pilot business for the current free trial design.

### Recommendation

For grocery (Fresh Basket specifically): change the free trial cap to **10 campaigns** or **30 days, unlimited campaigns, capped at 1,000 WhatsApp messages total** (which they cannot exceed anyway on the Meta free tier). This lets Fresh Basket run weekly offers for a month and actually see the value.

For automotive: the aha moment is not volume — it is the quality of the creative and the customer response. Consider supplementing the success metric with a qualitative check: "Did any customer mention seeing the WhatsApp message?"

Alternatively, re-frame the success metric SM-2 ("each pilot business publishes at least 2 campaigns before free trial ends") to something more outcome-focused: "At least 1 pilot business receives a direct customer response (phone call, walk-in, or WhatsApp reply) attributable to a campaign."

---

## Finding 3 — HIGH: Customer list assumptions are unrealistic for all three pilot businesses

**Severity: HIGH**
**Affects:** All three pilot businesses

### The PRD's assumption (FR-7, FR-9, FR-10)

The PRD describes WhatsApp Business contact sync as the primary onboarding method for customer data. It assumes structured customer profiles with loyalty tiers, preferred language, email addresses, and opt-in status.

### Reality for these businesses

**AutoCare Services:** The contact list lives in the service job register (paper or a basic app like Vyapar/Busy), the mechanic's personal WhatsApp chats, and the owner's phone contacts. There is almost certainly no "WhatsApp Business account" with a curated contact list — most small garages use a personal number or a basic WhatsApp Business number where contacts are just saved as "Rahul Car" or "Maruti Swift guy." They do NOT have email addresses for customers. Opt-in status is undefined — no customer has formally consented to receive promotions.

**Shree Ladies Garments:** Customer contacts are in a combination of: (a) the owner's personal phone contacts, (b) a WhatsApp group for loyal customers (50–80 people), (c) physical sale receipts. There may be a Tally/billing system but it is unlikely to have mobile numbers cross-referenced with names cleanly. Email addresses: near-zero. The "Loyal Customers" group with 142 contacts shown in UJ-2 is aspirational, not typical.

**Fresh Basket:** Grocery stores in India operate almost entirely on cash/UPI (PhonePe/GPay). The digital footprint is the UPI QR code. Customers are anonymous. A fresh basket owner may know 20–30 regular customers by face; structured mobile number records are exceptional, not the norm. Many grocery customers may have 5–10 regular customers' numbers saved, not hundreds.

### Specific gaps

1. **WhatsApp Business contact sync (FR-7) assumes the business already has a WhatsApp Business account with contacts in it.** In practice, all three businesses will be starting from near-zero structured contact lists. The sync feature is useless until they build the list from scratch.

2. **Opt-in status (FR-11) is a significant legal and operational hurdle.** None of these businesses have ever asked customers for explicit promotional consent. The PRD correctly enforces opt-in, but the PRD does not provide any mechanism to help businesses *collect* opt-ins from scratch. This is a critical missing feature (see Finding 8).

3. **Loyalty tiers (FR-9 field: Bronze/Silver/Gold/Platinum) are fictional for all three businesses.** They have no loyalty program. This field will be blank and unusable for months.

4. **The 142-contact "Loyal Customers" group in UJ-2 is aspirational.** A realistic starting point for Shree Ladies Garments is 20–50 contacts, mostly from the owner's personal WhatsApp or WhatsApp group.

### Recommendation

Add an explicit "cold start" onboarding path:
- Bulk import via CSV (name + mobile number minimum; all other fields optional)
- A "collect opt-in" WhatsApp message template that businesses can send to their existing informal contact list as a first action
- Guidance to business owners during onboarding: "Start by importing your 20 most loyal customers and send them a 'We'd love to stay in touch' message." This sets realistic expectations and gives the system something to work with.

The absence of a CSV import is a notable gap in the PRD (see also Finding 8).

---

## Finding 4 — HIGH: WhatsApp free tier ceiling will be hit immediately by Fresh Basket

**Severity: HIGH**
**Affects:** Fresh Basket primarily; also relevant to Shree Ladies Garments if she builds her list

### The constraint (FR-37, §4.6 Assumption)

The PRD states: "Sending is throttled to stay within the Branch's Meta free tier (1,000 marketing conversations/month per number)."

The PRD's assumption notes: "Meta's free tier of 1,000 marketing conversations/month per WhatsApp number is sufficient for all 3 pilot businesses during the free trial period."

### Why this assumption is incorrect

Meta's WhatsApp Cloud API charges per "conversation," not per message. As of 2025–2026, **Meta has moved to a per-message pricing model** (not per conversation), but marketing template messages are charged per message sent. The first 1,000 free "service conversations" per month are for user-initiated conversations, not business-initiated marketing messages. Marketing template messages (which is what this platform sends) are **not free** — they are charged at approximately ₹0.72–₹1.12 per marketing message even from the first message.

This is a fundamental misread of Meta's pricing that has cost implications for both the platform (cost basis is wrong) and the businesses (they will incur Meta charges from day one, which is not surfaced to them).

Even if we accept the PRD's framing that there is a 1,000 message free ceiling: Fresh Basket with weekly grocery offers to a 300-person list would hit 1,200 messages in the first month (4 campaigns × 300 contacts). The hard block at 1,000 (as stated in FR-37) means their 4th campaign cannot go out. This is a terrible pilot experience.

### Recommendation

1. Urgently verify the current Meta WhatsApp Cloud API pricing for marketing template messages (as of June 2026). The "1,000 free marketing conversations/month" assumption must be validated — it is almost certainly incorrect or has materially changed.
2. If Meta charges per marketing message from message 1, update the cost basis in Appendix C and surface expected Meta API pass-through costs to businesses transparently.
3. For the pilot, consider pre-absorbing Meta API costs within Fine Integrity's pilot budget rather than leaving pilot businesses uncertain about what they'll be charged.

---

## Finding 5 — MEDIUM: Festival calendar is good but missing critical occasions for automotive and grocery

**Severity: MEDIUM**
**Affects:** AutoCare Services (medium), Fresh Basket (medium)

### What the PRD includes (FR-15)
Diwali, Holi, Eid al-Fitr, Christmas, New Year, Onam, Navratri, Pongal, Back-to-School, Wedding Season, Monsoon Season, Summer, Year-End Clearance.

### What is missing by business type

**AutoCare Services — automotive services (PPF, denting, polishing, wash):**
- **Pre-monsoon car care** (May–June): "Before the rains hit, get your underbody coated." This is the single biggest seasonal campaign for automotive care businesses. The PRD has "Monsoon Season" but not "Pre-Monsoon" — the timing difference matters because customers act before the rains, not during.
- **New Year new car care** (January): First week of January is a strong moment for "Start the year fresh — full detailing" campaigns.
- **Road trip season** (October–November, post-Diwali): Long drives for family vacations trigger car servicing demand.
- **Car launch season** (whenever major launches happen): Aftermarket PPF and accessories spike after new model launches — this is niche but relevant to PPF/detailing shops.
- **Gudi Padwa / Ugadi / Baisakhi / Tamil New Year:** Regional "new beginnings" festivals trigger new car purchases and first-service visits. Missing from the calendar.

**Fresh Basket — grocery:**
- **Mango season** (April–June): The most commercially important seasonal moment for grocery in India. "Alphonso Mangoes arrived" is a campaign that sells itself.
- **Ganesh Chaturthi** (Maharashtra): Extremely high grocery demand for modak ingredients, flowers, coconuts. Missing from the calendar.
- **Makar Sankranti / Lohri / Pongal** (January): Pongal is listed but Makar Sankranti (North/West India) and Lohri (Punjab) are not. Grocery demand spikes for til, jaggery, groundnuts, rice.
- **Shravan month** (July–August, Maharashtra): Period of fasting — high demand for sabudana, fruits, milk products. Grocery-specific.
- **Weekend specials** (not a festival, but a recurring calendar slot): The PRD mentions this in UJ-3 but does not include "Weekend" as a calendar option. For Fresh Basket, Friday evening "weekend veggie special" is a weekly campaign need.
- **Harvest season / fresh produce arrivals:** No mechanism to create ad-hoc seasonal produce arrival campaigns ("Fresh Strawberries from Mahabaleshwar").

**Shree Ladies Garments — garments retail:**
- **Ganesh Chaturthi** (Mumbai/Pune): Very strong for new clothes purchasing. Missing.
- **Gudi Padwa** (Maharashtra New Year): New clothes are traditional. Missing.
- **Eid** is listed but **Eid al-Adha (Bakri Eid)** should be separate from Eid al-Fitr — different promotional context.
- **Valentine's Day / Friendship Day:** Modern garments retailers run promotions. Not present.
- **Republic Day / Independence Day sales:** Very common for garments retailers. Missing.
- **End of Season Sale (EOS):** Standard industry term for January and July clearance. "Year-End Clearance" partially covers this but the June–July EOS window is absent.

### Recommendation

Expand the festival calendar to at minimum 25 occasions, with industry-specific occasions tagged to Industries (so automotive businesses see pre-monsoon but grocery doesn't). The System Admin panel (FR-48) has this capability — it just needs the data. This is a data task, not an engineering task, and should be completed before pilot launch.

---

## Finding 6 — MEDIUM: Channel relevance varies significantly across the three businesses

**Severity: MEDIUM**
**Affects:** Fresh Basket (medium), AutoCare Services (low)

### WhatsApp as primary channel — assessment by business

**AutoCare Services:** STRONG FIT. Automotive service relationships are personal. A customer trusts "the mechanic" and responds to WhatsApp messages from known business numbers. Read rates for automotive service reminders on WhatsApp are high (industry anecdote: 60–80% read rates). The personal nature of the channel matches the personal nature of the service relationship.

**Shree Ladies Garments:** STRONG FIT. Fashion retail in India is highly visual. WhatsApp with a header image of a saree is well-suited. The store's loyal customers are likely already in the owner's WhatsApp contacts. This is the best-fit channel match in the pilot.

**Fresh Basket:** MODERATE FIT WITH GAPS. WhatsApp works for grocery but the timing and frequency requirements are different. Grocery customers expect offer communications at specific times (Thursday/Friday evenings for weekend planning). The problem is frequency: if Fresh Basket sends 4 campaigns/month, customers may find the WhatsApp messages intrusive. The opt-out risk is higher for grocery than for the other two businesses. Additionally, grocery customers are more price-driven than relationship-driven — the ROI of a polished AI-generated WhatsApp campaign vs. a simple text broadcast is harder to demonstrate.

### Missing channel consideration: Instagram / Facebook

The PRD explicitly excludes non-WhatsApp/email channels in v1. This is reasonable for the pilot. However, Shree Ladies Garments almost certainly already promotes on Instagram (posting saree photos is a standard practice for garments retailers in India). The absence of Instagram integration is not a blocker for v1 but will be a real objection when the owner asks "can this post to my Instagram too?" The product team should have a prepared answer for this.

### Email channel reality check

Email is irrelevant for all three pilot businesses in any practical sense. Indian consumers do not open promotional emails from local businesses. The email addresses of customers of these three businesses are near-non-existent (see Finding 3). The email channel will see near-zero usage in the pilot. This is not a problem — WhatsApp is the right primary channel — but the PRD should not present email and WhatsApp as equivalent options for these businesses. The email feature is infrastructure for future business types (B2B, education, corporate gifting) where email is expected.

---

## Finding 7 — MEDIUM: Success criteria SM-2 is achievable but inadequate as a proof of value

**Severity: MEDIUM**
**Affects:** All three pilot businesses

### SM-2 as stated
"Each pilot business publishes at least 2 Campaigns before free trial ends."

### Assessment

**Achievability:** Publishing 2 campaigns in a month is achievable for all three businesses IF:
1. Onboarding (Meta WhatsApp setup) completes successfully — this is a non-trivial blocker (see Finding 8 on Meta verification)
2. They have at least 20–30 contacts to send to
3. The admin is available and motivated during the pilot period

**AutoCare Services:** Achievable. Automotive owners are comfortable with mobile tech. If onboarded correctly, they can publish 2 campaigns without much hand-holding.

**Shree Ladies Garments:** Achievable, especially during the Navratri–Diwali window when there is natural promotional urgency.

**Fresh Basket:** Achievable for count, but the free trial cap of 3 campaigns is the constraint. They will hit 3 campaigns faster than the others but cannot go beyond — see Finding 2.

### Why SM-2 is inadequate as a value proof

Publishing a campaign is an output metric, not an outcome metric. A business owner who publishes 2 campaigns and sees a 5% read rate and 0 attributable sales will not convert to paid. The success metric should include at least one of:

- A customer mentioning the WhatsApp message in-store ("I saw your offer")
- A measurable spike in footfall, calls, or orders following a campaign
- The business owner voluntarily sending a 3rd campaign without being prompted

SM-6 (link click-through rate ≥ 10%) is a better proxy but it requires businesses to have a link to click through to — which Fresh Basket may not (grocery stores often don't have an ordering page; UJ-3 assumes a "store's ordering page" that may not exist for the pilot business).

### Recommendation

Add a qualitative success metric: "At least 2 of 3 pilot businesses report at least 1 customer referencing a campaign (in-store, by phone, or via WhatsApp reply) during the pilot period." Measure this via a simple weekly check-in with the pilot business owner.

---

## Finding 8 — MEDIUM: Missing features critical for Indian SMB cold-start

**Severity: MEDIUM**
**Affects:** All three pilot businesses

The following features are absent from the PRD and would materially affect pilot success:

### 8.1 Bulk CSV import of contacts (MISSING — HIGH PRIORITY)

All three businesses will start with contacts in spreadsheets, phone contacts exports, or basic billing software exports. There is no CSV/Excel import in the PRD. FR-8 covers manual add and FR-7 covers WhatsApp sync, but neither addresses bulk import. A business with 200 contacts cannot manually add them one by one. This is a day-one blocker for the pilot.

**Recommendation:** Add a FR for CSV import with field mapping (minimum: name, mobile; optional: email, city, loyalty tier). This is a standard feature in every competitor product and is expected by Indian SMBs.

### 8.2 Opt-in collection mechanism (MISSING — HIGH PRIORITY)

The PRD enforces opt-in correctly (FR-11, FR-37) but provides no way for businesses to *collect* opt-ins from customers who have not yet formally consented. For all three businesses, the answer today is "no customer has formally opted in." The platform cannot send a single WhatsApp message until opt-in is confirmed.

Without a first-party opt-in collection flow, the pilot cannot work as described. Options to add:
- A shareable opt-in link (e.g., `fineintegrity.com/optin/autocare`) that the business owner shares via their personal WhatsApp or puts on a physical card
- A QR code for the business to display at the counter/reception
- A WhatsApp "click to chat" link that captures a first-contact and records it as opt-in

**Recommendation:** Add a FR for opt-in collection (QR code + shareable link, minimum). Without this, the pilot businesses cannot build any list and the product cannot function.

### 8.3 Simple text-only WhatsApp message option (MISSING — MEDIUM PRIORITY)

The PRD focuses on AI-generated graphics-rich templates. But for Fresh Basket's "Weekend Vegetable Special," a simple text message like "🥦 Fresh vegetables in today! 20% off on all greens till Sunday — Fresh Basket" may outperform a designed template. Indian grocery customers are familiar with plain text WhatsApp broadcasts from local shops.

Meta WhatsApp Template requirements mean every message must be pre-approved, even plain text. But having a library of pre-approved simple text templates (non-AI-generated, human-written) would help businesses that want to move fast without the AI generation flow.

### 8.4 WhatsApp reply-to-book / reply-to-order (MISSING — MEDIUM PRIORITY)

The PRD explicitly excludes two-way conversation (§5, Non-Goals). However, for AutoCare Services, the most natural customer action is "reply to book a service slot." The CTA button links to a phone number, which is correct. But many automotive customers will just reply to the WhatsApp message with "book me for Saturday" and expect a response. The PRD says replies are visible in a read-only inbox (FR-43). This creates a notification gap: the admin sees replies in the platform inbox but the customer expects a reply in WhatsApp. The business will handle this manually but should be warned about this expectation mismatch.

### 8.5 Razorpay payment link CTA button (MISSING — MEDIUM PRIORITY)

The PRD notes Razorpay for billing. But for Fresh Basket, a "Pay Now" CTA that links to a Razorpay payment link or Google Pay UPI ID would be a high-value feature. Grocery pre-orders paid in advance have higher fulfillment rates. This could be a v2 feature but is worth noting as a gap that Fresh Basket will ask about.

---

## Finding 9 — MEDIUM: Competitive threat from free tools is real and underestimated

**Severity: MEDIUM**
**Affects:** All three pilot businesses

### The real competitors for these three businesses

The PRD names Zoko, Interakt, and WATI as competitors and correctly notes their pricing gap. But the actual competition in the pilot business context is not those tools — it is:

**1. WhatsApp Business broadcast lists (free, built-in)**
WhatsApp Business allows sending a broadcast message to up to 256 contacts. A garments shop owner can do this for free in 2 minutes: open WhatsApp, create a broadcast list, write a message, attach a JPG she designed in Canva, send. This is the real competitive benchmark. The value proposition against this is: (a) professional templates they couldn't create themselves, (b) Hindi AI-generated copy they don't have to write, (c) delivery tracking, (d) scheduling. Whether these are worth ₹1,800–₹3,600/month is the open question.

**2. Canva (free for basic) + WhatsApp Business broadcast**
Canva has Indian festival templates. A savvy garments shop owner already uses Canva to create Navratri sale graphics. She exports the JPG and sends via WhatsApp broadcast. This is zero cost and takes 15 minutes. The product needs to be demonstrably faster, better-looking, and more insightful than this combination.

**3. Instagram / Facebook (free, organic reach)**
Shree Ladies Garments and Fresh Basket are both natural users of Instagram/Facebook organic posts. These are free and reach an audience that self-selected to follow. WhatsApp campaigns reach an opt-in list — which is more valuable per contact — but building that list takes effort (see Finding 3, 8.2).

**4. Google My Business posts (free)**
For AutoCare Services, Google My Business is a relevant free channel. Automotive customers search locally. A "Monsoon special" Google My Business post reaches people actively searching — arguably higher purchase intent than a WhatsApp blast.

### The product's defensible differentiation

Against these free tools, the product's honest advantages are:
1. **AI copy generation in Hindi** — not available in Canva or WhatsApp Business
2. **Approval workflow** — valuable only if the business has an executive-admin separation (AutoCare might; the others may not)
3. **Delivery tracking and analytics** — genuinely not available in WhatsApp Business broadcast
4. **Scheduling** — moderately valuable; WhatsApp Business doesn't schedule
5. **Festival calendar context** — mild differentiation; Canva has festival templates too

The PRD should be honest internally that the defensible moat for these specific pilot businesses is narrow: **analytics + AI Hindi copy + scheduling**. Everything else is convenience.

### Recommendation

Design the pilot onboarding to demonstrate the analytics advantage on the very first campaign. The moment a business owner sees "138 of 142 people received this, 89 read it, 23 clicked" — that is something WhatsApp Business broadcast cannot show. Lead with analytics in the demo, not with AI generation.

---

## Finding 10 — LOW: Meta WhatsApp verification is a pilot-blocking operational dependency

**Severity: LOW (process risk, not product gap)**
**Affects:** All three pilot businesses

### The issue

The PRD correctly identifies Meta Business verification as an open question (§8, Q3): "How will the 3 pilot businesses be guided through Meta's WhatsApp Business Platform verification?"

In practice, Meta's WhatsApp Cloud API requires:
1. A Meta Business Manager account (can be new or existing)
2. Business verification (typically requires: business registration documents, address, website or Facebook page)
3. A dedicated phone number not currently registered with WhatsApp
4. Embedded signup flow (which the PRD addresses in FR-5)

For AutoCare Services, Shree Ladies Garments, and Fresh Basket:
- Most will NOT have a formal business registration (GST may or may not be registered, but a sole proprietor's GST registration should suffice)
- They likely do NOT have a website
- Their current number is probably already registered as a personal WhatsApp or WhatsApp Business account — it cannot be reused for the API without de-registering

**This process takes 2–7 business days after submission, with Meta's support being notoriously slow.**

If Fine Integrity is targeting a Navratri campaign window (October 2026), Meta verification must begin no later than September 1, 2026. Fine Integrity should provide hands-on, white-glove support for this step — it cannot be self-served by a garments shop owner.

### Recommendation

Assign one person at Fine Integrity to shepherd all three pilot businesses through Meta Business verification before the product build is complete. Start this process immediately after pilot agreement is signed — before the product is ready. The product build (estimated 8–12 weeks) provides exactly enough time if started now.

---

## Summary Table

| # | Finding | Severity | Businesses Affected | Action |
|---|---|---|---|---|
| 1 | Pricing ₹3/msg and ₹0.50/email is too high; willingness to pay is ₹500–₹1,500/month flat | CRITICAL | All 3 | Redesign pricing to flat monthly tiers |
| 2 | 3-campaign free trial is too thin for grocery; Fresh Basket needs ≥10 campaigns to see value | HIGH | Fresh Basket, AutoCare | Raise trial cap; differentiate by business type |
| 3 | Businesses are starting from near-zero customer lists; opt-in data and structured contacts don't exist | HIGH | All 3 | Add CSV import + opt-in collection flow |
| 4 | Meta WhatsApp free tier assumption is likely wrong; marketing template messages may not be free | HIGH | Fresh Basket primarily | Verify Meta pricing urgently; update cost basis |
| 5 | Festival calendar missing key occasions: Pre-Monsoon, Mango Season, Ganesh Chaturthi, Gudi Padwa | MEDIUM | AutoCare, Fresh Basket, Garments | Expand calendar to 25+ occasions with industry tags |
| 6 | Email channel has near-zero relevance for all 3 businesses; Fresh Basket's high-frequency needs risk opt-outs | MEDIUM | All 3 (email), Fresh Basket (frequency) | Set expectations; deprioritize email for pilot |
| 7 | SM-2 (2 campaigns published) is an output metric; no outcome metric tied to business results | MEDIUM | All 3 | Add qualitative outcome metric; fix SM-6 for no-website businesses |
| 8 | Missing: CSV import, opt-in collection (QR/link), plain-text templates | MEDIUM | All 3 | Add FRs before architecture phase |
| 9 | Real competition is free tools (WA broadcast + Canva); product's defensible advantage is narrow | MEDIUM | All 3 | Lead pilot demos with analytics, not AI generation |
| 10 | Meta WhatsApp verification takes 2–7 days and must start now for October pilot | LOW | All 3 | Assign white-glove support; begin verification now |

---

## What the PRD Gets Right (Strengths to Preserve)

- **Festival calendar concept:** Correct and commercially sound. Tying AI generation context to Diwali/Navratri is a meaningful differentiator vs. Canva's static templates.
- **Approval workflow:** Well-designed. The Executive → Admin flow is exactly right for small businesses where the owner (Admin) wants oversight but doesn't want to create every message themselves.
- **Hindi language support:** Critical for AutoCare Services (Pune), likely important for Shree Ladies Garments. Devanagari rendering on Android devices will be the make-or-break test.
- **WhatsApp-first design:** Correct channel choice for all three businesses.
- **Subdomain per business:** Professionally appealing onboarding experience for SMB owners who will feel legitimized by `autocare.fineintegrity.com`.
- **Branch architecture:** Foresighted for AutoCare if they have multiple service locations. Even for single-location businesses it cleanly organizes the data model.
- **Click tracking (FR-42):** This is the single most important analytics feature for SMBs. Seeing who clicked the CTA is actionable intelligence a business owner can act on (call those 23 people who clicked but didn't purchase).
- **Template reuse and clone (FR-22, FR-25):** Correct design. Indian SMBs will want to reuse last Diwali's template. The festival-to-festival clone workflow is a real time-saver.
- **Pilot timing (Navratri–Diwali window):** Excellent. This is the single highest-ROI promotional window for all three pilot businesses. Building urgency around this window is the right go-to-market trigger.

---

*Review completed: 2026-06-15. This review addresses market fit for Indian SMB pilot businesses only. Technical architecture and security reviews are separate workstreams.*
