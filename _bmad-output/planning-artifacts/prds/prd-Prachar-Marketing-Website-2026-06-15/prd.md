---
title: "Prachar Marketing Website"
status: final
created: 2026-06-15
updated: 2026-06-15
owner: Nilesh (Fine Integrity)
source_brief: "_bmad-output/planning-artifacts/briefs/brief-Campaign Promotion Automation-2026-06-15/brief.md"
source_product_prd: "_bmad-output/planning-artifacts/prds/prd-Campaign-Promotion-Automation-2026-06-14/prd.md"
---

# PRD: Prachar Marketing Website (fineintegrity.com)

## 0. Document Purpose

This PRD is for Nilesh (Fine Integrity) and any web developer, UX designer, or content writer building the Prachar marketing website. It defines what the site must do and contain — not how to build it. The source of truth for Prachar's product features and pricing is the product PRD (`prd-Campaign-Promotion-Automation-2026-06-14/prd.md`); this document references that PRD rather than duplicating it.

Features are grouped by page section. Functional Requirements (FR-1 through FR-26) carry stable IDs for downstream design and implementation tickets. `[ASSUMPTION]` tags mark inferred decisions requiring confirmation; the Assumptions Index (§8) collects them.

---

## 1. Vision

### Why This Website Exists

Prachar solves a real problem for Indian SMBs — but only if the right business owners can find it, understand it in under 30 seconds, and trust it enough to start a free trial without speaking to a salesperson. The marketing website at fineintegrity.com is the primary acquisition channel for v1.

The audience is skeptical: business owners and admins burned by software promises before, browsing on Android over 4G, deciding whether to engage before they finish their chai. The site earns trust by being specific (real pricing in INR, named festival seasons, real industry examples), fast (under 3 seconds on 4G), and risk-free (free trial, no credit card).

### What It Does

fineintegrity.com is a conversion-focused static marketing site with one purpose: sending qualified visitors to app.fineintegrity.com/register to start a free trial. The page scrolls from hero to pricing to sign-up CTA — no second page required. It is not a blog, a help centre, or a feature encyclopedia.

### Why October 2026

The Navratri–Dussehra–Diwali window (October–November) is the highest-revenue promotional season for all three pilot businesses. The site must be live and indexed before October 1, 2026, so a business searching "WhatsApp promotions for Diwali" can find Prachar organically or by referral before the season peaks.

---

## 2. Target Visitor

### 2.1 Jobs To Be Done

**Business Owner / Admin (primary)**
- Understand in 30 seconds whether Prachar is worth 5 minutes of my time
- See pricing in INR before I commit to anything
- Know that other Indian businesses like mine are already using it
- Start a free trial without giving my credit card details

**Business Executive (secondary)**
- Find a tool that saves an hour per promotion campaign
- Share the link with my boss in a way that makes the case for me — the site must work as a forwarded WhatsApp link, not just a search landing page

### 2.2 Non-Visitors (v1)

- End customers of pilot businesses (they receive WhatsApp/email campaigns but never visit fineintegrity.com)
- Enterprise marketing teams needing A/B testing, CRM integration, or custom SLAs
- Developers looking for API documentation or technical integration guides
- Non-Indian businesses

### 2.3 Key Visitor Journeys

**UJ-1. Rajan discovers Prachar via a WhatsApp forward and signs up.**
- **Persona + context:** Rajan owns a 2-branch salon in Pune. A friend sends him fineintegrity.com via WhatsApp: "check this for Diwali offers."
- **Entry state:** Opens the link on Android Chrome, no prior knowledge of Prachar, mid-morning.
- **Path:**
  1. Hero loads in under 3 seconds. Headline communicates value in plain language. Primary CTA is visible above the fold.
  2. Scrolls through How It Works (3 steps: Describe → AI Generates → Approve & Send). Recognises the flow.
  3. Sees the Features section — notices Indian Festival Calendar, WhatsApp + Email channels, Salons & Beauty as a named industry.
  4. Reaches Pricing — sees Free Trial, no credit card required. Taps "Start Free Trial" on the Starter tier.
  5. Redirected to app.fineintegrity.com/register with UTM parameters preserved.
- **Climax:** Rajan completes registration and is inside the product. The marketing site has done its job.
- **Resolution:** Prachar product onboarding takes over.
- **Edge case:** Rajan's 4G connection is slow. Critical content (hero headline + primary CTA) renders within 2.5 seconds even if images are still loading; the hero image does not block text render.

---

**UJ-2. Meena researches and forwards the pricing section to her boss.**
- **Persona + context:** Meena is an executive at a garments shop. She finds fineintegrity.com via Google: "WhatsApp bulk message Navratri offer India." It's September.
- **Entry state:** Desktop Chrome, organic search. Meena needs her boss's sign-off for any software spend.
- **Path:**
  1. Lands on homepage. Scrolls to Pricing.
  2. Sees ₹999/month Starter tier. Copies the page URL.
  3. Sends to boss via WhatsApp: "Sir dekho — ₹999/month mein Navratri campaign automatic. Free trial bhi hai."
  4. Boss opens on mobile. Sees pricing, taps "Start Free Trial."
  5. Redirected to app.fineintegrity.com/register.
- **Climax:** Boss (Business Admin) signs up from the forwarded link.
- **Resolution:** The site worked as a shareable artifact, not just a landing page. The pricing section URL with `#pricing` anchor renders correctly when opened directly.
- **Edge case:** The `#pricing` deep-link must scroll the homepage to the Pricing section on mobile Chrome; anchor navigation must work on first load, not only after full page render.

---

## 3. Glossary

- **Marketing Site** — The static website at fineintegrity.com. Distinct from the Prachar product, which lives at app.fineintegrity.com (registration/onboarding) and businessname.fineintegrity.com (per-tenant product).
- **Product** — The Prachar SaaS platform. Described fully in the product PRD.
- **Visitor** — A person who arrives at fineintegrity.com. Not yet a registered Prachar user.
- **User** — A registered Prachar account holder. Visitors become Users by completing registration at app.fineintegrity.com/register.
- **CTA (Call to Action)** — A button or link whose purpose is to move the visitor toward sign-up. Primary CTA label: "Start Free Trial". Secondary CTA label: "See Pricing".
- **Hero** — The first visible section of the homepage: headline, sub-headline, and primary CTA, fully visible above the fold on a 375px mobile viewport without scrolling.
- **Section** — A full-width horizontal band within the homepage. Order: Hero → How It Works → Features → Social Proof → Pricing → Final CTA → Footer.
- **Anchor link** — A URL with a `#section-id` fragment that scrolls the page to a named section (e.g., fineintegrity.com/#pricing).
- **UTM parameters** — URL query parameters (utm_source, utm_medium, utm_campaign, utm_content) appended to app.fineintegrity.com/register links to attribute sign-up source in analytics.
- **Core Web Vitals** — Google's page experience metrics: LCP (Largest Contentful Paint < 2.5s), CLS (Cumulative Layout Shift < 0.1), INP (Interaction to Next Paint < 200ms).
- **Static build** — Website compiled to plain HTML/CSS/JS files at build time; served without a server-side runtime. Content changes require a rebuild and redeploy. Headless CMS migration is planned post-launch.
- **Free Trial** — 1-month free access to Prachar with full features and up to 5 Campaigns per Branch. No credit card required. Defined fully in the product PRD §6.1.

---

## 4. Features

### 4.1 Site-Wide Layout and Navigation

Every page shares a consistent sticky header and footer. The header contains the Prachar logo, primary navigation links, and a primary CTA button; on mobile, navigation collapses into a hamburger menu. The footer contains legal links, contact, and any active social media links. Navigation links for homepage sections use anchor links. The site is a single-page scroll experience; /privacy-policy and /terms are the only separate pages. Realizes UJ-1, UJ-2.

#### FR-1: Sticky header
The header is fixed to the top of the viewport as the visitor scrolls. It contains: Prachar logo (links to homepage top), navigation links (Features, Pricing, Contact), and a "Start Free Trial" CTA button.

**Consequences (testable):**
- Header is visible at all scroll positions on homepage, /privacy-policy, and /terms.
- "Start Free Trial" in the header links to app.fineintegrity.com/register?utm_source=website&utm_medium=header&utm_campaign=free-trial.
- Logo click navigates to / (homepage top) from any page.
- Header height does not exceed 64px on mobile so it does not consume excessive viewport space.

#### FR-2: Mobile navigation
On viewports ≤768px, primary navigation links are hidden and a hamburger icon is shown. Tapping the icon opens a full-width overlay or slide-in drawer containing the navigation links and the "Start Free Trial" CTA.

**Consequences (testable):**
- Navigation links are not visible on mobile until the hamburger icon is tapped.
- The drawer is dismissible by tapping outside it or tapping a close (×) icon.
- All links in the mobile drawer function identically to their desktop equivalents.
- The drawer does not shift page content when it opens (uses overlay, not push layout).

#### FR-3: Footer
The footer contains: Prachar logo, one-line tagline, navigation links mirroring the header, Privacy Policy link (/privacy-policy), Terms of Service link (/terms), contact link (hello@fineintegrity.com), and social media icons for any active Fine Integrity profiles. `[ASSUMPTION: Fine Integrity confirms which social media profiles are active before launch; footer renders without social icons if none are confirmed.]`

**Consequences (testable):**
- /privacy-policy and /terms footer links open their respective standalone pages.
- Contact link opens the visitor's email client with hello@fineintegrity.com pre-populated.
- Footer is present and identical on homepage, /privacy-policy, and /terms.

#### FR-3A: Favicon and brand icons
The site includes a favicon and web app icons at standard sizes (16×16, 32×32, 180×180 Apple Touch Icon, and a 192×192 Android icon) so that bookmarks, browser tabs, and WhatsApp link previews display the Prachar brand mark rather than a browser default.

**Consequences (testable):**
- /favicon.ico is present and returns HTTP 200.
- Apple Touch Icon (180×180) and Android icon (192×192) are declared in `<head>` and resolve correctly.
- Browser tab displays the Prachar icon, not a blank or browser-default icon.
- WhatsApp link preview displays the og:image (FR-19), not a broken icon state.

---

### 4.2 Homepage — Hero Section

The hero is the first thing every visitor sees. It must communicate the value proposition and present the primary CTA entirely above the fold on a 375px mobile viewport. The hero contains a headline, a sub-headline, a primary CTA button, and a hero visual — a static image of a sample Prachar-generated campaign (festival motif, WhatsApp message UI frame, Hindi/bilingual text, branded design). Realizes UJ-1, UJ-2.

#### FR-4: Hero headline and sub-headline
The hero displays a primary headline (≤10 words) and a sub-headline (1–2 sentences, ≤25 words) communicating what Prachar does, for whom, and why it is different from manual WhatsApp broadcasts.

**Consequences (testable):**
- Headline and sub-headline are fully visible above the fold on a 375×667px viewport without scrolling.
- Headline renders at ≥28px on mobile; sub-headline at ≥16px.
- No section content below the primary CTA appears above the fold.

**Notes:** Suggested headline: *"Festival campaigns your customers actually open — in 5 minutes."* Sub-headline: *"AI-generated WhatsApp and email promotions for Indian businesses. No designer. No agency. Just describe your offer."* Final copy is a content/UX decision, not a PRD requirement.

**Copy guardrail — urgency:** The hero headline or sub-headline must name or strongly imply a specific upcoming seasonal moment (Navratri / Diwali / Eid / monsoon) relevant to the current calendar date at launch. Generic "festival promotions" without a specific moment does not satisfy the urgency barrier identified in the brief. Copy must be updated at each major seasonal transition (minimum twice yearly). `[ASSUMPTION: Nilesh or a designated owner updates hero copy ahead of each major seasonal window; this is not automated in the static build.]`

**Copy guardrail — honesty:** Hero copy must not claim AI superiority, proprietary algorithms, or technological moats. The honest pitch is speed and Indian-market specificity. Claims like "most advanced AI" or "cutting-edge machine learning" are prohibited.

#### FR-5: Primary CTA — hero
A primary CTA button in the hero: label "Start Free Trial" with sub-label "No Credit Card Required" (smaller text below or within the button).

**Consequences (testable):**
- Button links to app.fineintegrity.com/register?utm_source=website&utm_medium=hero&utm_campaign=free-trial.
- Minimum tappable touch target: 44×44px.
- Clicking/tapping navigates the browser to app.fineintegrity.com/register — no modal, no form on the marketing site.

#### FR-6: Hero visual
A static image (not video or animation in v1) showing a sample Prachar-generated campaign. The visual reinforces the value proposition: festival motif, WhatsApp message UI frame, bilingual (Hindi + English) promotional text, recognisable branded design.

**Consequences (testable):**
- Visual is present on desktop (alongside headline) and mobile (below the headline and CTA, or as a background).
- Image is served in WebP format with a JPEG fallback; displayed image weight ≤150KB.
- Image has descriptive alt text for screen readers.
- Image does not block LCP — text and CTA render before the image fully loads.

---

### 4.3 Homepage — How It Works Section

Three-step visual showing the Prachar workflow in plain language: (1) Describe your offer, (2) AI generates your campaign, (3) Approve and send. Breaks the comprehension barrier — translates "AI-powered campaign automation" into three actions the visitor already understands. Realizes UJ-1.

#### FR-7: Three-step process display
The section displays exactly three numbered steps in sequence. On desktop, steps are shown horizontally; on mobile, they stack vertically.

**Consequences (testable):**
- All three steps are visible without horizontal scrolling on any viewport ≥320px wide.
- Each step has a distinct icon or illustration, a bold one-line label, and a one-sentence description in outcome language.
- No step uses technical language (e.g., "Claude Haiku", "Meta WhatsApp Cloud API").

#### FR-8: How It Works CTA
A secondary CTA below the three steps links to app.fineintegrity.com/register?utm_source=website&utm_medium=how-it-works&utm_campaign=free-trial.

**Consequences (testable):**
- CTA button is present below the three steps.
- Link includes correct UTM parameters.

---

### 4.4 Homepage — Features Section

Five feature cards, each describing one product capability in outcome language. This is a reason-to-believe section — not a technical feature list. Each card has an icon, a bold title (≤6 words), and 1–2 outcome sentences (≤30 words). Realizes UJ-1.

#### FR-9: Five feature cards
Cards cover: (1) AI Campaign Generation, (2) Indian Festival Calendar, (3) WhatsApp + Email Delivery, (4) Approval Workflow, (5) Campaign Analytics. On desktop: 2- or 3-column grid. On mobile: vertical stack.

**Consequences (testable):**
- All five cards are present and legible at all viewport sizes ≥320px.
- No card contains implementation-level language (no mention of "Claude", "Azure", "Meta API", "Unsplash").
- No card claims AI superiority, proprietary technology, or algorithmic moats; differentiation is expressed through outcomes and Indian-market specificity (e.g., "Built for Indian festivals" not "Powered by advanced AI").
- At least one feature card implicitly contrasts with the manual-WhatsApp-broadcast status quo (e.g., "No designer. No agency.") or the named alternatives (Zoko, Interakt, WATI) without naming them negatively.
- Icons are SVG (scalable; ≤5KB each).

**Notes:** Suggested outcome-language titles and descriptions:
1. **AI writes it for you** — "Describe your promotion in English or Hindi. Get a professionally designed WhatsApp or email campaign in under 15 seconds."
2. **Built for Indian festivals** — "Diwali, Navratri, Eid, Holi, and 15+ more built in. AI picks the right design and tone automatically."
3. **WhatsApp + Email, one platform** — "Send to opted-in customers on both channels. See delivery, read rates, and clicks in one dashboard — no toggling between two tools."
4. **Approval before it goes out** — "Every campaign goes through your team's approval before it reaches a single customer. Full audit trail included."
5. **Know what's working** — "Delivery, open rates, and clicks — per customer, per campaign, exportable to CSV."

#### FR-10: Industry callout
Within or adjacent to the Features section, a representative list of 8–10 supported industries helps visitors self-identify. Content is static. `[ASSUMPTION: A curated subset of 8–10 industries (from the full 25 in the product PRD) is displayed; final selection is a content/UX decision.]`

**Consequences (testable):**
- At least 8 industry names are visible or scannable without navigating away from the page.
- Industry list content is static HTML; no API call at render time.

---

### 4.5 Homepage — Social Proof Section

One to three proof elements addressing the trust barrier. At launch, proof may be testimonial quotes (from pilot businesses, with explicit written permission), an industry/city callout ("Used by automotive services, garments shops, and grocery businesses in Maharashtra"), or a pilot partner logo block. No fabricated or generic social proof. Realizes UJ-1.

#### FR-11: Social proof block
At least one proof element is present between the Features and Pricing sections. Content is static. `[ASSUMPTION: At least one pilot business (AutoCare Services, Shree Ladies Garments, or Fresh Basket) grants explicit written permission to be named before October 1, 2026. If no permission is obtained, the section displays an industry/city callout. No fabricated testimonials are shipped under any circumstance.]`

**Consequences (testable):**
- At least one proof element is present and visible in this position.
- If testimonial quotes are used, each is attributed to a named person, business name, and city.
- Section renders correctly on mobile and desktop.

---

### 4.6 Homepage — Pricing Section

Transparent pricing table displaying all four Prachar tiers. Visitors who reach pricing are in active evaluation — this section must be scannable on mobile, show INR prominently, and show all tiers without any additional click. No "Contact Sales" option. A short FAQ below the table addresses the most common objections before the visitor reaches the CTA. Realizes UJ-1, UJ-2.

#### FR-12: Pricing table — four tiers
Four pricing cards: Free Trial (₹0 / 1 month), Starter (₹999/month), Growth (₹1,999/month), Business (₹3,499/month). Each card shows: tier name, monthly price in ₹, included WhatsApp message count, included email count, overage rates (WhatsApp per-message; email per-message), and a CTA button linking to app.fineintegrity.com/register.

**Consequences (testable):**
- All four tiers are displayed with pricing and volumes exactly matching the product PRD Appendix C.
- Pricing is displayed in ₹; no USD.
- On mobile (≤768px), cards stack vertically; all four are accessible without horizontal scrolling.
- Each card CTA links to app.fineintegrity.com/register with utm_source=website, utm_medium=pricing, utm_campaign=free-trial, utm_content={tier-slug} (free-trial / starter / growth / business).
- Growth tier (₹1,999/month) is visually distinguished as the recommended tier with a "Most Popular" badge. Starter and Business tiers have no badge.

#### FR-13: Pricing FAQ accordion
3–5 question accordion below the pricing table. Collapsed by default; each question expands on tap/click. Content is static.

**Consequences (testable):**
- FAQ is present below the pricing cards on all viewport sizes.
- Each question-answer pair expands and collapses independently.
- No API call or JavaScript framework required for accordion behaviour (CSS-only or minimal JS acceptable).

**Notes — suggested FAQ questions:**
1. Is the free trial really free? — Yes. No credit card, no auto-charge, all features for 1 month, up to 5 Campaigns per Branch.
2. What is a "Branch"? — A physical or logical unit of your business — each Branch has its own WhatsApp number, customer list, and billing.
3. What payment methods are accepted? — UPI and credit/debit card. `[NOTE FOR PM: Do not name Razorpay specifically until Razorpay is confirmed — see Open Question 4.]`
4. Are WhatsApp message costs included? — Meta charges approximately ₹1–1.25 per conversation separately. Your Prachar plan includes a monthly message quota; overages are charged at the rates shown above.
5. Can I cancel anytime? — Yes. No contracts, no lock-in. Cancel from your account settings.

---

### 4.7 Homepage — Final CTA Section

A full-width section immediately above the footer that restates the value proposition and presents the primary CTA one last time — for visitors who scrolled the full page without clicking. One headline, one sentence, one button.

#### FR-14: Final CTA block
A full-width section with: headline (≤8 words), one supporting sentence (≤20 words), and "Start Free Trial" CTA button.

**Consequences (testable):**
- Section is present between the Pricing section and the Footer on the homepage.
- CTA button links to app.fineintegrity.com/register?utm_source=website&utm_medium=final-cta&utm_campaign=free-trial.
- Section background is visually distinct from the Pricing section above it.

---

### 4.8 Contact

A lightweight contact option for visitors with pre-sales questions. v1 is a simple email link to hello@fineintegrity.com — no live chat, no contact form, no support ticketing. The link appears in the header navigation and the footer. `[ASSUMPTION: A standalone /contact page is out of scope for v1. Revisit after pilot if pre-sales enquiry volume warrants a form.]`

#### FR-15: Contact email link
A mailto:hello@fineintegrity.com link is accessible from the navigation and the footer on all pages.

**Consequences (testable):**
- Clicking/tapping the link opens the visitor's email client with hello@fineintegrity.com pre-filled in the To field.
- Link is present in both the header navigation and the footer.
- Link text is "Contact" or "hello@fineintegrity.com" (not a generic "Get in touch" button that hides the address).

---

### 4.9 Legal Pages

Two standalone pages required before public launch: Privacy Policy (/privacy-policy) and Terms of Service (/terms). Both are separate pages — not modals. Content is authored by Fine Integrity or a legal resource; this PRD defines structural and technical requirements only. `[ASSUMPTION: Legal content is authored and approved by Fine Integrity before October 1, 2026. Placeholder or "coming soon" content is not acceptable for public launch.]`

#### FR-16: Privacy Policy page
Standalone page at /privacy-policy. Linked from the footer on all pages.

**Consequences (testable):**
- /privacy-policy returns HTTP 200 with content.
- Page uses the same header and footer as the rest of the site.
- Footer link on homepage, /terms, and /privacy-policy itself points to /privacy-policy.

#### FR-17: Terms of Service page
Standalone page at /terms. Linked from the footer on all pages.

**Consequences (testable):**
- /terms returns HTTP 200 with content.
- Footer link on all pages points to /terms.

---

### 4.10 SEO and Metadata

Technical SEO requirements ensuring correct Google indexing, correct social sharing previews (WhatsApp, LinkedIn, Twitter), and organic discoverability by Indian SMBs. `[ASSUMPTION: SEO keyword research — identifying target queries such as "WhatsApp marketing for Indian business" or "Diwali promotion tool" — is a content/marketing task outside the scope of this PRD. This PRD covers technical requirements that enable SEO; keyword and content strategy are downstream.]`

#### FR-18: Title tags and meta descriptions
Every page has a unique `<title>` (≤60 characters) and `<meta name="description">` (≤160 characters) that accurately describe the page and include relevant keywords.

**Consequences (testable):**
- No two pages share the same title tag.
- All title tags ≤60 characters; all meta descriptions ≤160 characters.
- Homepage title tag contains "Prachar" and a primary keyword phrase.

#### FR-19: Open Graph and Twitter Card metadata
Every page includes full Open Graph tags (og:title, og:description, og:image at 1200×630px, og:url, og:type) and Twitter Card tags (twitter:card, twitter:title, twitter:description, twitter:image).

**Consequences (testable):**
- WhatsApp link preview of fineintegrity.com renders the og:title and og:image correctly (verifiable via WhatsApp's link preview before sending, or via opengraph.xyz).
- og:title for the homepage is ≤60 characters and communicates the value proposition clearly when read out of context (i.e., in a WhatsApp chat before the recipient taps the link — this is how UJ-2's boss first encounters Prachar).
- og:description for the homepage is ≤160 characters and includes the free trial hook.
- All OG and Twitter Card tags are valid and present on homepage, /privacy-policy, and /terms.
- og:image file weight ≤200KB; dimensions exactly 1200×630px.

#### FR-20: Structured data — SoftwareApplication schema
Homepage includes a `<script type="application/ld+json">` block with SoftwareApplication schema: applicationCategory (MarketingAutomation), operatingSystem (Web), offers (free trial), name (Prachar), and url.

**Consequences (testable):**
- Structured data validates without errors in Google's Rich Results Test tool.
- applicationCategory, offers, operatingSystem, name, and url fields are populated with correct values.

#### FR-21: Sitemap and robots.txt
/sitemap.xml lists all public pages. /robots.txt permits all crawlers and references the sitemap URL.

**Consequences (testable):**
- /sitemap.xml is accessible and lists homepage, /privacy-policy, /terms.
- /robots.txt contains `Sitemap: https://fineintegrity.com/sitemap.xml` and does not disallow any public page.

#### FR-22: Canonical URLs and redirects
Every page includes `<link rel="canonical">` pointing to its HTTPS non-www URL. HTTP and www variants redirect 301 to the HTTPS non-www canonical.

**Consequences (testable):**
- http://fineintegrity.com → 301 → https://fineintegrity.com.
- https://www.fineintegrity.com → 301 → https://fineintegrity.com.
- Canonical tag on homepage is `<link rel="canonical" href="https://fineintegrity.com/">`.

---

### 4.11 Analytics and Conversion Tracking

Google Analytics 4 is the analytics platform for v1. `[ASSUMPTION: GA4 is the chosen platform; no privacy-focused alternative (PostHog, Plausible) required at launch. Revisit if Indian PDPB compliance imposes data residency constraints on analytics vendors.]` All critical visitor actions are tracked as GA4 events so the success metrics in §6 are measurable from day one.

#### FR-23: GA4 integration
GA4 JavaScript snippet (or Google Tag Manager container) installed on all pages. Pageviews tracked automatically.

**Consequences (testable):**
- GA4 pageview events fire on homepage, /privacy-policy, and /terms.
- GA4 property is configured under a Fine Integrity Google account, not a personal account.
- GA4 script loads asynchronously and does not block page rendering.

#### FR-24: CTA click event tracking
Every "Start Free Trial" CTA click fires a GA4 custom event: event_name = `cta_click`, with parameters: cta_location (hero / header / how-it-works / pricing / final-cta / mobile-nav) and plan_tier (populated only for pricing-section CTAs: free-trial / starter / growth / business; null elsewhere).

**Consequences (testable):**
- GA4 DebugView shows `cta_click` events with correct cta_location for all CTA placements.
- plan_tier is populated for all four pricing-section CTA buttons and absent for all other CTAs.

#### FR-25: Scroll depth tracking
GA4 records scroll depth milestones on the homepage at 25%, 50%, 75%, and 90% of page height.

**Consequences (testable):**
- Scroll depth events fire and appear in GA4 reports within 24 hours of homepage sessions.
- Events are labelled with percent_scrolled parameter.

#### FR-26: UTM parameter passthrough
All CTA links to app.fineintegrity.com/register include UTM parameters (utm_source, utm_medium, utm_campaign, utm_content) per the UTM matrix in Appendix C. The Prachar product registration page must preserve these parameters through the sign-up flow for cross-domain attribution. `[ASSUMPTION: GA4 cross-domain measurement is configured to link fineintegrity.com and app.fineintegrity.com in the same GA4 property, so that completed sign-ups are attributed to the correct marketing source. This requires coordination between the marketing site build and the product build — confirm ownership before implementation (see Open Question 3).]`

**Consequences (testable):**
- UTM parameters are present on all CTA links leaving fineintegrity.com (verifiable in browser network tab).
- GA4 cross-domain measurement is configured so a session starting on fineintegrity.com continues into app.fineintegrity.com/register without creating a new session.

---

## 5. MVP Scope

**In scope:**
- Single-page homepage: Hero → How It Works → Features (5 cards) → Social Proof → Pricing (4 tiers + FAQ) → Final CTA → Footer
- Sticky header with navigation, primary CTA, and mobile hamburger menu
- /privacy-policy and /terms pages
- SEO: title tags, meta descriptions, Open Graph / Twitter Card tags, SoftwareApplication schema, sitemap.xml, robots.txt, canonical URLs, HTTP→HTTPS and www→non-www redirects
- GA4 integration: pageviews, cta_click events, scroll depth, UTM passthrough, cross-domain measurement with app.fineintegrity.com
- All CTA links to app.fineintegrity.com/register with UTM parameters per Appendix C
- Static HTML/CSS/JS build — no server-side runtime
- Azure Static Web Apps hosting on fineintegrity.com (Central India region) `[ASSUMPTION: Azure Static Web Apps is the hosting platform; aligns with Nilesh's Azure expertise and data-residency preference.]`
- HTTPS enforced; HTTP → HTTPS 301 redirect; www → non-www 301 redirect
- Mobile-first design; all sections fully operable on 375px viewport
- Core Web Vitals targets: LCP < 2.5s, CLS < 0.1, INP < 200ms on simulated 4G mobile

**Out of scope for v1:**
- Headless CMS — planned post-launch `[NOTE FOR PM: CMS migration is the most time-sensitive post-launch upgrade — pricing and social proof will need updates as the pilot progresses.]`
- Blog / content marketing — deferred until post-pilot
- Help documentation — lives in the Prachar product, not the marketing site
- Hindi language version — English only; the product generates campaigns in Hindi
- Video or animated demos — static hero visual is sufficient
- Live chat — pre-sales questions go to hello@fineintegrity.com
- Industry-specific landing pages — single homepage serves all industries
- A/B testing infrastructure
- In-app sign-up on the marketing site — registration happens at app.fineintegrity.com/register; no form, modal, or embedded flow on the marketing site
- Full PDPB/GDPR cookie consent and compliance audit — required before scaling beyond India
- Server-side rendering or dynamic personalisation

---

## 6. Success Metrics

**Primary**

- **SM-1:** Free trial sign-up conversion rate — ≥3% of unique homepage visitors click a CTA and complete registration at app.fineintegrity.com/register. Validates FR-5, FR-8, FR-12, FR-14, FR-24, FR-26. **⚠ Launch blocker dependency:** SM-1 is unmeasurable without GA4 cross-domain measurement between fineintegrity.com and app.fineintegrity.com (FR-26, Open Question 3). This must be resolved and verified before launch, not after.
- **SM-2:** Median time-to-first-CTA-click — ≤90 seconds from first page load to first CTA click. Validates FR-4, FR-5, FR-7. Measured via GA4 engagement time + cta_click event timestamp.

**Secondary**

- **SM-3:** Mobile (Android) bounce rate — ≤50% of Android/Chrome sessions result in immediate exit. Validates FR-6 (hero visual), FR-4 (headline clarity), platform NFRs.
- **SM-4:** Pricing section reach — ≥40% of homepage sessions scroll to or click the Pricing section. Validates FR-12, FR-25.
- **SM-5:** Indian Tier 1+2 city traffic share — ≥80% of sign-up-attributed sessions originate from Indian cities. Validates targeting, UTM attribution (FR-26).
- **SM-6:** Core Web Vitals — LCP < 2.5s at P75 on mobile in Google Search Console. Validates Appendix A NFRs and FR-6.

**Counter-metrics (do not optimise)**

- **SM-C1:** Page complexity creep — Do not add sections, animations, or third-party scripts that push LCP above 2.5s. Mobile load speed is a conversion driver for this audience, not a secondary concern.
- **SM-C2:** Scroll depth to pricing — Do not add content between the hero and pricing that pushes the Pricing section below three mobile scrolls. Pricing visibility is a precondition for UJ-2.

---

## 7. Open Questions

1. **Social proof content:** Will at least one pilot business (AutoCare Services, Shree Ladies Garments, Fresh Basket) grant explicit written permission to be named on the public site before October 1, 2026? If not, what is the fallback content for the Social Proof section?
2. **Legal content readiness:** Who authors the Privacy Policy and Terms of Service, and by what date? Both pages are required before public launch and block go-live.
3. **GA4 cross-domain measurement ownership ⚠ Launch blocker:** The sign-up flow crosses from fineintegrity.com to app.fineintegrity.com. GA4 cross-domain measurement must be configured and verified before launch or SM-1 is unmeasurable from day one. Who owns this — the marketing site build or the product build? (Ref: FR-26, SM-1.)
4. **Payment gateway naming in FAQ:** The Pricing FAQ (FR-13) references "UPI and card." Razorpay should be named once confirmed. Has Razorpay been confirmed? (Ref: product PRD Open Question 2.)
5. **fineintegrity.com domain control:** Is fineintegrity.com registered and DNS-controlled by Fine Integrity? Who is the registrar? DNS control is required to point the domain to Azure Static Web Apps.
6. **Highlighted pricing tier:** ~~Resolved~~ — Growth (₹1,999/month) is the "Most Popular" highlighted tier. Confirmed by Nilesh 2026-06-15.
7. **app.fineintegrity.com readiness:** All marketing site CTAs redirect to app.fineintegrity.com/register. This subdomain must be live before the marketing site launches. What is the confirmed readiness date?
8. **Hero visual production owner and timeline:** FR-6 requires a static image showing a real-looking Prachar-generated campaign (festival motif, WhatsApp UI frame, bilingual text). Who produces this, and by what date? This is often the longest-lead asset on a marketing site — it may depend on the product being functional enough to generate a real sample campaign. Confirm owner and deadline before UX begins.
9. **Cookie consent / India DPDP Act 2023:** GA4 collects IP addresses and device identifiers from day one. India's Digital Personal Data Protection Act 2023 applies. The current PRD defers "full compliance" to post-launch, but does not commit to even a minimal consent notice. Before launch: does Fine Integrity's legal position require any cookie/data-collection notice on fineintegrity.com? Confirm with Nilesh or a legal resource before go-live.

---

## 8. Assumptions Index

| FR / Section | Assumption | Action required if wrong |
|---|---|---|
| §3 | v1 is a pure static build; headless CMS post-launch. | — |
| FR-3 | Footer social icons optional; omit if no active profiles confirmed. | Fine Integrity to confirm before launch. |
| FR-11 | At least one pilot business grants written permission to be named. | Fall back to industry/city callout; no fabricated testimonials. |
| FR-12 | Growth (₹1,999) is the "Most Popular" highlighted tier. | Confirmed by Nilesh 2026-06-15. |
| FR-15 | Standalone /contact page is out of scope for v1. | Revisit post-pilot if enquiry volume warrants a form. |
| FR-16/17 | Legal pages authored by Fine Integrity before launch; placeholder not acceptable. | Blocks go-live — see Open Question 2. |
| FR-23 | GA4 is the analytics platform for v1. | — |
| FR-26 | GA4 cross-domain measurement links fineintegrity.com and app.fineintegrity.com. | Requires coordination between site build and product build — see Open Question 3. |
| §4.10 | SEO keyword research is a content/marketing task, out of scope for this PRD. | — |
| §5 | Azure Static Web Apps (Central India region) is the hosting platform. | — |

---

## Appendix A: Cross-Cutting NFRs

| Attribute | Requirement |
|---|---|
| LCP (mobile, 4G) | < 2.5s at P75 (Google Core Web Vitals pass threshold) |
| CLS | < 0.1 (no layout shift after initial render) |
| INP | < 200ms |
| Critical path page weight | < 500KB total transferred on first load |
| Image format | WebP primary; JPEG/PNG fallback for browsers without WebP support |
| Hero image weight | ≤150KB at display size |
| HTTPS | All traffic TLS 1.2+; HTTP → HTTPS 301 |
| www redirect | www.fineintegrity.com → 301 → fineintegrity.com |
| Mobile viewport | All content and interactions operable at 375px minimum width |
| Touch targets | All tappable elements ≥44×44px |
| Typography | One typeface for the whole site (two at most). `[ASSUMPTION: Final typeface selection is a UX/design decision; the PRD requires it be a system font stack or a single preloaded web font — no runtime font API calls that block render.]` |
| Font loading | Web font (if used) preloaded via `<link rel="preload">`; no FOUT (Flash of Unstyled Text) on mobile; font-display: swap on all @font-face declarations |
| Accessibility | WCAG 2.1 AA: colour contrast ≥4.5:1 for body text, ≥3:1 for large text; keyboard navigable; all images have alt text; form elements labelled |
| Build output | Static HTML/CSS/JS; no server-side runtime dependency at serve time |
| Hosting | Azure Static Web Apps, Central India region |
| CDN | Azure CDN or Azure Static Web Apps built-in global edge distribution |
| Deploy time | Rebuild and redeploy completes in < 5 minutes for content-only changes |

---

## Appendix B: Page Structure

| URL | Page | Type |
|---|---|---|
| / | Homepage | Single-page scroll: Hero → How It Works → Features → Social Proof → Pricing → Final CTA → Footer |
| /privacy-policy | Privacy Policy | Standalone static page |
| /terms | Terms of Service | Standalone static page |
| /sitemap.xml | XML Sitemap | Auto-generated at build |
| /robots.txt | Robots file | Static |
| /* (all other paths) | 404 | Static custom 404 page with navigation back to homepage. Do not redirect all 404s to / — this creates soft-404s that Google treats as indexing errors. |

---

## Appendix C: UTM Parameter Matrix

| CTA Location | utm_source | utm_medium | utm_campaign | utm_content |
|---|---|---|---|---|
| Header "Start Free Trial" | website | header | free-trial | — |
| Hero primary CTA | website | hero | free-trial | — |
| How It Works CTA | website | how-it-works | free-trial | — |
| Pricing — Free Trial tier | website | pricing | free-trial | free-trial |
| Pricing — Starter tier | website | pricing | free-trial | starter |
| Pricing — Growth tier | website | pricing | free-trial | growth |
| Pricing — Business tier | website | pricing | free-trial | business |
| Final CTA section | website | final-cta | free-trial | — |
| Mobile nav drawer | website | mobile-nav | free-trial | — |
