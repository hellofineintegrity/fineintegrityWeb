# Campaign Promotion Automation — PRD Session 1
**Date**: 2026-06-14  
**Facilitator**: BMad PRD (Claude Sonnet 4.6)  
**User**: Nilesh (Fine Integrity)  
**Status**: PRD in progress — Coaching Path, Vision + Features entry  
**Workspace**: `_bmad-output/planning-artifacts/prds/prd-Campaign-Promotion-Automation-2026-06-14/`

---

## Product Overview

**Campaign Promotion Automation** is a SaaS platform that enables business executives to describe a promotion or campaign in plain English, and receive an AI-generated, graphics-rich, emoji-friendly promotional template. The template goes through an internal approval workflow and is published to customers via email and/or WhatsApp.

---

## Vision Statement (Confirmed)

Small and mid-sized businesses lose customers to competitors because their promotional outreach is slow, inconsistent, and relies on manual effort. A sales executive who wants to send a Diwali offer or a new product discount must cobble together a message in WhatsApp, write it themselves, get informal approval, and blast it manually — no consistency, no tracking, no professional look.

**Campaign Promotion Automation** gives business executives an AI-powered platform to describe a promotion in plain English, receive a polished, graphics-rich template instantly, route it through an approval workflow, and publish it to customers over email and WhatsApp — all from one place. Businesses that adopt it run more campaigns, faster, with less effort, and attract more customers through timely, professional promotions tied to festivals, seasons, and product moments.

---

## Key Product Decisions

| # | Decision | Rationale |
|---|----------|-----------|
| 1 | Coaching path (Vision + Features entry) | User preference; feature-driven product |
| 2 | AI model: Anthropic Claude Haiku | Cost-optimized; fast creative copy generation |
| 3 | WhatsApp: Meta WhatsApp Cloud API (direct) | Lowest cost; no BSP markup; 1,000 free service conversations/month |
| 4 | Customer data: WhatsApp Business contact sync → customer profile; manual add/edit also supported | User requirement |
| 5 | Pricing: 1 month free trial → per-promotion-published | User requirement |
| 6 | Pilot scope: 3 businesses; cost-optimized stack | User requirement |
| 7 | Subdomain per business: `businessname.fineintegrity.com` | SaaS multi-tenant model; owner is fineintegrity.com |
| 8 | Branch model: One subdomain per business, branch selector inside app | Simpler; branches share templates at business level |
| 9 | Templates shared at business level across all branches | User confirmed |
| 10 | Multiple branches required for pilot (not post-pilot) | User confirmed |
| 11 | Graphics: Pre-built layouts + brand assets + Unsplash free API + AI text | Cost-optimized; no DALL-E needed |
| 12 | Channel selection first, then format generated | Email = HTML; WhatsApp = image header + text + CTA button |
| 13 | AI iterations: up to 4 revision rounds per template | Business rule to prevent infinite loops |
| 14 | 1 template generated per AI run (not multiple variants) | User confirmed |
| 15 | Indian festival/seasonal calendar built into platform | Key differentiator; user confirmed |
| 16 | Meta template submission built into WhatsApp publish flow; status tracked in-app | Required for WhatsApp API compliance |
| 17 | Regional language support required | Key differentiator for Indian market |
| 18 | System Admin (Nilesh) manages industry list | Initially 25 Indian-market industries in dropdown |
| 19 | Opt-out/unsubscribe required | TRAI compliance in India |
| 20 | Subdomain auto-provisioned on business registration | No manual System Admin approval needed |
| 21 | One WhatsApp number per business instance (pilot) | Pilot simplification |
| 22 | Business profile includes logo + brand colors + tagline | Auto-injected into promotions |
| 23 | Rejection flow: Admin rejects with comments → back to executive for modification | User confirmed |
| 24 | Executives see only their own drafts | User confirmed |
| 25 | Admin registers the business | User confirmed |
| 26 | Viewer role: included (read-only analytics) | Recommended; suits Indian SMB owner oversight pattern |

---

## User Roles

| Role | Scope | Permissions |
|---|---|---|
| **System Admin** | Platform (Fine Integrity / Nilesh) | Manage industries, manage all tenants, platform settings |
| **Business Admin** | Business instance | Register business, create + approve + publish promotions, manage team, manage customers, connect WhatsApp |
| **Business Executive** | Business instance | Create promotions with AI (own drafts only), edit own drafts, submit for approval, modify after rejection |
| **Viewer** | Business instance | View published campaigns + analytics only — no create/approve/publish |

---

## Authentication Methods

- Gmail OAuth
- Email + verification code
- Mobile + OTP

---

## Business Profile Fields

- Business name
- Industry (from System Admin-managed dropdown — 25 Indian market industries)
- Contact person name
- Phone number
- Email
- Address
- Logo (uploaded)
- Brand colors
- Tagline
- Branch information

---

## Customer Profile Fields

- Salutation
- Full name
- Mobile number
- Email
- Address
- WhatsApp opt-in status
- Opt-in date
- Opt-out status
- Opt-out date
- Preferred language (for regional template delivery)
- City / branch tag
- Loyalty tier

---

## Customer Management Rules

- Admin + Executive both can add/edit customer profiles
- WhatsApp Business contacts auto-sync → customer profiles when number added to WA Business account
- Customer groups: both manual (named groups) + auto-tagged by segment/industry
- Opt-out/unsubscribe required: unsubscribe link in emails, STOP reply on WhatsApp (TRAI compliance)
- WhatsApp opt-in status tracked per customer (Meta compliance)

---

## Feature Groups (Completed in Session)

### FG-1: Business Onboarding & Profile ✅
- Self-registration: Gmail OAuth / email verification / mobile OTP
- Auto-provisioned subdomain on registration
- Business profile setup with logo, brand colors, tagline
- Branch creation and management (branch selector inside app)
- WhatsApp Business account connection (one number per instance for pilot)

### FG-2: Customer Management ✅
- WhatsApp contact sync → customer profiles
- Manual add/edit by Admin or Executive
- Rich customer profile fields (see above)
- Customer groups (manual + auto-segment)
- Opt-in/opt-out tracking and enforcement

### FG-3: AI Promotion Generation ✅
- Plain English description input
- Context inputs: industry, channel (Email or WhatsApp), preferred language, festival/occasion
- Indian festival + seasonal calendar picker (built-in)
- AI generates 1 template in channel-specific format
- Up to 4 AI revision iterations ("make it more festive", "add Hindi phrases", etc.)
- Graphics approach: pre-built layouts by industry + occasion + brand colors/logo + Unsplash stock photos + AI-written text
- Business profile data auto-injected (name, contact, address, logo, brand colors)
- Regional language support (Hindi + regional Indian languages)
- WhatsApp format: header image + body text (160 chars) + CTA button
- Email format: responsive HTML with full layout

### FG-4: Template Library & Campaign Management 🔄 (In Progress — unanswered questions below)

---

## Open Questions (Pending Nilesh's Answers — FG-4)

1. **Template ownership**: Shared at business level across all branches? (Recommended: yes)
2. **Campaign targeting**: All customers / specific group / specific branch / all of above?
3. **Scheduling**: Can campaigns be scheduled for a future date/time, or always "send now"?
4. **Re-use / recurring campaigns**: Clone previous campaigns (e.g., last year's Diwali)?
5. **Analytics access**: Visible to Executives (read-only) or Admin only?

---

## Research Highlights (Background Intelligence)

### Competitor Landscape (Indian Market)
| Tool | Focus | Price Range | Gap |
|---|---|---|---|
| Zoko | WhatsApp + email, ecommerce | ₹999–5,000/month | No AI generation |
| Interakt | WhatsApp-first, conversational | ₹2,500–10,000/month | No festival calendar |
| WATI | WhatsApp API aggregator | ₹500–5,000/month | Siloed analytics |
| AiSensy | AI copywriting | ₹2,000–8,000/month | No template pre-optimization |
| Gallabox | Omnichannel | ₹1,500–6,000/month | Poor regional language |
| Brevo | Email + SMS | ₹0–3,000/month | No WhatsApp |

**Our differentiators**: AI template pre-optimization before Meta submission, Indian festival calendar, unified WhatsApp + email analytics, regional language support.

### WhatsApp Cloud API Constraints
- All promotional messages require pre-approved Meta templates
- Opt-in required per customer (Meta compliance)
- ~₹0.40–1.50 per promotional message
- Template rejection triggers: excessive emojis/caps, urgency language, unsubstantiated claims
- 1,000 free service conversations/month per number

### Indian Festival Calendar (Built-in)
**National**: Diwali (Oct-Nov), Holi (Mar), Eid (varies), Christmas (Dec), New Year (Jan)  
**Regional**: Onam (Aug-Sep), Pongal (Jan), Navratri (Oct)  
**Seasonal**: Back-to-school (May-Jun), Wedding season (Nov-Jan / Jun-Jul), Summer (Apr-Jun), Monsoon (Jul-Aug)

### 25 Indian Market Industries (Seed List)
1. Fashion / Apparel
2. Electronics & Mobile Retail
3. Home & Kitchen
4. Jewelry
5. Books & Stationery
6. Quick Service Restaurants (QSR)
7. Bakeries & Confectionery
8. Cloud Kitchens / Food Delivery
9. Cafes & Dessert Shops
10. Specialty Food / Organic Products
11. Salons & Beauty
12. Health & Wellness
13. Restaurants & Dining
14. Travel & Tourism
15. Real Estate
16. B2B Manufacturing / Trading
17. Education & Coaching
18. Healthcare Clinics & Diagnostics
19. Automotive
20. Home Services
21. SaaS / Software Services
22. Digital Marketing Agencies
23. Logistics & Courier
24. Event Planning & Decorators
25. Insurance Agents

### AI Promotional Content Best Practices
- WhatsApp: conversational tone, short sentences, 1–2 emojis max, local cultural references
- Email: subject line with emoji + urgency, 100–150 word body, mobile-first
- High-performing patterns: Discount leads, urgency + social proof, FOMO + time, personalized segment
- Regional mixing: Hindi/English transliteration increases engagement 15–20%
- TRAI compliance: every message needs opt-out option

---

## Sections Remaining

- FG-4: Template Library & Campaign Management (answers pending)
- FG-5: Approval Workflow
- FG-6: Publishing & Delivery
- FG-7: Analytics & Reporting
- Section 4: Non-Functional Requirements (performance, security, scalability)
- Section 5: Pricing & Monetization
- Section 6: Pilot Plan

---

## Pending Next Steps

Resume at: **FG-4 — Template Library & Campaign Management** — Nilesh to answer the 5 open questions listed above.
