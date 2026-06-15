---
name: "Prachar (प्रचार) Marketing Website — Visual Design Specification"
status: final
created: 2026-06-15
updated: 2026-06-15
sources:
  - "_bmad-output/planning-artifacts/prds/prd-Prachar-Marketing-Website-2026-06-15/prd.md"
  - "_bmad-output/planning-artifacts/briefs/brief-Campaign Promotion Automation-2026-06-15/brief.md"

colors:
  primary: "#F97316"
  primary-light: "#FED7AA"
  primary-deep: "#EA580C"
  accent-amber: "#FBBF24"
  indigo-dark: "#1E1B4B"
  indigo-mid: "#312E81"
  indigo-soft: "#4338CA"
  background-page: "#FFF8F1"
  surface-card: "#FFFFFF"
  text-muted: "#6B6B8A"
  border-subtle: "#E8E0F7"
  success: "#16A34A"
  canvas-section-bg: "#EAEAF0"

typography:
  font-stack: "'Segoe UI', system-ui, -apple-system, sans-serif"
  display.weight: 800
  display.letter-spacing-mobile: "-0.5px"
  display.letter-spacing-desktop: "-1px"
  display.line-height: "1.15–1.2"
  body.weight: "400–500"
  body.line-height: "1.6–1.65"
  small.weight: "500–600"
  small.letter-spacing: "0.5–1.5px"
  small.transform: "uppercase"

rounded:
  sm: "6px"
  md: "12px"
  lg: "16px"
  pill: "99px"

spacing:
  base-unit: "4px"
  scale: [4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 48, 56, 64]

components:
  wordmark.on-dark.text-color: "#FFFFFF"
  wordmark.on-dark.accent-bar-color: "#F97316"
  wordmark.on-dark.accent-bar-height: "3px"
  wordmark.on-dark.devanagari-color: "#FED7AA"
  wordmark.on-dark.devanagari-size: "9px"
  wordmark.on-light.text-color: "#1E1B4B"
  wordmark.on-light.accent-bar-color: "#F97316"

  header.height: "64px"
  header.background: "#1E1B4B"
  header.border-bottom: "2px solid #F97316"
  header.nav-link-color: "#FED7AA"
  header.cta-background: "#F97316"
  header.cta-text-color: "#FFFFFF"

  hero.gradient: "linear-gradient(160deg, #1E1B4B 0%, #312E81 60%, #4338CA 100%)"
  hero.glow-color: "rgba(249,115,22,0.35)"
  hero.glow-position: "top-right radial"

  eyebrow-tag.background: "rgba(249,115,22,0.15)"
  eyebrow-tag.border: "1px solid rgba(249,115,22,0.4)"
  eyebrow-tag.text-color: "#FBBF24"
  eyebrow-tag.font-size: "10–11px"
  eyebrow-tag.font-weight: 600
  eyebrow-tag.border-radius: "99px"
  eyebrow-tag.text-transform: "uppercase"

  cta-primary.background: "#F97316"
  cta-primary.text-color: "#FFFFFF"
  cta-primary.border-radius: "12px"
  cta-primary.box-shadow: "0 4px 20px rgba(249,115,22,0.45)"
  cta-primary.hover-background: "#EA580C"

  feature-card.background: "#FFFFFF"
  feature-card.border: "1px solid #E8E0F7"
  feature-card.border-radius: "12px"
  feature-card.icon-size: "40x40px"
  feature-card.icon-border-radius: "10px"
  feature-card.icon-gradient: "linear-gradient(135deg, #F97316 0%, #EA580C 100%)"

  pricing-card-highlighted.border: "3px solid #1E1B4B"
  pricing-card-highlighted.badge-background: "#F97316"
  pricing-card-highlighted.badge-text: "#FFFFFF"
  pricing-card-default.border: "1px solid #E8E0F7"

  trust-indicator.dot-size: "6–7px"
  trust-indicator.dot-color: "#16A34A"
  trust-indicator.text-color: "#6B6B8A"

  language-selector.icon-color: "#FED7AA"
  language-selector.text-color: "#FED7AA"
  language-selector.active-check-color: "#F97316"
  language-selector.disabled-text-color: "#6B6B8A"

  faq-accordion.expand-icon-rotation: "180deg"
  faq-accordion.answer-text-color: "#6B6B8A"
---

# Prachar (प्रचार) — Visual Design Specification

## Brand & Style

### Direction: Festive Bold

Prachar is a premium Indian SaaS product aimed at SMB owners and admins who discover it during festival season, often via a WhatsApp forward on a mid-range Android device. The visual language must do two things simultaneously: convey the warmth and energy of the Indian festive calendar — the reds and golds of Diwali, the saffron of celebration — and hold the trust register of a legitimate, global-quality software product.

The direction, **Festive Bold**, achieves both through a constrained two-colour palette: **Primary Saffron (#F97316)** and **Indigo Dark (#1E1B4B)**. Saffron draws the eye, marks the call to action, and signals celebration. Indigo anchors the layout in authority, recalls the deep blues of premium fintech and enterprise SaaS, and prevents the palette from reading as decorative.

The result is not a generic SaaS site decorated with Indian elements. The Indian festive energy is structural: it lives in the palette, the eyebrow tags, the hero gradient's warmth, and the amber-highlighted "Most Popular" badge. The design feels native to the market without being folkloric or culturally caricatured.

### Visual Personality

- **Warm** — the off-white background (#FFF8F1) carries the warmth of a festival invitation card, not cold white.
- **Bold** — display text is weight 800 with tight letter-spacing; the hierarchy is decisive and fast to scan on a small screen.
- **Trustworthy** — indigo dark appears on the header, hero, and pricing highlight border, grounding every premium touchpoint in the same deep authority colour.
- **Celebratory** — saffron appears only on interactive elements (CTAs, active states) and decorative accents (eyebrow tags, icon containers, accent bars). It never fills a background section on the light canvas.

---

## Colors

The palette is intentionally narrow. Two chromatic colours do the work of the entire design; a third (amber, #FBBF24) appears sparingly as a highlight. This restraint avoids visual noise on small screens, keeps saffron impactful wherever it appears, and keeps the site legible in the compressed preview state of a WhatsApp link card.

### Colour Roles

| Token | Hex | Role |
|---|---|---|
| `colors.primary` | `#F97316` | Primary CTA buttons, active states, icon container gradient start, accent bar, eyebrow border |
| `colors.primary-light` | `#FED7AA` | Devanagari sub-script on wordmark; nav link colour on dark header; muted warm highlights |
| `colors.primary-deep` | `#EA580C` | CTA hover state; error/destructive UI state (never red) |
| `colors.accent-amber` | `#FBBF24` | Eyebrow tag text; "Most Popular" sub-label; maximum one instance per section |
| `colors.indigo-dark` | `#1E1B4B` | Page headline text on light; sticky header background; pricing highlighted card border; hero gradient start |
| `colors.indigo-mid` | `#312E81` | Hero gradient midpoint; dark surface secondary use |
| `colors.indigo-soft` | `#4338CA` | Hero gradient end; gradient depth reference |
| `colors.background-page` | `#FFF8F1` | Warm off-white page background; replaces pure white on the light canvas |
| `colors.surface-card` | `#FFFFFF` | Feature cards, pricing cards, FAQ panels |
| `colors.text-muted` | `#6B6B8A` | Body copy on light backgrounds; FAQ answers; trust indicator labels |
| `colors.border-subtle` | `#E8E0F7` | Default card borders, dividers; carries a faint violet warmth |
| `colors.success` | `#16A34A` | Trust indicator dots; opt-in confirmation states |
| `colors.canvas-section-bg` | `#EAEAF0` | Alternating section backgrounds (e.g., How It Works, Social Proof) |

### Colour Usage Rules

**Saffron on dark surfaces only for interactive elements.** Primary saffron (#F97316) is reserved for CTAs, icon containers, accent bars, and eyebrow decorations. At full opacity across a section background it reads as a warning banner and undermines the premium register.

**Indigo dark is the trust anchor.** Every premium touchpoint — the sticky header, the hero gradient, the "Most Popular" pricing card — is grounded in #1E1B4B. Never substitute a lighter shade in high-trust components.

**Amber accent is a highlight, not a second primary.** #FBBF24 appears in eyebrow tag text and promotional badge labels. More than one amber element per section creates a carnival effect.

**No pure black (#000000) anywhere.** Headline text on light backgrounds uses Indigo Dark (#1E1B4B). Body text uses Muted (#6B6B8A).

**No red for error states.** Saffron Deep (#EA580C) handles all destructive or error-state uses, maintaining palette consistency while remaining perceptually distinct from success green (#16A34A).

**Gradients belong to dark surfaces.** The hero gradient, dark sections, and icon container gradients are the only gradient uses. Light sections (Features, Pricing, FAQ) use flat white cards on the warm off-white canvas.

---

## Typography

### Font Decision: System Stack

No custom web font loads in v1. The stack — `'Segoe UI', system-ui, -apple-system, sans-serif` — resolves to Segoe UI on Windows, San Francisco on iOS/macOS, and Roboto on Android. This eliminates render blocking, keeps total page weight well under 500KB, and ensures near-zero Cumulative Layout Shift from font swaps.

Cross-platform type rendering will vary slightly. The design compensates through weight, size, and spacing rather than relying on distinctive letterform character.

### Type Scale

| Role | Weight | Size (mobile) | Size (desktop) | Line-height | Letter-spacing |
|---|---|---|---|---|---|
| Display / H1 | 800 | 32–36px | 56–64px | 1.15–1.2 | −0.5px / −1px |
| H2 (section headline) | 700 | 24–28px | 36–42px | 1.2–1.25 | −0.25px |
| H3 (card headline) | 600–700 | 18–20px | 20–22px | 1.3 | 0 |
| Body | 400–500 | 15–16px | 16–17px | 1.6–1.65 | 0 |
| Small / meta | 500–600 | 11–12px | 12–13px | 1.4 | 0.5–1.5px |
| CTA label | 600–700 | 15–16px | 16px | 1 | 0 |

### Type Hierarchy Rationale

Weight 800 with negative letter-spacing is critical for the display size. On a 375px viewport, the hero headline must read as a bold statement in under two seconds. Tight tracking at large sizes prevents the wide, loose look of lighter-weight system text and gives the headline the graphic impact of a well-designed festival banner.

Body text at 400–500 with 1.6–1.65 line-height prioritises legibility on mid-range Android screens. Small/meta text — used for eyebrow tags, pricing period labels, and section labels — is always uppercase with generous letter-spacing (0.5–1.5px) to compensate for reduced legibility at all-caps small sizes.

---

## Layout & Spacing

### Grid

- **Mobile (< 768px):** Single column; horizontal page padding 16px (4 × 4px base unit).
- **Tablet (768px–1024px):** 12-column grid; gutter 24px; column margin 32px.
- **Desktop (≥ 1024px):** 12-column grid; max-width container 1200px; centred with auto horizontal margins; gutter 32px; column margin 48px.

### Spacing Scale

The 4px base unit produces: **4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 48, 56, 64 px**. All padding, margin, gap, and inset values use this scale. No arbitrary values.

### Section Rhythm

Each homepage section is a full-width horizontal band with consistent vertical padding:

- **Mobile:** 48px top / 48px bottom
- **Desktop:** 80px top / 80px bottom

Section backgrounds alternate between `colors.background-page` (#FFF8F1) and `colors.canvas-section-bg` (#EAEAF0), creating visual breathing room without borders or dividers. The hero is always the indigo gradient; the footer is always flat `colors.indigo-dark`.

### Content Width

Long-form text is constrained to **640px maximum on desktop** (approximately 65–75 characters per line). Card grids and two-column feature layouts expand to the full 1200px container.

---

## Elevation & Depth

Depth marks the boundary between interactive surface and static canvas — it is not decorative.

### Elevation Levels

| Level | Use | Shadow |
|---|---|---|
| 0 | Section backgrounds, canvas | none |
| 1 | Feature cards, FAQ panels | `0 1px 4px rgba(30,27,75,0.08)` |
| 2 | Pricing cards (default) | `0 2px 8px rgba(30,27,75,0.10)` |
| 3 | Pricing card — highlighted (Growth) | `0 8px 32px rgba(30,27,75,0.18)` |
| 4 | Primary CTA button | `0 4px 20px rgba(249,115,22,0.45)` |
| 5 | Sticky header (on scroll) | `0 2px 12px rgba(30,27,75,0.25)` |

Card shadows use Indigo Dark at low opacity; CTA shadows use Primary Saffron at medium opacity to produce a glow on dark backgrounds.

The Growth pricing card uses Level 3 shadow because it is the page's primary conversion surface. The elevated shadow — combined with the 3px Indigo Dark border and the saffron "Most Popular" badge — creates clear focal hierarchy in the pricing row without animation.

---

## Shapes

Rounded corners communicate "friendly premium" throughout. Sharp corners (0px radius) do not appear anywhere — they would read as dated or aggressive against this palette.

### Corner Radius Scale

| Token | Value | Applied to |
|---|---|---|
| `rounded.sm` | 6px | Tags, small badges, tooltips, chip labels |
| `rounded.md` | 12px | CTA buttons, feature cards, pricing cards, input fields, FAQ accordion panels |
| `rounded.lg` | 16px | Large hero visual frame, WhatsApp message mock-up container, modal overlays |
| `rounded.pill` | 99px | Eyebrow tags, language selector, "Most Popular" badge, toggle switches |

The pill shape is reserved for label-like elements that signal category or status rather than a clickable surface. Eyebrow tags use the pill shape with saffron-tinted background to echo a festival badge.

Icon containers (40×40px) use a 10px radius — between `rounded.sm` and `rounded.md` — to feel like a compact tile rather than a button or card.

---

## Components

### Wordmark

The Prachar wordmark appears in the sticky header, hero, and footer in two variants.

**On dark backgrounds (header, hero, footer):** "Prachar" renders in white (#FFFFFF). A 3px saffron (#F97316) underline accent bar sits beneath the letter 'a' only — not the full wordmark. Below the Latin wordmark, "प्रचार" appears at 9px in Saffron Light (#FED7AA), grounding the brand in its Indian market identity without competing with primary wordmark legibility.

**On light backgrounds:** "Prachar" renders in Indigo Dark (#1E1B4B). The saffron accent bar beneath 'a' remains unchanged. The Devanagari sub-script uses Saffron (#F97316) at 9px.

### Sticky Header

The header is a 64px full-width band fixed to the viewport top. Its background is Indigo Dark (#1E1B4B). A 2px bottom border in Primary Saffron (#F97316) separates it from page content — both a brand accent and a visual signal that the header is a distinct surface.

Navigation links render in Saffron Light (#FED7AA) at body weight (500). The #FED7AA choice over pure white maintains cohesion with the saffron palette. Active or hovered nav links transition to white (#FFFFFF). On mobile (≤768px), navigation links collapse to a hamburger icon in #FED7AA.

### Hero Gradient & Decoration

The hero uses `linear-gradient(160deg, #1E1B4B 0%, #312E81 60%, #4338CA 100%)`. The 160° angle creates a diagonal wash from deep indigo at the upper-left (matching the header for visual continuity) to a brighter indigo-soft at the lower-right, giving the hero depth and forward motion without animation.

A decorative radial saffron glow at the upper-right — `radial-gradient(ellipse at top right, rgba(249,115,22,0.35) 0%, transparent 60%)` — evokes warm festive light. It renders behind all content and does not affect text legibility.

### Eyebrow Tag

Eyebrow tags are pill-shaped label elements placed above section headlines (e.g., "🎉 Festival Ready", "Trusted by Indian Businesses").

- Background: `rgba(249,115,22,0.15)`
- Border: `1px solid rgba(249,115,22,0.4)`
- Text colour: Amber Accent (#FBBF24)
- Typography: 10–11px, weight 600, uppercase, letter-spacing 1–1.5px
- Border-radius: 99px (pill)

On light backgrounds, the translucent saffron background reads as a warm apricot tint against #FFF8F1. Maximum one eyebrow tag per section; do not stack two above a single headline.

### Primary CTA Button

The primary CTA appears in the hero (large), sticky header (compact), each pricing tier, and the final CTA section.

- Background: Primary Saffron (#F97316)
- Text colour: white (#FFFFFF)
- Border-radius: 12px (`rounded.md`)
- Box-shadow: `0 4px 20px rgba(249,115,22,0.45)`
- Hover: background transitions to Saffron Deep (#EA580C); box-shadow deepens to `0 6px 24px rgba(234,88,12,0.5)`
- Typography: 16px, weight 600–700
- Minimum touch target: 44×44px

The hero variant includes a "No Credit Card Required" sub-label at 11px Saffron Light (#FED7AA). The header compact variant shows the primary label only.

### Feature Card

Feature cards appear in a grid (2-column mobile, 3-column desktop) in the Features section.

- Background: Surface Card (#FFFFFF)
- Border: `1px solid #E8E0F7`
- Border-radius: 12px (`rounded.md`)
- Elevation: Level 1 shadow
- Internal padding: 24px
- Icon container: 40×40px (10px radius), `linear-gradient(135deg, #F97316 0%, #EA580C 100%)`, icon SVG in white
- Headline: H3 weight, Indigo Dark (#1E1B4B)
- Body copy: Muted (#6B6B8A)

The icon container gradient is the strongest colour element on the light canvas. One per card distributes visual weight across the grid rather than concentrating it.

### Pricing Card — Highlighted (Growth Tier)

The Growth card is the primary conversion target in the pricing section. Three stacked signals distinguish it from Starter and Pro cards:

1. **Border:** 3px solid Indigo Dark (#1E1B4B) vs. 1px solid Border Subtle (#E8E0F7) on default cards
2. **Elevation:** Level 3 shadow — lifts the card above its siblings
3. **"Most Popular" badge:** Pill-shaped (99px radius), Primary Saffron (#F97316) background, white text, positioned at the top-centre above the tier name

All cards use Surface Card (#FFFFFF). The distinction is in borders, shadows, and the badge — not background colour changes, which would disrupt tonal consistency.

### Trust Indicator

Trust indicators appear inline in the hero and Social Proof section. Each consists of a 6–7px filled circle in Success Green (#16A34A) followed by a short text label in Muted (#6B6B8A). Examples: "• 14-day free trial", "• No credit card required", "• 3 businesses already live".

The green dot associates Prachar with affirming states (confirmed, verified) rather than promotional energy. Muted text ensures these read as factual, not promotional.

### Language Selector

The language selector sits in the sticky header, right-aligned near the CTA. It shows a globe SVG icon followed by "EN ▾" in Saffron Light (#FED7AA).

The dropdown panel (border-radius 12px, Surface Card background, Border Subtle border, Level 1 elevation) presents:
- "English ✓" — active state, check mark in Primary Saffron (#F97316)
- "हिन्दी — Coming Soon" — disabled state, text in Muted (#6B6B8A), no interactive affordance

The "Coming Soon" label signals that Hindi localisation is a known roadmap item — a trust signal for Indian visitors.

### FAQ Accordion

The FAQ section uses a stacked accordion. Each item has:

- **Question row:** full-width, background `colors.background-page`, 16px vertical padding, question text in Indigo Dark (#1E1B4B) at H3 weight
- **Expand icon:** chevron or plus/minus SVG in Muted (#6B6B8A), right-aligned; rotates 180° (or transforms from + to −) when expanded
- **Answer panel:** Muted (#6B6B8A) body text, 16px padding, Surface Card (#FFFFFF) background
- **Bottom border:** `1px solid #E8E0F7` between each item

All items collapse by default. Single-expand behaviour: no more than one item open at a time. Outermost container uses `rounded.md` (12px).

---

## Do's and Don'ts

### Do

**Use saffron on dark surfaces for interactive elements.** Primary saffron (#F97316) reaches maximum impact against the deep indigo backgrounds of the header, hero, and highlighted pricing card. Reserve it for CTAs, icon container gradients, active states, and decorative accents.

**Maintain indigo dark as the premium trust anchor.** Every component meant to convey authority — the header, the hero gradient, the pricing highlight border — grounds in #1E1B4B. Substituting a lighter shade undermines the authority signal.

**Use the warm off-white (#FFF8F1) as the light canvas.** Pure white (#FFFFFF) is for card surfaces only. The page background is #FFF8F1, which carries palette warmth through even the lightest sections.

**Keep the colour count per section to two chromatic colours.** Each section relies on saffron and indigo; muted and border-subtle handle neutral roles. Amber (#FBBF24) may appear once per section maximum.

**Use `colors.primary-deep` (#EA580C) for error and destructive states.** This maintains palette consistency while remaining perceptually distinct from primary saffron.

**Trust the type hierarchy.** Weight 800 display text with negative letter-spacing is the primary differentiator on small screens. Fix hierarchy before adding decorative colour or shadow.

### Don't

**Don't use saffron as a section background fill on light pages.** A full-width saffron band reads as a warning banner, not a premium SaaS section.

**Don't use gradients on light sections.** The hero gradient earns its place as the first and most prominent section. Gradients in Features, Pricing, or FAQ diminish the hero's impact. Light sections use flat fills only (#FFF8F1, #EAEAF0, #FFFFFF).

**Don't use pure black (#000000) anywhere.** Not for text, borders, or shadows. Black against the warm off-white breaks colour temperature consistency.

**Don't introduce a third chromatic colour per section.** Saffron and indigo are the full palette. A third colour triggers a cascade of contrast and balance adjustments the system is not designed to absorb. If a new colour is needed, revise the palette definition.

**Don't use red for any UI state.** Red conflicts with the celebratory register. Saffron Deep (#EA580C) handles all destructive and error states. A pure red in the design is an implementation error.

**Don't add amber accent (#FBBF24) to more than one element per section.** Two amber elements cancel each other out and read as visual noise.

**Don't underline or decorate nav links on hover with saffron.** The saffron underline is a wordmark-specific accent (beneath 'a' only). Using it as a general hover decoration dilutes the wordmark's distinctiveness. Nav link hover states use colour transitions to white only.
