---
name: "Prachar Marketing Website — Experience Spine"
status: final
created: 2026-06-15
updated: 2026-06-15
sources:
  - "_bmad-output/planning-artifacts/prds/prd-Prachar-Marketing-Website-2026-06-15/prd.md"
  - "_bmad-output/planning-artifacts/briefs/brief-Campaign Promotion Automation-2026-06-15/brief.md"
visual_reference: "DESIGN.md (same directory) — all color, type, spacing, and icon tokens live there; this file cross-references by token name only"
---

# EXPERIENCE.md — Prachar Marketing Website (fineintegrity.com)

This document is the behavioral spine for the Prachar marketing website. It defines how the site feels, flows, and speaks — not how it looks. Visual specifications (color, type scale, spacing, icons) live in DESIGN.md and are referenced here by token name only (e.g., `{colors.primary}`). Implementation tickets derive from the FR-IDs in the PRD.

**Single mandate:** Convert a skeptical Indian SMB owner or admin into a free-trial sign-up at app.fineintegrity.com/register. Every behavioral decision serves that outcome or is removed.

---

## 1. Foundation

### 1.1 Product North Star

Prachar is an AI-powered WhatsApp + Email campaign automation SaaS built for Indian small and medium businesses. The marketing site exists for one reason: to send a qualified visitor to app.fineintegrity.com/register to start a free trial. It is not a blog, a feature encyclopedia, or a help centre.

The audience is skeptical — owners and admins who have been burned by software promises before, browsing on an Android phone over a 4G connection, deciding whether to engage before they finish their chai. Trust comes from specificity (real INR pricing, named festival seasons, real industry examples), speed (under 2.5 seconds on 4G), and risk removal (free trial, no credit card required).

### 1.2 Design Principles

**1. Outcome language always.** Every word on the page is from the visitor's world, not the product's internals. "AI writes it for you" — not "Claude Haiku generates content." "Know what's working" — not "Campaign analytics dashboard." "Built for Indian festivals" — not "Powered by advanced ML." This is not a style preference; it is a trust mechanism for an audience that distrusts technical jargon.

**2. Speed is a feature.** The primary user (Rajan, Android Chrome, 4G) experiences the page at 4G latency on mid-range hardware. Every interaction, render, and asset decision is made with 375px and a mobile data connection as the primary constraint. Desktop enhancements are additive; mobile performance is non-negotiable.

**3. One outcome, no detours.** The page has one action it wants visitors to take. Navigation, content, and copy exist to move visitors toward that action or to remove barriers to it. Anything that does not serve conversion is absent.

**4. Specificity earns trust.** Generic claims ("best-in-class", "enterprise-grade", "advanced AI") are prohibited. Specific claims ("Diwali, Navratri, Eid, Holi, and 15+ more built in", "₹999/month", "Free trial — no credit card") build trust because they are falsifiable and concrete.

**5. Risk-free positioning is structural.** "No Credit Card Required" is not optional copy — it appears as a sub-label on every primary CTA. The free trial framing removes the single largest objection before the visitor can form it.

### 1.3 Platform Constraints

- **Static build only.** HTML/CSS/JS compiled at build time; no server runtime. Content changes require a rebuild and redeploy. Behavioral patterns must work within these constraints (CSS-only accordions, anchor-scroll via fragment, no server-side personalisation).
- **Hosting:** Azure Static Web Apps, Central India region.
- **Primary viewport:** 375px width (Android Chrome). All sections, interactions, and CTAs must be fully operable at this size.
- **Desktop reference:** 1280px.
- **No UI library.** Custom design system only; no shadcn, MUI, Tailwind component library, or third-party component framework.
- **Language:** English v1. Hindi website content is out of scope; the language selector is present as infrastructure for the v1 nav but Hindi is disabled ("Coming Soon").

---

## 2. Information Architecture

### 2.1 Surface Map

| Surface | URL | Type | Purpose |
|---|---|---|---|
| Homepage | / | Single-page scroll | Primary conversion surface. All sign-up intent is captured here. |
| Privacy Policy | /privacy-policy | Standalone static page | Legal requirement; shared header and footer. |
| Terms of Service | /terms | Standalone static page | Legal requirement; shared header and footer. |
| 404 | /* (unmatched paths) | Static custom page | Graceful error recovery with navigation back to /. Not a redirect to /. |
| XML Sitemap | /sitemap.xml | Auto-generated at build | Google indexing signal. |
| Robots file | /robots.txt | Static | Permits all crawlers; references sitemap URL. |
| Language selector state | — (UI state in header) | Dropdown overlay | English (active), हिन्दी (disabled, "Coming Soon"). Infrastructure for future Hindi site. |

### 2.2 Homepage Section Order

The homepage is a single-page scroll. Section order is fixed and optimized for the conversion narrative. Deviation requires justification against conversion impact.

```
Sticky Header (64px, always visible)
  │
  ▼
Hero
  → Headline + sub-headline + primary CTA (above fold at 375px)
  → Hero visual (festival campaign mockup)
  │
  ▼
How It Works
  → 3-step workflow (Describe → AI Generates → Approve & Send)
  → Secondary CTA
  │
  ▼
Features
  → 5 feature cards (reason-to-believe)
  → Industry callout (8–10 industries, static)
  │
  ▼
Social Proof
  → At least 1 proof element (testimonial or industry/city callout)
  │
  ▼
Pricing  ← anchor: #pricing (must work on first mobile load — UJ-2)
  → 4 pricing tiers (vertical stack on mobile)
  → Growth tier "Most Popular" badge
  → FAQ accordion (3–5 items, CSS-only or minimal JS)
  │
  ▼
Final CTA
  → Restatement headline + primary CTA (for visitors who scrolled the full page)
  │
  ▼
Footer
  → Logo, tagline, nav links, Privacy Policy, Terms, Contact
```

### 2.3 Navigation Structure

**Header (sticky, 64px):**
- Prachar logo (links to / top)
- Navigation: Features | Pricing | Contact
- Primary CTA: "Start Free Trial"
- Language selector: globe icon + "EN ▾"
- Mobile (≤768px): hamburger icon; navigation collapses into overlay drawer

**Footer:**
- Prachar logo + one-line tagline
- Navigation links (mirrors header)
- Legal: Privacy Policy | Terms of Service
- Contact: hello@fineintegrity.com
- Social icons (only if active profiles confirmed before launch; omit if none)

**Language selector dropdown:**
- English — active state, saffron checkmark (`{colors.accent.saffron}`)
- हिन्दी — disabled state, "Coming Soon" label, visually muted

---

## 3. Voice and Tone

### 3.1 Core Voice

Prachar speaks like a knowledgeable business associate who understands Indian SMB life — direct, specific, and respectful of the visitor's time and intelligence. Not casual. Not corporate. The voice trusts the visitor to make a good decision when given real information.

### 3.2 Microcopy Rules (non-negotiable)

**Outcome language only.** Every description of a product capability must be expressed as a visitor outcome, not a technical feature. Apply the test: "Can a garments shop owner in Nagpur picture this happening for their business?" If not, rewrite.

| Prohibited | Permitted |
|---|---|
| "Claude Haiku generates content" | "AI writes it for you" |
| "Meta WhatsApp Cloud API delivery" | "Sent directly to your customers on WhatsApp" |
| "Powered by advanced machine learning" | "Built for Indian festivals — Diwali, Eid, Holi, and 15+ more" |
| "Our proprietary AI models" | "Describe your offer in English or Hindi" |
| "Most advanced campaign automation" | "In under 15 seconds" |

**No AI superiority claims.** Do not claim AI moats, proprietary algorithms, or technological superiority. The honest pitch is speed and Indian-market specificity.

**Festival urgency is mandatory in the hero.** The hero headline or sub-headline must name or strongly imply a specific upcoming seasonal moment (Navratri, Diwali, Eid, monsoon) relevant to the launch date. "Festival promotions" without a specific named moment does not satisfy this requirement. Refresh this copy at each major seasonal transition — minimum twice yearly.

**"No Credit Card Required" on every primary CTA.** This sub-label appears on every instance of the "Start Free Trial" button: hero, pricing cards, and final CTA section. It is not optional. It is the primary objection-removal mechanism.

**Pricing in ₹ only.** No USD anywhere on the site. No currency conversion. No "from $X" framing.

**WhatsApp opt-out instruction required.** If a sample WhatsApp message appears anywhere on the site (hero visual, feature screenshots, etc.), it must include a STOP/opt-out instruction visible in the sample (e.g., "Reply STOP to unsubscribe"). This is both a legal signal and a trust signal to potential customers who are themselves WhatsApp users.

### 3.3 Seasonal Copy Freshness Obligation

The hero headline is the highest-leverage copy on the site. Its urgency depends on naming a seasonal moment the visitor cares about right now.

| Season | Window | Hero copy focus |
|---|---|---|
| Navratri–Diwali | October–November | Named priority; site must be live and indexed before October 1 |
| New Year | December–January | New Year offer campaigns |
| Valentine / Holi | February–March | Gifting / seasonal celebration |
| Eid / summer | April–June | Seasonal transition |
| Independence Day / Raksha Bandhan | July–August | Patriotic / family themes |
| Navratri (return) | September–October | Refresh for the peak window |

**Owner:** Nilesh or a designated content owner must update hero copy before each major seasonal window. This is not automated in the static build. Stale copy reduces urgency and measurably harms conversion.

---

## 4. Component Patterns

### 4.1 Sticky Header (FR-1, FR-2, FR-3A)

**Behavior:** Fixed to the viewport top at all scroll positions on all pages. Never hides on scroll-down or re-reveals on scroll-up — always present.

**Height:** 64px maximum on mobile. On desktop, 64px or up to 80px if visual density warrants.

**Logo:** Prachar wordmark (Latin "Prachar" + देवनागरी "प्रचार" subtitle or integrated). Links to / from all pages. At favicon sizes, the brand mark must be readable; at 16×16 and 32×32, use a monogram if the wordmark is illegible.

**Desktop layout:** Logo left — nav links centre — language selector + CTA right.

**Mobile layout (≤768px):** Logo left — hamburger icon right. Navigation links hidden until hamburger is tapped.

**Language selector:** Globe icon + "EN ▾" trigger. On tap/click, opens a compact dropdown (not a modal, not a full-screen overlay) showing: English (active, saffron checkmark), हिन्दी (disabled, muted, "Coming Soon" inline). Clicking English dismisses the dropdown with no action. Clicking हिन्दी does nothing (disabled). Clicking outside closes the dropdown.

**CTA in header:** "Start Free Trial" — UTM: `utm_source=website&utm_medium=header&utm_campaign=free-trial`. Touch target ≥44×44px. On mobile, this CTA may be omitted from the always-visible header to preserve space, but must appear inside the hamburger drawer.

### 4.2 Mobile Navigation Drawer (FR-2)

**Trigger:** Hamburger icon (≥44×44px touch target). Transforms to an × close icon when the drawer is open.

**Layout:** Full-width overlay, slides in from the right or drops from the top. Does not push page content. Background content is dimmed but not scrollable while the drawer is open.

**Contents:** Navigation links (Features, Pricing, Contact) + "Start Free Trial" CTA + language selector state.

**Dismissal:** Tap outside the drawer, tap the × close icon, or tap a navigation link (which also navigates).

**Anchor links in drawer:** Features, Pricing, Contact are anchor links to homepage sections (#features, #pricing). When tapped from any page, they navigate to / first, then scroll to the section. Drawer closes on link tap.

### 4.3 Hero Section (FR-4, FR-5, FR-6)

**Above-fold requirement (375×667px, no scroll):** Headline + sub-headline + primary CTA button (including "No Credit Card Required" sub-label) must be fully visible without scrolling. No content from below-fold sections may be visible above the fold.

**Headline:** ≤10 words. Outcome language. Names a specific seasonal moment. Renders at ≥28px on mobile.

**Sub-headline:** 1–2 sentences, ≤25 words. Renders at ≥16px on mobile.

**Primary CTA button:**
- Label: "Start Free Trial"
- Sub-label (smaller text, same button): "No Credit Card Required"
- Touch target: ≥44×44px
- UTM: `utm_source=website&utm_medium=hero&utm_campaign=free-trial`
- On tap/click: navigates directly to app.fineintegrity.com/register. No modal, no inline form, no intermediate step.

**Hero visual (FR-6):**
- Static image (no video, no animation in v1)
- Shows a sample Prachar-generated campaign: festival motif, WhatsApp message UI frame, bilingual (Hindi + English) promotional text, branded design
- If a WhatsApp message is shown, a STOP/opt-out instruction must be visible in the sample
- Format: WebP primary, JPEG fallback; displayed weight ≤150KB
- Alt text: descriptive, communicates the campaign content shown
- **LCP critical rule:** The image must not block text or CTA render. Image loads asynchronously after text and CTA are painted. On mobile, hero image appears below the CTA, not as a full-bleed background behind the headline. On desktop, image sits alongside the headline in a split or contained visual panel.

### 4.4 How It Works Section (FR-7, FR-8)

**Three steps, fixed labels:**
1. Describe your offer
2. AI generates your campaign
3. Approve and send

Each step: distinct SVG icon (≤5KB), bold one-line label, one outcome sentence (≤20 words, no technical language).

**Layout:**
- Desktop: horizontal row with directional connectors (arrows or numbered sequence)
- Mobile: vertical stack, full width, top-to-bottom

No horizontal scrolling at any viewport ≥320px wide.

**Secondary CTA below steps:** UTM `utm_source=website&utm_medium=how-it-works&utm_campaign=free-trial`. Same "No Credit Card Required" sub-label.

### 4.5 Features Section (FR-9, FR-10)

**Five feature cards:** Each card — SVG icon (≤5KB), bold title (≤6 words), 1–2 outcome sentences (≤30 words total).

| Card | Title direction | Outcome focus |
|---|---|---|
| 1 | AI writes it for you | Speed; describe in English or Hindi; under 15 seconds |
| 2 | Built for Indian festivals | Named festivals (Diwali, Navratri, Eid, Holi, 15+); automatic design and tone |
| 3 | WhatsApp + Email, one platform | Both channels; single dashboard; no toggling |
| 4 | Approval before it goes out | Team approval before send; full audit trail |
| 5 | Know what's working | Delivery, open rates, clicks; per campaign; exportable |

**Prohibited in all cards:** Claude, Azure, Meta API, Unsplash, any AI superiority claim, any technical moat claim.

**At least one card must contrast with the status quo** of manual WhatsApp broadcasts — e.g., "No designer. No agency." without naming Zoko, Interakt, or WATI negatively.

**Layout:**
- Desktop: 2–3 column grid
- Mobile: vertical stack, full width

**Industry callout (FR-10):** Within or immediately adjacent to the Features section. 8–10 industry names, static HTML. Helps visitors self-identify. Examples drawn from the PRD's 25 supported industries: Salons & Beauty, Garments & Fashion, Automotive Services, Grocery & Supermarkets, Restaurants & Food, Jewellery, Pharmacies, Electronics, Education & Coaching, Travel & Tours.

### 4.6 Social Proof Section (FR-11)

**Position:** Between Features and Pricing. Must not be omitted — at minimum, an industry/city callout is required.

**Proof hierarchy (use whichever has explicit written permission):**
1. Named testimonial quote: attributed person + business name + city + optional photo
2. Pilot partner logo block (with written permission)
3. Industry/city callout: "Used by automotive services, garments shops, and grocery businesses in Maharashtra" (no individual attribution required)

**Absolute prohibition:** No fabricated testimonials. No generic "thousands of happy customers" without attribution. If no pilot business grants permission before launch, use option 3.

**Mobile:** Proof elements render at full width, legible, no truncation.

### 4.7 Pricing Section (FR-12, FR-13)

**Anchor:** `id="pricing"` on the section element. Fragment navigation to `/#pricing` must scroll correctly on mobile Chrome on first page load — not only after full page render. (This is the climax requirement for UJ-2.)

**Four pricing tiers:**

| Tier | Price | Distinction |
|---|---|---|
| Free Trial | ₹0 / 1 month | Entry point; all features; up to 5 Campaigns per Branch |
| Starter | ₹999/month | — |
| Growth | ₹1,999/month | "Most Popular" badge; visually distinguished |
| Business | ₹3,499/month | — |

Each tier card shows: tier name, monthly price in ₹, included WhatsApp message count, included email count, overage rates (per-message for both channels), CTA button.

**"Most Popular" badge on Growth tier only.** The badge uses `{colors.accent.saffron}` or `{colors.primary}` per DESIGN.md and is visually prominent without being aggressive.

**CTA buttons per tier:**
- Free Trial: UTM `utm_content=free-trial`
- Starter: UTM `utm_content=starter`
- Growth: UTM `utm_content=growth`
- Business: UTM `utm_content=business`
- All share: `utm_source=website&utm_medium=pricing&utm_campaign=free-trial`
- "No Credit Card Required" sub-label on every pricing CTA button

**Layout:**
- Mobile (≤768px): vertical stack, one card per row, full width. All four cards accessible without horizontal scrolling.
- Desktop: 4-column grid or 2×2 grid with Growth tier visually elevated

**Pricing in ₹ only.** No USD, no conversion note, no "from X."

**FAQ accordion (FR-13):**

Position: below pricing cards, above Final CTA section.

Default state: all questions collapsed.

Interaction: tap/click a question to expand its answer; tap again to collapse. Questions expand and collapse independently. CSS-only or minimal vanilla JS — no JavaScript framework required.

Suggested questions (copy is a content decision):
1. Is the free trial really free?
2. What is a "Branch"?
3. What payment methods are accepted? (Do not name Razorpay until confirmed — see PRD Open Question 4)
4. Are WhatsApp message costs included?
5. Can I cancel anytime?

### 4.8 Final CTA Section (FR-14)

**Position:** Between Pricing (including FAQ) and Footer.

**Content:** Headline (≤8 words) + one supporting sentence (≤20 words) + "Start Free Trial" button with "No Credit Card Required" sub-label.

**Purpose:** Captures visitors who scrolled the full page without clicking. No new information — just restatement and a clear action.

**Background:** Visually distinct from the Pricing section directly above (different background color per DESIGN.md tokens — could be `{colors.primary}` or a contrasting surface).

**UTM:** `utm_source=website&utm_medium=final-cta&utm_campaign=free-trial`

### 4.9 Footer (FR-3, FR-15)

**Contents:** Prachar logo + one-line tagline | navigation links (Features, Pricing, Contact) | Privacy Policy (/privacy-policy) | Terms of Service (/terms) | Contact: hello@fineintegrity.com | social icons (only if active profiles confirmed before launch)

**Contact link:** Must render the email address visibly — not "Get in touch" behind a hidden mailto. Link text: "Contact" or "hello@fineintegrity.com". Opens the visitor's email client with hello@fineintegrity.com pre-populated.

**Identical on all pages:** Homepage, /privacy-policy, /terms.

### 4.10 Legal Pages (FR-16, FR-17)

Both pages use the same sticky header and footer as the rest of the site. Content is static. Placeholder or "coming soon" content is not acceptable for public launch. Both pages must be authored, approved, and live before go-live.

### 4.11 Custom 404 Page

Static page served at all unmatched paths (/* not otherwise defined). Not a redirect to / — redirecting all 404s to / creates soft-404s that Google treats as indexing errors.

Contents: Prachar logo/header, friendly error message, link back to / (homepage), footer. Same visual language as the rest of the site. Clarity and navigation back to / are the only goals.

### 4.12 Favicon and Brand Icons (FR-3A)

Declared in `<head>` on every page:
- /favicon.ico (16×16, 32×32)
- Apple Touch Icon: 180×180px
- Android icon: 192×192px

At small sizes (16×16, 32×32), use a monogram mark if the wordmark is illegible. WhatsApp link preview relies on og:image (FR-19), not the favicon — but a missing favicon creates a blank browser tab that reduces perceived quality.

---

## 5. State Patterns

### 5.1 Page Load State

**Critical path behavior:** On first load at 375px over a 4G connection, the hero headline, sub-headline, and primary CTA button must be painted before the hero image fully loads. The hero image is not on the critical rendering path.

**Font loading:** Declare with `font-display: swap` so the system fallback renders immediately. Preload via `<link rel="preload">` in `<head>`. No FOUT on mobile.

**Image loading:** Hero image uses `loading="lazy"` or is deferred until after above-fold content renders. All below-fold images use `loading="lazy"`.

### 5.2 Scroll State

**Sticky header:** Always visible. No scroll-triggered hide/show behavior.

**Anchor scroll (#pricing, #features, etc.):** Fragment navigation must trigger section scroll on first page load, not only after full page render. The `#pricing` anchor is a UJ-2 requirement — the boss opens `fineintegrity.com/#pricing` on mobile and must land at the pricing section, not the page top.

**Scroll depth tracking:** GA4 fires scroll depth events at 25%, 50%, 75%, and 90% of homepage scroll height. These are measurement events with no visual feedback to the visitor.

### 5.3 Hamburger Menu State

| State | Trigger | Visual |
|---|---|---|
| Closed | Default | Hamburger icon visible; nav links hidden |
| Open | Tap hamburger | Overlay appears; icon changes to ×; background content dimmed |
| Closing | Tap ×, tap outside, or tap a nav link | Overlay animates out; background content returns to full opacity |

Overlay does not push page content. Page scroll is locked while drawer is open on mobile.

### 5.4 Language Selector State

| State | Visual | Behavior |
|---|---|---|
| Closed | Globe icon + "EN ▾" | Default; dropdown hidden |
| Open | Dropdown visible | Shows English (active, saffron checkmark) + हिन्दी (disabled, "Coming Soon") |
| English selected | Checkmark; dropdown closes | No action (already active) |
| हिन्दी selected | Disabled; no action | Cursor: not-allowed; "Coming Soon" label |

### 5.5 FAQ Accordion State

| State | Default | On tap/click |
|---|---|---|
| All items | Collapsed (question visible, answer hidden) | Clicked item expands; other items unchanged |
| Expanded item | Answer visible | Tap header again to collapse |

Each question-answer pair is independent. Opening one does not close another. Implemented with CSS `:checked` trick (hidden checkbox) or minimal vanilla JS — no framework dependency.

### 5.6 Pricing Tier State

Growth tier card carries a persistent "Most Popular" badge — not a hover state, not a selected state. It is always visible. Free Trial, Starter, and Business cards have no badge at any state.

### 5.7 CTA Hover / Focus State

All CTA buttons:
- `:hover` (desktop): visual feedback per DESIGN.md (background color shift or shadow elevation)
- `:focus-visible` (keyboard): visible outline (`{colors.focus}` or equivalent with ≥3:1 contrast against surrounding background) — never `outline: none` without a replacement
- `:active`: pressed state feedback

On touch devices, `:hover` is suppressed or handled via JS touch detection to avoid sticky hover states on mobile.

---

## 6. Interaction Primitives

### 6.1 Touch Targets

All tappable and clickable elements ≥44×44px. This applies to: CTA buttons, hamburger icon, × close icon, nav links in the drawer, language selector trigger, FAQ accordion headers, pricing CTA buttons, footer links.

Where a link's text is small (e.g., a footer nav link in 14px), the tap target extends via padding, not by increasing font size. The visual area need not be 44×44px; the interactive area must be.

### 6.2 CTA Button Anatomy

```
┌──────────────────────────────┐
│    Start Free Trial          │  ← primary label (≥16px, bold)
│    No Credit Card Required   │  ← sub-label (≤12px, lighter weight)
└──────────────────────────────┘
```

Both label and sub-label sit within the same button element. The sub-label is not a separate element below the button — it is inside the button's bounding box, contributing to the touch target area and associating the sub-label with the CTA semantically.

### 6.3 Anchor Navigation

Navigation links targeting homepage sections (#features, #pricing, #how-it-works) use HTML fragment links. No JavaScript scroll library required.

`#pricing` anchor: The Pricing section element carries `id="pricing"`. When the page loads with a `#pricing` fragment in the URL, the browser scrolls to this element on first load. Verify on mobile Chrome before launch (UJ-2 requirement).

If `scroll-margin-top` is needed to account for the sticky header height (64px), apply via CSS:
```css
#pricing, #features, #how-it-works {
  scroll-margin-top: 64px;
}
```

### 6.4 External Navigation (CTA Links)

All CTAs navigate the current tab to app.fineintegrity.com/register with the correct UTM parameters. No `target="_blank"` on primary CTAs — opening a new tab on mobile is disorienting and breaks UTM session attribution.

### 6.5 GA4 Event Firing

CTA clicks fire a GA4 event before navigation. The event fires synchronously with the click; navigation to app.fineintegrity.com/register happens after the event beacon is dispatched. Use `navigator.sendBeacon` or a short timeout if needed to prevent event loss during navigation.

```
Event name: cta_click
Parameters:
  cta_location: hero | header | how-it-works | pricing | final-cta | mobile-nav
  plan_tier: free-trial | starter | growth | business | null
```

`plan_tier` is populated only for pricing-section CTA buttons; it is null (absent) for all other CTA locations.

---

## 7. Accessibility Floor

Prachar must meet WCAG 2.1 AA. This is the floor. No feature ships below this level.

| Requirement | Standard | Notes |
|---|---|---|
| Color contrast — body text | ≥4.5:1 against background | All body copy, card text, FAQ answers |
| Color contrast — large text (≥18px bold or ≥24px) | ≥3:1 against background | Section headings, pricing tier names |
| Color contrast — interactive elements | ≥3:1 for non-text UI components | CTA button borders, focus rings, icons |
| Keyboard navigation | Full keyboard operability | Tab order follows visual reading order. No keyboard trap anywhere. |
| Focus indicators | Visible at `:focus-visible` | Never `outline: none` without a compliant replacement. Contrast ≥3:1. |
| Images — alt text | All `<img>` elements carry descriptive `alt` | Hero visual alt describes the campaign shown. Decorative images carry `alt=""`. |
| Touch targets | ≥44×44px | All tappable elements. See §6.1. |
| Font display | `font-display: swap` | All `@font-face` declarations. Prevents invisible text during font load. |
| Semantic HTML | Heading hierarchy, landmark regions | Single `<h1>` per page (hero headline). Sections use `<section>` with `aria-label`. Footer uses `<footer>`. Nav uses `<nav>`. |
| FAQ accordion | `aria-expanded` on trigger element | Communicates open/closed state to screen readers. Answer panel uses `aria-hidden` when collapsed. |
| Form elements | All labels associated | Not applicable on this site (no forms) — registration form is on app.fineintegrity.com. |
| Language attribute | `lang="en"` on `<html>` | Correct for v1. Update to `lang="hi"` on Hindi pages when launched. |
| Reduced motion | Respect `prefers-reduced-motion` | Any animations (drawer slide-in, FAQ expand) must be suppressed when the OS requests reduced motion. |

---

## 8. Conversion Discipline

### 8.1 Single-Outcome Architecture

The marketing site has exactly one exit goal: a click to app.fineintegrity.com/register. Every navigational element, content choice, and interactive state is evaluated against this goal.

Prohibited patterns:
- External links from the homepage (except the CTA destination) that take the visitor off the conversion path
- Content that answers product support questions (direct to hello@fineintegrity.com instead)
- Social media links in the header (footer-only, and only if active profiles are confirmed)
- Video autoplay or animated demos that delay LCP and distract from the primary CTA
- Pop-ups, modals, or exit-intent overlays (not consistent with this audience's trust profile)
- Email capture on the marketing site (registration happens at app.fineintegrity.com/register; no lead-gen form)

### 8.2 CTA Placement Rules

A primary CTA ("Start Free Trial" with "No Credit Card Required" sub-label) must appear in each of these positions:

| Position | Always present? | UTM medium |
|---|---|---|
| Sticky header (desktop; or mobile nav drawer) | Yes | header / mobile-nav |
| Hero section (above fold) | Yes | hero |
| How It Works section (below steps) | Yes | how-it-works |
| Each pricing tier card (4 CTAs) | Yes | pricing |
| Final CTA section | Yes | final-cta |

No section between the hero and the footer may be more than one mobile scroll from the nearest CTA. Visitors must never be in a position where they want to sign up but cannot see or reach a CTA without significant scrolling.

### 8.3 Copy Guardrails

The following rules are enforced at review time, before any content ships:

1. **No technical brand names exposed:** Claude, Azure, Meta, Razorpay, Unsplash, SendGrid, Twilio — none appear in public-facing copy.
2. **No AI superiority claims:** "Most advanced," "cutting-edge," "proprietary AI," "state-of-the-art" — all prohibited.
3. **No generic social proof:** "Thousands of happy customers," "Trusted by businesses" without attribution — prohibited until real, verified proof exists.
4. **No UGC or testimonials without explicit written permission** from the named individual or business.
5. **Specific numbers over vague claims:** "₹999/month" over "affordable." "Under 15 seconds" over "fast." "Diwali, Navratri, Eid, Holi, and 15+ more" over "all major Indian festivals."
6. **Festival naming must be current:** A live site that says "Diwali is coming" in March fails the urgency test and actively harms trust.

### 8.4 Seasonal Freshness Obligation

The hero copy has a defined shelf life. A site launched in October 2026 for Diwali must be refreshed for the next seasonal window (New Year, Holi, etc.) or it degrades in conversion performance and SEO relevance.

**Operational requirement:** Nilesh or a designated content owner must maintain a content calendar with refresh dates for hero copy. The static build must be deployable in under 5 minutes (Appendix A NFR) to make seasonal refreshes practical.

**Minimum refresh cadence:** Twice yearly (October window and April window). Best practice: before each major festival window (6–8 times yearly).

**Risk if neglected:** Visitors arriving from search for "Holi promotion tool" in March who see "Get ready for Diwali campaigns" lose trust immediately. Copy currency is a conversion variable, not a cosmetic preference.

### 8.5 WhatsApp Link Preview Discipline (UJ-2 Requirement)

Meena's boss in UJ-2 encounters Prachar for the first time as a WhatsApp link preview — before he taps the link. The og:image and og:title are the entire first impression.

**og:title requirements:**
- ≤60 characters
- Communicates the core value proposition out of context (readable in a chat thread without surrounding page context)
- Must not rely on the og:image for meaning — some WhatsApp clients suppress images

**og:description requirements:**
- ≤160 characters
- Includes the free trial hook ("Free trial — no credit card")
- Includes the price anchor if space allows ("from ₹999/month")

**og:image requirements:**
- Exactly 1200×630px
- ≤200KB file weight
- Shows Prachar branding, the product value proposition, and ideally a festival motif
- Must not contain text that is illegible at WhatsApp preview thumbnail size
- Preferred: product name prominently placed in the top-left or center; value proposition in 2–3 words; festival visual element

**Testing protocol:** Before launch, verify the WhatsApp link preview by sending fineintegrity.com and fineintegrity.com/#pricing to a test WhatsApp number and confirming og:title, og:description, and og:image render correctly.

---

## 9. Responsive and Platform

### 9.1 Breakpoint Strategy

This site is mobile-first. CSS is written for 375px as the base, with breakpoints adding complexity upward.

| Breakpoint name | Width | Context |
|---|---|---|
| Mobile (base) | 375px | Android Chrome (primary audience) |
| Mobile large | 430px | iPhone 14 / large Android |
| Tablet | 768px | iPad portrait; hamburger transitions to desktop nav at this point |
| Desktop | 1280px | Reference desktop viewport |
| Wide | ≥1440px | Large monitor; content max-width constrains layout |

Content max-width: 1200px (or per DESIGN.md token `{layout.maxWidth}`). Centered horizontally on wide viewports. Full-width sections (hero, pricing background) extend to viewport edge; content within is constrained.

### 9.2 Mobile-First Behavior Rules

- **Layout:** All sections default to single-column, full-width stack. Grid and multi-column layouts are added at ≥768px.
- **Images:** `srcset` with WebP primary and JPEG fallback. Display size specified in `sizes` attribute to prevent downloading desktop-size images on mobile.
- **Touch:** All interactions designed for tap-first. Hover states are additive (desktop enhancement), never load-bearing for comprehension.
- **Scroll:** Native scroll throughout. No custom scroll libraries. Horizontal scrolling prohibited at any viewport ≥320px.
- **Font sizes:** Minimum 16px for body text on mobile (prevents iOS auto-zoom on form inputs, if ever added; maintains readability on 4G-challenged users).

### 9.3 Performance Budget (Android 4G Baseline)

| Metric | Target | Source |
|---|---|---|
| LCP | < 2.5s at P75 | PRD Appendix A, Core Web Vitals |
| CLS | < 0.1 | PRD Appendix A |
| INP | < 200ms | PRD Appendix A |
| Total transferred (first load) | < 500KB | PRD Appendix A |
| Hero image weight | ≤150KB | FR-6 |
| og:image weight | ≤200KB | FR-19 |
| SVG icons | ≤5KB each | FR-9 |

**LCP element must be text or a pre-sized image with explicit width/height declared.** If the hero image is the LCP candidate, it must have `<link rel="preload">` in `<head>` and explicit dimensions in the `<img>` tag to prevent CLS.

**GA4 loads asynchronously.** The analytics snippet must not be render-blocking. Load via `async` attribute or before `</body>`.

### 9.4 Android Chrome Specific

- `#pricing` anchor fragment must work on first load. Verify on Android Chrome (the browser used by Rajan in UJ-1 and the boss in UJ-2). Some mobile browsers have quirks with fragment navigation on first load — test explicitly before launch.
- Sticky header on Android Chrome: verify the header stays fixed during soft keyboard open/close (if any future input is added). For v1 (no forms on this page), this is not a risk.
- Meta viewport tag: `<meta name="viewport" content="width=device-width, initial-scale=1">`. No `user-scalable=no` — accessibility requirement.

### 9.5 SEO and Metadata (Technical)

| Element | Requirement | FR |
|---|---|---|
| `<title>` | ≤60 chars; unique per page; contains "Prachar" and primary keyword | FR-18 |
| `<meta name="description">` | ≤160 chars; unique per page | FR-18 |
| `og:title` | ≤60 chars; value proposition legible out of context | FR-19 |
| `og:description` | ≤160 chars; free trial hook | FR-19 |
| `og:image` | 1200×630px; ≤200KB | FR-19 |
| `og:url` | Canonical HTTPS URL for each page | FR-19 |
| `og:type` | website (homepage); article or website for legal pages | FR-19 |
| Twitter Card tags | twitter:card, title, description, image | FR-19 |
| SoftwareApplication schema | ld+json in `<head>`; applicationCategory, operatingSystem, offers, name, url | FR-20 |
| Canonical tag | `<link rel="canonical">` on every page | FR-22 |
| sitemap.xml | Lists homepage, /privacy-policy, /terms | FR-21 |
| robots.txt | Allows all; references sitemap | FR-21 |

---

## 10. Inspiration and Anti-Patterns

### 10.1 Inspiration (Behavioral, Not Visual)

- **WhatsApp's own onboarding web pages:** Direct, outcome-first language. Clear single-action per screen. No technical exposition. Trusted in India because of WhatsApp's ubiquity.
- **Razorpay's early marketing pages:** INR-first pricing, Indian business specificity, clear trust signals without corporate coldness.
- **Zepto / Blinkit marketing:** Speed-of-value communication. The first three seconds communicate the entire proposition. Regional specificity as a differentiator.

### 10.2 Anti-Patterns (Explicitly Prohibited)

| Anti-pattern | Why prohibited |
|---|---|
| Hero with full-bleed background image behind text | Image becomes LCP candidate, blocks text render, fails the 2.5s threshold for Rajan's 4G load |
| "Request a Demo" as primary CTA | Creates a salesperson dependency; this audience wants a self-serve trial, not a sales call |
| Pricing behind a "Contact Sales" click | Meena's boss cannot evaluate without seeing ₹ pricing; opaque pricing loses this audience immediately |
| Testimonials without attribution | Generic proof signals fabrication to a skeptical audience |
| Multiple competing CTAs on the same screen ("Sign Up" and "Learn More" and "Book a Demo") | Splits attention; reduces conversion; violates single-outcome architecture |
| Autoplay video in the hero | LCP impact; 4G data cost; Android Chrome may mute and confuse |
| Pop-ups or exit-intent overlays | Trust-destroying for this audience; interrupts Rajan mid-scroll |
| Carousel or horizontal scroll in the features or pricing sections | Accessibility failure; pricing cards must be in a vertical stack |
| Sticky bottom CTA bar (separate from header) | Visual clutter on small screens; combined with the sticky header, consumes too much viewport |
| Storing UTM parameters in localStorage | Unnecessary complexity; UTM passthrough via URL params is sufficient; GA4 cross-domain handles attribution |

---

## 11. Key Flows

### 11.1 UJ-1: Rajan — WhatsApp Forward to Sign-Up (Mobile, Android)

**Protagonist:** Rajan, salon owner, 2 branches in Pune. Mid-morning. Android phone, 4G.

**Entry:** Rajan's friend sends him `fineintegrity.com` via WhatsApp with the message "check this for Diwali offers." Rajan taps the link; it opens in the Android Chrome in-app browser.

**Step 1 — Hero loads.**
- HTML, CSS, and above-fold text render first.
- Within 2.5 seconds, Rajan sees the headline (Diwali-specific, ≤10 words), sub-headline, and the "Start Free Trial — No Credit Card Required" button.
- The hero image may still be loading; this does not block the CTA.
- Rajan reads the headline. It names Diwali. It implies 5 minutes. He keeps scrolling.

**Step 2 — How It Works.**
- Rajan scrolls down. He sees the 3-step vertical stack: Describe → AI Generates → Approve & Send.
- No technical vocabulary. He scrolls past the secondary CTA — he wants to know more first.

**Step 3 — Features.**
- He notices "Built for Indian festivals" and "Salons & Beauty" in the industry callout.
- Self-identification moment: he is in the list. Prachar is for him.

**Step 4 — Social Proof.**
- He sees a testimonial or industry/city callout. Another business like his already uses it.
- Trust barrier lowers.

**Step 5 — Pricing.**
- He sees Free Trial at ₹0 and Starter at ₹999/month.
- "No credit card required" is on the Free Trial CTA button.
- He taps "Start Free Trial" on the Free Trial tier.

**Step 6 — Registration.**
- Browser navigates to app.fineintegrity.com/register. UTM parameters preserved: `utm_source=website&utm_medium=pricing&utm_campaign=free-trial&utm_content=free-trial`.
- GA4 fires `cta_click` with `cta_location=pricing`, `plan_tier=free-trial`.
- The marketing site's job is done. Product onboarding takes over.

**Edge case — slow 4G:** Hero text and CTA must be visible within 2.5 seconds even if the hero image has not loaded. Rajan can tap the CTA before the image paints. This is the explicit design requirement — text-first, image-asynchronous.

**Edge case — session interrupted:** Rajan gets a WhatsApp message mid-scroll. When he returns, the page is still loaded. The sticky header "Start Free Trial" is visible at any scroll position, providing a persistent re-entry point.

---

### 11.2 UJ-2: Meena to Boss — Desktop Research to Mobile Sign-Up via WhatsApp Forward

**Protagonists:** Meena, executive at a garments shop (desktop Chrome). Her unnamed boss (mobile Chrome, receives a WhatsApp forward). It is September.

**Entry (Meena):** Google search for "WhatsApp bulk message Navratri offer India." fineintegrity.com appears organically. Meena clicks through to the homepage.

**Step 1 — Meena lands, scrolls to Pricing.**
- Meena is evaluating, not browsing. She scrolls directly to pricing.
- She sees ₹999/month on Starter and the "Most Popular" badge on Growth at ₹1,999/month.
- She confirms: Free Trial, no credit card.

**Step 2 — Meena copies the URL.**
- Meena copies `fineintegrity.com/#pricing` (or navigates via the header nav, which sets the fragment).
- She sends it to her boss on WhatsApp: *"Sir dekho — ₹999/month mein Navratri campaign automatic. Free trial bhi hai."*

**Climax beat 1 — The WhatsApp preview renders.**
- WhatsApp fetches og:title and og:image for the URL. Because og: tags are set at the homepage level, the preview renders the main Prachar value proposition image and title.
- The boss reads the og:title ("Festival campaigns in 5 minutes — Prachar") and og:description ("Free trial — no credit card required. From ₹999/month.") before he taps.
- Trust established before the link opens.

**Step 3 — Boss opens on mobile.**
- Boss taps the link. Android Chrome opens `fineintegrity.com/#pricing`.
- **Climax beat 2 — #pricing anchor works on first load.** The page loads and scrolls directly to the Pricing section. The boss lands at pricing — exactly where Meena sent him.
- He sees the four pricing tiers in a vertical mobile stack.
- He sees Free Trial (₹0, 1 month, no credit card) and Starter (₹999/month).

**Step 4 — Boss signs up.**
- Boss taps "Start Free Trial" on the Free Trial tier.
- Browser navigates to app.fineintegrity.com/register with UTM parameters: `utm_source=website&utm_medium=pricing&utm_campaign=free-trial&utm_content=free-trial`.
- GA4 cross-domain session continues from fineintegrity.com to app.fineintegrity.com.
- Sign-up is attributed to organic search (Meena's entry) via GA4 cross-domain measurement.

**Edge case — #pricing does not scroll on first load.** The boss lands at the top of the homepage and sees the hero. The hero is still conversion-optimized and contains a "Start Free Trial" CTA — the site still converts — but the boss must find pricing himself. Mitigation: apply `scroll-margin-top: 64px` on `#pricing` via CSS; test on Android Chrome before launch; verify hash navigation works without JavaScript if possible.

**Edge case — boss opens in WhatsApp in-app browser.** Some Android WhatsApp versions open links in the in-app browser, which may have quirks with anchor fragment navigation. If testing reveals in-app browser failures, add a JavaScript fallback that reads `window.location.hash` on DOMContentLoaded and manually scrolls to the element.

---

## 12. Open Questions (Inherited from PRD)

The following questions from the PRD affect behavioral decisions in this document. Resolution is owned by Nilesh or the PM.

| # | Question | Impact on Experience |
|---|---|---|
| OQ-1 | Will a pilot business grant written permission to be named in Social Proof? | Determines whether Social Proof carries a testimonial (high trust) or an industry/city callout (lower trust but always available) |
| OQ-2 | Who authors Privacy Policy and Terms of Service, and by what date? | Both pages must be live before launch; they block go-live |
| OQ-3 (Launch blocker) | Who owns GA4 cross-domain measurement setup between fineintegrity.com and app.fineintegrity.com? | SM-1 (conversion rate) is unmeasurable without this; UJ-2 attribution is broken without it |
| OQ-4 | Is Razorpay confirmed as the payment gateway? | Affects Pricing FAQ Q3 copy — do not name Razorpay until confirmed |
| OQ-7 | Is app.fineintegrity.com/register live and ready to receive traffic? | All CTAs point here; if the destination is not ready, every CTA click is a dead end |
| OQ-8 | Who produces the hero visual (festival campaign mockup) and by what date? | Hero visual is the longest-lead asset; UX cannot finalize the above-fold layout until visual dimensions and style are confirmed |
| OQ-9 | Does Fine Integrity's legal position require a cookie/data-collection notice under India's DPDP Act 2023? | If yes, a consent banner is required at page load — this affects LCP, above-fold layout, and CTA visibility |

---

*End of EXPERIENCE.md — Prachar Marketing Website behavioral spine.*
*Visual specifications: see DESIGN.md in this same directory.*
*Functional requirements: see PRD at `_bmad-output/planning-artifacts/prds/prd-Prachar-Marketing-Website-2026-06-15/prd.md`.*
