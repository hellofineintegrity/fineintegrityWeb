---
title: Campaign Promotion Automation
status: final
created: 2026-06-14
updated: 2026-06-15
owner: Nilesh (Fine Integrity)
---

# PRD: Campaign Promotion Automation

## 0. Document Purpose

This PRD is for the product owner (Nilesh, Fine Integrity), downstream architects, UX designers, and developers building the Campaign Promotion Automation SaaS platform. It defines what the system must do — not how to build it. Features are grouped by capability domain; Functional Requirements (FR-1 through FR-52, plus FR-7A, FR-49, FR-50) carry stable IDs for downstream epics and tickets. `[ASSUMPTION]` tags mark inferred decisions requiring confirmation; the Assumptions Index (§9) collects them. Technical implementation decisions (database schema, API design, framework choices) belong in the architecture document.

---

## 1. Vision

### Why This Exists

Small and mid-sized Indian businesses lose customers to competitors because their promotional outreach is slow, inconsistent, and entirely manual. A sales executive wanting to send a Diwali offer or a monsoon car-care discount must write the message themselves, get informal WhatsApp approval, and blast it without knowing whether it reached anyone or drove any sales. There is no professional look, no approval discipline, no delivery tracking, and no way to learn from previous campaigns.

### What It Does

**Campaign Promotion Automation** is a multi-tenant SaaS platform that lets any business executive describe a promotion in plain English — "50% off sarees for Navratri, valid till October 15" — and receive an AI-generated, graphics-rich, regionally relevant promotional template within seconds. The template is routed through an internal approval workflow, then published to customer groups via email or WhatsApp. Every step — creation, approval, scheduling, delivery, and performance — happens in one platform, accessible from any device.

### Why Now

Competition is intensifying for Indian SMBs across retail, food, services, and automotive. Existing tools (Zoko, Interakt, WATI) serve this market but require manual template creation, have no Indian festival calendar, offer no AI content generation, and silo email and WhatsApp analytics. The availability of cost-effective AI models (Claude Haiku) and Meta's WhatsApp Cloud API makes it now possible to build this at a price point Indian SMBs can afford. The pilot targets the Navratri–Dussehra–Diwali season (October–November 2026), the highest-revenue promotional window for all three pilot businesses.

---

## 2. Target Users

### 2.1 Jobs To Be Done

**Business Executive**
- Create a professional, graphics-rich promotional campaign without design skills or marketing expertise
- Communicate in plain English (or Hindi) and get a polished result
- Send culturally relevant campaigns timed to Indian festivals and seasons
- Know that the promotion looks right before it goes out

**Business Admin**
- Maintain quality control over what the business sends to customers
- Publish approved promotions to the right customer groups at the right time
- See whether campaigns are actually working (delivered, opened, clicked)
- Manage the business's customer list and team access

**Business Owner (Viewer)**
- See how campaigns are performing without being in the operational workflow
- Understand whether the investment in promotions is driving results

**System Admin (Fine Integrity)**
- Onboard and manage business tenants on the platform
- Control the industry list and platform-wide settings
- Monitor platform health and usage across all tenants

### 2.2 Non-Users (v1)

- End customers (recipients) — they receive promotions via email/WhatsApp but do not log into the platform
- Enterprise marketing teams requiring advanced segmentation, A/B testing, or CRM integration
- Businesses outside India (v1 is India-focused — festival calendar, pricing, TRAI compliance)
- Businesses requiring an API to integrate with third-party systems

### 2.3 Key User Journeys

**UJ-1. Rahul creates a monsoon car-care promotion in Hindi.**
- **Persona + context:** Rahul is an executive at AutoCare Services (pilot business, automotive). It's July. He wants to push a monsoon wash + underbody coat package to their WhatsApp customer list.
- **Entry state:** Authenticated on `autocare.fineintegrity.com`, on the Create Promotion screen. Branch: AutoCare Pune.
- **Path:**
  1. Selects channel: WhatsApp. Selects language: Hindi.
  2. Opens Festival Calendar, picks "Monsoon Season." Adds description: "Monsoon special — full wash, underbody coat, and interior vacuum at ₹799. Offer valid July 1–31."
  3. AI generates a template: bold header image with rain-themed layout from automotive industry pack, Hindi promotional text, AutoCare logo and contact injected, "Book Now" CTA with the garage's phone number.
  4. Rahul finds the CTA color too dull. Types: "Make the CTA button orange and add a rain emoji in the headline." AI revises (iteration 2 of 4).
  5. Satisfied, he saves the template to the library and submits for Admin approval.
- **Climax:** Rahul sees the promotion in "Awaiting Approval" state. He gets an in-app notification confirming submission. The Admin receives an in-app + email notification to review.
- **Resolution:** Rahul returns to his drafts. The promotion is visible under "Submitted." He cannot edit it unless it is rejected.
- **Edge case:** If Rahul tries to submit without a description or with a description that triggers AI content warnings (e.g., false medical claims), the system surfaces an inline error before generating.

---

**UJ-2. Priya schedules a Navratri campaign for her garments customers.**
- **Persona + context:** Priya is the Admin at Shree Ladies Garments (pilot business). She sees a pending promotion from her executive — a Navratri saree offer. She wants to approve it, target the "Loyal Customers" group, and schedule it to send on Navratri eve.
- **Entry state:** Authenticated on `shreegaments.fineintegrity.com`, Approvals inbox.
- **Path:**
  1. Opens the pending promotion. Reviews the AI-generated WhatsApp template — Hindi text, saree imagery, brand colors. Adds a comment: "Add the store's Instagram link in the footer." Clicks Reject.
  2. Executive Meena receives a rejection notification (in-app + email + WhatsApp) with Priya's comment. Meena edits, re-prompts AI with the Instagram link, resubmits.
  3. Priya reviews the updated version. Approves.
  4. Creates a Campaign: selects "Loyal Customers" group (142 contacts), sets start date Oct 1 at 8:00 AM, end date Oct 10.
  5. System checks Meta template approval status — shows "Pending Meta Approval" (submitted automatically on first approval). Priya clicks "Submit to Meta." Status updates to "Pending."
  6. Meta approves (typically within a few hours; up to 48 hours during peak festival season). Campaign status updates to "Approved — Scheduled." Priya is notified.
- **Climax:** On October 1 at 8:00 AM, the platform sends the WhatsApp campaign to 142 opted-in customers automatically. Priya sees the delivery dashboard update in real time.
- **Resolution:** Campaign moves to "Active." Priya checks analytics the next morning — 138 delivered, 89 read, 23 clicked the product link.
- **Edge case:** If Meta rejects the template, Priya and the executive both receive a rejection notification with Meta's stated reason. The campaign is held; the promotion returns to editing state.

---

**UJ-3. Vikram's grocery customers receive and act on a weekend offer.**
- **Persona + context:** A customer of Fresh Basket (pilot grocery business) receives a WhatsApp message on a Friday evening.
- **Entry state:** Customer's personal WhatsApp, no platform login required.
- **Path:**
  1. Customer receives a WhatsApp message from Fresh Basket's business number: header image (produce photography, brand green colors), text "Weekend special — 20% off fresh vegetables. Valid Sat–Sun only. Order via link 🛒", "Shop Now" button linking to the store's ordering page.
  2. Customer taps "Shop Now" — tracked click registered in the platform.
  3. If customer replies "STOP," the platform registers an opt-out, marks their profile `opted_out = true`, and excludes them from all future WhatsApp campaigns.
- **Climax:** Fresh Basket Admin sees a spike in click-through on Friday evening in the campaign dashboard.
- **Resolution:** Customer is opted out if they replied STOP and will not receive future WhatsApp campaigns unless they opt back in.

---

## 3. Glossary

- **Platform** — The Campaign Promotion Automation SaaS system operated by Fine Integrity.
- **Business** — An organisation registered on the Platform. Represented by a subdomain (`businessname.fineintegrity.com`). Contains one or more Branches.
- **Branch** — A physical or logical sub-unit of a Business (e.g., AutoCare Pune, AutoCare Mumbai). Each Branch has its own WhatsApp number, customer list, and billing. Branches share the parent Business's Templates and brand assets.
- **Business Admin** — A Branch-level user who can create, approve, and publish Promotions; manage the Customer list; and manage team members.
- **Business Executive** — A Branch-level user who can create Promotions using AI, edit their own Drafts, and submit for Admin approval.
- **Viewer** — A Branch-level user with read-only access to published Campaigns and Analytics.
- **System Admin** — The Fine Integrity operator account. Manages the platform, industry list, and all Business tenants.
- **Promotion** — An AI-generated or manually authored marketing message (text + graphics) describing an offer. Exists as a Draft, Submitted, Approved, Rejected, or Archived state.
- **Template** — A saved, reusable Promotion layout. Stored at the Business level; accessible by all Branches.
- **Campaign** — A Template bound to a target Customer Group, a Channel, a start date, and an end date. The unit of scheduling and delivery.
- **Channel** — The delivery medium: Email or WhatsApp. Selected before AI generation; determines the output format.
- **WhatsApp Template** — A Meta-approved message format (header image + body text + CTA button) required by the WhatsApp Cloud API before any marketing message can be sent.
- **Customer Profile** — A contact record containing demographic, communication, and consent fields for a single customer.
- **Customer Group** — A named collection of Customer Profiles used as a Campaign delivery target.
- **Opt-in** — A customer's explicit consent to receive promotional WhatsApp messages from the Business, recorded with timestamp.
- **Opt-out** — A customer's withdrawal of consent (via STOP reply or unsubscribe link), recorded with timestamp. Excludes the customer from future Campaigns on the opted-out Channel.
- **Festival Calendar** — The Platform's built-in index of Indian festivals and seasonal moments selectable as AI generation context.
- **Approval Workflow** — The defined state machine a Promotion passes through: Draft → Submitted → Approved/Rejected → (if Approved) Published.
- **Industry** — A business category managed by the System Admin; selected during Business onboarding and used as AI generation context.

---

## 4. Features

### 4.1 Business Registration & Onboarding

**Description:** Any business can self-register on the Platform and receive an operational tenant within minutes. Registration supports three authentication methods. On successful registration, the system auto-provisions a subdomain, sets up the Business profile, and guides the Admin through connecting a WhatsApp number and adding the first Branch. The Business Admin — not the System Admin — performs all registration steps; System Admin intervention is not required for onboarding. Business profile data (logo, brand colors, contact info) is injected into all AI-generated Promotions automatically.

**Functional Requirements:**

#### FR-1: Multi-method business registration
Business Admin can register a new Business account via Gmail OAuth, email address + verification code, or mobile number + OTP. Each method is independently selectable. Registration requires: Business name, industry (from Platform dropdown), and primary contact details.

**Consequences (testable):**
- Gmail OAuth registration redirects through Google's OAuth 2.0 flow and returns a verified email without a separate verification step.
- Email registration sends a 6-digit verification code to the provided email; code expires after 10 minutes; resend allowed once per 2 minutes.
- Mobile OTP is 6 digits, expires after 5 minutes; maximum 3 OTP attempts before a 15-minute lockout.
- Duplicate email or mobile number registration surfaces an inline error: "An account with this email/number already exists."

#### FR-2: Automatic subdomain provisioning
On successful registration, the Platform automatically provisions a unique subdomain (`businessname.fineintegrity.com`) and makes it accessible within 60 seconds.

**Consequences (testable):**
- Subdomain is derived from Business name (lowercase, spaces replaced with hyphens, special characters stripped).
- If subdomain already exists, system appends a numeric suffix (e.g., `matrix-2.fineintegrity.com`) and informs the Admin.
- Subdomain is active and routed to the correct Business tenant within 60 seconds of registration.

#### FR-3: Business profile setup
Business Admin can complete and edit the Business profile including: Business name, Industry (dropdown), logo (image upload), brand colors (hex picker, minimum 2: primary + accent), tagline, contact person name, phone, email, and address.

**Consequences (testable):**
- Logo accepts PNG/JPG/SVG up to 5 MB; stored in Azure Blob Storage.
- All Business profile fields are available as injectable variables in AI-generated Promotions.
- Brand colors are applied automatically to all pre-built graphic layouts used in Promotion generation.
- Profile edits are reflected in new Promotions immediately; previously generated Promotions are not retroactively updated.

#### FR-4: Branch creation and management
Business Admin can create, name, and manage multiple Branches within the Business. Each Branch has its own WhatsApp number, Customer list, and billing account. Templates are shared across all Branches.

**Consequences (testable):**
- Branch selector is visible in the navigation after login; switching branches scopes all data views (customers, campaigns, analytics) to the selected branch.
- A Business must have at least one Branch; the first Branch is created during onboarding.
- Branch name and WhatsApp number are independently configurable per Branch.

#### FR-5: WhatsApp Business account connection
Business Admin can connect a WhatsApp Business phone number to a Branch via the Meta WhatsApp Cloud API. Connection requires the Admin to authenticate through Meta's embedded signup flow.

**Consequences (testable):**
- A Branch without a connected WhatsApp number cannot create WhatsApp-channel Campaigns.
- Connection status is visible on the Branch settings page: Connected / Disconnected / Error.
- Disconnection or API error surfaces an alert to the Admin.

#### FR-6: Team member invitation and role assignment
Business Admin can invite team members by email or mobile number, assign them a Role (Executive or Viewer), and revoke access at any time.

**Consequences (testable):**
- Invited user receives an email + WhatsApp invitation link valid for 48 hours.
- Admin can change a user's Role at any time; the change takes effect on the user's next page load.
- Revoking access immediately terminates the user's active sessions.

---

### 4.2 Customer Management

**Description:** Each Branch maintains its own Customer list, populated via WhatsApp contact sync and/or manual entry by Admins or Executives. Customer Profiles carry consent status (opt-in/opt-out) per Channel, enabling compliant campaign targeting. TRAI compliance requires every outbound promotion to include an opt-out mechanism; the Platform enforces this automatically.

**Functional Requirements:**

#### FR-7: Customer profile creation via inbound WhatsApp message
When a customer sends any WhatsApp message to the Branch's connected WhatsApp Business number, the Platform automatically creates a Customer Profile for that customer using the phone number (`wa_id`) received in the Meta webhook. The customer's WhatsApp display name is used as the initial name value.

**Consequences (testable):**
- Profile creation is triggered solely by an inbound webhook event from Meta; the Platform does not poll or read the business's WhatsApp address book (the Cloud API does not expose this).
- If a profile already exists for that `wa_id`, the inbound event does not overwrite any fields the Admin/Executive has already completed.
- Newly created profiles have `whatsapp_opt_in = false` by default; opt-in must be explicitly collected (FR-49).
- Inbound message content is not stored by the Platform; only the sender's `wa_id` and display name are captured for profile creation.

#### FR-7A: Customer bulk import via CSV
Business Admin can upload a CSV file to bulk-import Customer Profiles into the Branch. The CSV must contain at minimum: mobile number. Optional columns: name, email, address, preferred language, city, loyalty tier.

**Consequences (testable):**
- CSV upload validates mobile number format (10-digit Indian mobile number or E.164); rows with invalid numbers are rejected with a row-level error report.
- Duplicate mobile numbers (matched against existing profiles) are skipped; Admin is shown a count of skipped duplicates.
- Imported profiles have `whatsapp_opt_in = false` and `email_opt_in = false` by default; opt-in must be separately collected.
- CSV import supports up to 5,000 rows per upload; larger files must be split.

#### FR-8: Manual customer add and edit
Business Admin and Business Executive can manually add a new Customer Profile or edit any field of an existing Customer Profile within their Branch.

**Consequences (testable):**
- Mobile number is the unique identifier; duplicate mobile numbers within a Branch are rejected with an inline error.
- All profile fields are editable except opt-in/opt-out dates (system-recorded on the event that triggers the change).
- Changes are saved immediately with no approval required.

#### FR-9: Customer Profile fields
Each Customer Profile contains: Salutation, Full name, Mobile number (required), Email, Address, WhatsApp opt-in status (boolean), WhatsApp opt-in date (system-set), WhatsApp opt-out status (boolean), WhatsApp opt-out date (system-set), Email opt-in status (boolean), Email opt-in date (system-set), Email opt-out status (boolean), Email opt-out date (system-set), Preferred language (dropdown: English, Hindi, Gujarati, Marathi, Tamil, Telugu, Bengali, Kannada, Malayalam, Punjabi), City/Branch tag, Loyalty tier (Bronze / Silver / Gold / Platinum — Admin-assignable).

**Consequences (testable):**
- Preferred language field drives which language version of an AI-generated Template is recommended for campaigns targeting that customer.
- Opted-out customers are excluded from Campaign delivery for the opted-out Channel; system enforces this at send time regardless of group membership.

#### FR-10: Customer Groups
Business Admin can create named Customer Groups manually (assign specific Customer Profiles) or via auto-segment rules (e.g., "all Gold tier customers in Pune"). Both types are available.

**Consequences (testable):**
- A Customer Profile can belong to multiple Groups simultaneously.
- Auto-segment Groups update dynamically as Customer Profiles change (e.g., a customer promoted to Gold tier is added to the Gold Group automatically).
- Manual Groups are static until explicitly edited.

#### FR-11: Opt-in and opt-out tracking
The Platform records opt-in and opt-out events per Customer Profile per Channel (WhatsApp and Email separately) with timestamps. Opted-out customers are automatically excluded from Campaign delivery for the relevant Channel at send time.

**Consequences (testable):**
- WhatsApp opt-in date is set when a customer completes the opt-in flow (FR-49); Email opt-in date is set when a customer clicks the opt-in confirmation link.
- Opted-out customers remain in Customer Groups but are silently excluded at send time; excluded count is reported in Campaign analytics separately for each Channel.
- WhatsApp opt-out cannot be manually overridden by an Admin. Re-opt-in for WhatsApp requires a customer-initiated action via a non-WhatsApp channel (web form or in-store QR) — messaging an opted-out WhatsApp number for any reason (including re-consent requests) violates Meta policy.
- Email opt-out cannot be manually overridden by an Admin. Re-opt-in for email requires the customer to click a re-subscribe link on the business's opt-in page.

#### FR-12: Opt-out / unsubscribe handling
Every outbound email includes a one-click unsubscribe link. Every WhatsApp campaign message includes an opt-out instruction (e.g., "Reply STOP to unsubscribe"). On receipt of STOP reply or unsubscribe link click, the Platform immediately sets the customer's opt-out status for that Channel.

**Consequences (testable):**
- Unsubscribe link in email is unique per customer per campaign; clicking it sets `email_opt_out = true` without requiring login.
- STOP reply on WhatsApp is processed within 60 seconds of receipt; `whatsapp_opt_out = true` is set immediately.
- Admin receives an in-app notification when a customer opts out.

#### FR-49: WhatsApp opt-in collection via shareable link
Business Admin can generate a unique, Branch-specific opt-in link that they share with customers (via existing WhatsApp messages, SMS, printed QR codes, or in-person). When a customer opens the link, they are shown a consent page explaining what they are opting into (promotional messages from the Business) and confirm via a button click.

**Consequences (testable):**
- Each Branch has one active opt-in link at a time; the link is stable (does not change on each visit).
- On customer confirmation: `whatsapp_opt_in = true` and `whatsapp_opt_in_date` are set; if a Customer Profile for that mobile number already exists, it is updated; if not, a new profile is created with the mobile number.
- Opt-in consent page displays the Business name, branch name, and a plain-language description: "You are agreeing to receive promotional messages from [Business Name] on WhatsApp."
- The opt-in link is accessible without platform login (customer-facing public page).
- Admin can view a count of opt-ins collected via the link in the Branch settings page.

#### FR-50: Email opt-in enforcement at send time
Before dispatching any email Campaign message to a Customer Profile, the Platform checks `email_opt_in = true` and `email_opt_out = false`. Customers failing this check are excluded from email delivery.

**Consequences (testable):**
- Email campaigns cannot be sent to customers with `email_opt_in = false` regardless of group membership.
- Excluded-by-email-opt-in count is shown in Campaign analytics separately from opted-out count.
- Admin can set `email_opt_in = true` only when the customer has provided explicit consent (e.g., at point of sale, via a web form); the field is not auto-populated.

---

### 4.3 AI Promotion Generation

**Description:** This is the core creative engine of the Platform. The Executive or Admin selects a Channel, provides a plain-English (or Hindi) description of the promotion, optionally picks a festival or season from the Calendar, and the AI generates a complete, channel-appropriate Promotion. For WhatsApp: a header image (pre-built layout with brand assets applied) + short body text + CTA button. For Email: a responsive HTML layout with full promotional design. The AI model is Anthropic Claude Haiku (cost-optimised; fast creative copy generation). Unsplash free API provides stock photography. Brand assets (logo, colors, tagline, contact info) are injected automatically. Up to 4 revision iterations are supported per generation session via natural language feedback.

**Functional Requirements:**

#### FR-13: Channel-first selection
Before initiating AI generation, the user must select the Channel (Email or WhatsApp). The selected Channel determines the output format and cannot be changed mid-generation.

**Consequences (testable):**
- Switching Channel after generation discards the current draft and prompts the user to confirm before proceeding.
- WhatsApp channel is only available if the Branch has a connected WhatsApp number (FR-5).

#### FR-14: Promotion description input
The user provides a plain-English or Hindi description of the promotion (minimum 10 characters, maximum 500 characters). The system accepts mixed English-Hindi input.

**Consequences (testable):**
- Descriptions shorter than 10 characters surface an inline validation error.
- AI generates content in the language selected by the user (FR-19); the description language does not need to match the output language.

#### FR-15: Indian Festival Calendar picker
The user can optionally select a festival or seasonal moment from the built-in Festival Calendar as additional context for AI generation. The Calendar contains at least: Diwali, Holi, Eid al-Fitr, Christmas, New Year, Onam, Navratri, Dussehra, Pongal, Ganesh Chaturthi, Gudi Padwa, Makar Sankranti, Back-to-School, Wedding Season, Pre-Monsoon Season, Monsoon Season, Summer, Year-End Clearance, Weekend Special.

**Consequences (testable):**
- Selected festival name and cultural context are passed to the AI as generation context.
- Festival Calendar dates are updated annually by the System Admin.
- Festival selection is optional; generation proceeds without it if skipped.

#### FR-16: AI template generation
On submission, the AI (Claude Haiku) generates one Promotion template tailored to the selected Channel, Industry, language, festival context (if provided), and description. Business profile data is auto-injected.

**Consequences (testable):**
- Template generation completes within 15 seconds under normal load.
- Generated WhatsApp template: header image slot + body text (max 1,024 characters) + CTA button (max 25-character label) + footer (opt-out instruction).
- Generated email template: responsive HTML with subject line, headline, body copy, image section, CTA button, footer (unsubscribe link, business contact).
- Business name, contact person, phone, email, address, logo, and brand colors are present in the generated output.
- If AI generation fails, the user sees a human-readable error and a Retry button; the failed attempt does not count against the 4-iteration limit.

#### FR-17: Pre-built graphic layout application
For each generated Promotion, the Platform selects the most contextually appropriate pre-built graphic layout from its library (organised by Industry × Occasion) and applies the Business's brand colors and logo. Stock photography is sourced from Unsplash free API.

**Consequences (testable):**
- Layout library contains a minimum of 50 templates at launch, covering all 25 Industries and at least the 10 most common festival/seasonal occasions.
- Brand primary color is applied to the layout's dominant color slot; accent color to the secondary slot.
- Logo is placed in the header of all layouts; if no logo is uploaded, the Business name is rendered as text.
- Unsplash photos are fetched at generation time; if the Unsplash API is unavailable, the Platform falls back to the layout's default placeholder imagery.

#### FR-18: AI revision iterations
After initial generation, the user can provide natural-language revision feedback (e.g., "make the headline larger," "add rain emoji," "translate to Gujarati") to refine the Promotion. Maximum 4 revision iterations per generation session.

**Consequences (testable):**
- Revision count is displayed to the user (e.g., "2 of 4 revisions used").
- After 4 revisions, the revision input is disabled; the user must save and manually edit, or start a new generation session.
- Each revision replaces the previous output; prior revision versions are not retained.

#### FR-19: Regional language support
The user selects an output language for AI-generated text content. Supported languages at launch: English, Hindi. Additional Indian languages (Gujarati, Marathi, Tamil, Telugu) are planned for v2.

**Consequences (testable):**
- Language selection determines the language of all AI-generated text (body copy, CTA, subject line).
- Hindi output uses Devanagari script (not transliteration). `[ASSUMPTION: Devanagari rendering in WhatsApp templates is confirmed to display correctly on common Indian Android devices.]`
- English-Hindi mixed output is supported when the description is in mixed language.

#### FR-20: Business profile data injection
Business profile fields (name, contact person, phone, email, address, logo, brand colors, tagline) are automatically available to the AI and applied to every generated Promotion without user action.

**Consequences (testable):**
- Promotions generated before a Business profile field is set will show a visible placeholder (e.g., "[PHONE NUMBER]") for missing fields.
- Updating the Business profile does not retroactively modify previously generated Promotions.

---

### 4.4 Template Library & Campaign Management

**Description:** Generated Promotions are saved as reusable Templates accessible by all Branches of a Business. A Campaign binds a Template to a target Customer Group, a Channel, a start date, an end date, and optionally a scheduled send time. Campaigns can be cloned to accelerate recurring seasonal efforts (e.g., cloning last year's Diwali campaign). Expired Campaigns are auto-archived.

**Functional Requirements:**

#### FR-21: Template save and library
Any saved Promotion becomes a Template in the Business-level Template Library, visible to all Branches. Templates are searchable and filterable by Industry, occasion/festival, Channel, and language.

**Consequences (testable):**
- Template Library is accessible from all Branch contexts.
- Search returns results matching any of: template name, description text, festival tag, Industry.
- Filter by Channel (Email / WhatsApp), language, and Industry can be combined.

#### FR-22: Template clone and customise
Any user (Admin or Executive) can clone an existing Template to create a new Draft, then re-enter the AI generation flow with the cloned content as the starting point for revisions.

**Consequences (testable):**
- Cloning creates a new Draft; the original Template is unchanged.
- Cloned Draft retains the original's Channel, language, and festival tag; all can be changed before re-generation.

#### FR-23: Campaign creation
Business Admin can create a Campaign by selecting a Template, a target (all Branch customers / a named Customer Group / a specific Branch), a Channel, a start date, an end date, and an optional scheduled send date/time.

**Consequences (testable):**
- Campaign cannot be created with a start date in the past.
- End date must be after start date.
- Campaign targeting options: All customers in Branch / Named Customer Group / All customers in a specific Branch (for multi-branch Businesses).

#### FR-24: Campaign scheduling
Campaign can be scheduled to send at a specific future date and time (e.g., "Send on Oct 1 at 8:00 AM"). The Platform delivers the Campaign automatically at the scheduled time without further user action.

**Consequences (testable):**
- Scheduled campaigns display countdown ("Sends in 3 days, 4 hours") on the Campaign list view.
- If a scheduled Campaign's WhatsApp Template has not yet received Meta approval at send time, delivery is held and Admin is notified immediately.
- Admin can reschedule or cancel a scheduled Campaign at any time before the send time.

#### FR-25: Campaign clone and reuse
Admin can clone any previous Campaign (including Archived ones) to create a new Campaign pre-filled with the same Template, targeting, and channel — with dates cleared for re-entry.

**Consequences (testable):**
- Cloning an Archived Campaign restores it as a new Draft with no delivery history attached.
- Clone action is available on any Campaign in any state.

#### FR-26: Auto-archive of expired campaigns
Campaigns whose end date has passed are automatically moved to Archived state, regardless of their current state (including Draft or Submitted). Archived Campaigns are visible in an "Archived" filter view but do not appear in active lists.

**Consequences (testable):**
- Auto-archive runs at midnight (IST) daily.
- Archived Campaigns cannot be edited or published but can be cloned (FR-25).
- Promotions in Submitted state that reach end date are archived and the Admin is notified.

---

### 4.5 Approval Workflow

**Description:** Every Promotion created by a Business Executive must be approved by a Business Admin before it can be published. Business Admin-created Promotions bypass the approval step (self-approval). The workflow is: Draft → Submitted (by Executive) → Approved or Rejected (by Admin). Rejection sends the Promotion back to Draft with Admin comments attached. The Executive may withdraw a Submitted Promotion at any time before the Admin acts on it. A full audit trail is maintained.

**Functional Requirements:**

#### FR-27: Promotion submission
Business Executive can submit a Draft Promotion for Admin approval. Submission is a single action from the Promotion editor.

**Consequences (testable):**
- Submitted Promotion is locked for editing by the Executive until Rejected or Withdrawn.
- On submission, Admin receives an in-app notification, an email notification, and a WhatsApp notification.
- Executive receives an in-app confirmation that submission was successful.

#### FR-28: Admin approval and self-approval
Business Admin can approve any Submitted Promotion. Admin-created Promotions can be self-approved directly (no second reviewer required for pilot). Approval moves the Promotion to Approved state, enabling Campaign creation.

**Consequences (testable):**
- Only users with Admin role can approve Promotions.
- On approval, the Executive who created the Promotion receives an in-app + email + WhatsApp notification.
- Admin cannot approve a Promotion that has already been approved or is in Archived state.

#### FR-29: Admin rejection with comments
Business Admin can reject a Submitted Promotion with a mandatory written comment explaining the rejection. Rejection returns the Promotion to Draft state with the comment visible to the Executive.

**Consequences (testable):**
- Rejection comment is mandatory (minimum 10 characters); rejecting without a comment is blocked.
- On rejection, the Executive receives an in-app + email + WhatsApp notification with the comment text.
- The Executive can view all rejection comments on a Promotion's history before re-editing.

#### FR-30: Executive withdrawal
Business Executive can withdraw a Submitted Promotion (before the Admin has acted on it) to return it to Draft state for editing.

**Consequences (testable):**
- Withdrawal is available only when Promotion state is Submitted.
- Withdrawing does not consume one of the 4 AI revision iterations.
- Admin receives an in-app notification that the Promotion was withdrawn.

#### FR-31: Promotion modification and resubmit
After rejection or withdrawal, the Business Executive can edit the Draft and resubmit for approval. Resubmission follows the same workflow as initial submission (FR-27).

**Consequences (testable):**
- Resubmission uses the same 4-iteration AI revision budget (iterations already used are counted).
- Rejection comments from prior rounds remain visible in the Promotion history.

#### FR-32: Approval audit trail
The Platform maintains a complete, immutable audit trail for every Promotion showing: who created it, every state transition (with timestamp and actor), all rejection comments, and who published the resulting Campaign.

**Consequences (testable):**
- Audit trail is accessible to Business Admin on any Promotion's detail view.
- Audit trail cannot be edited or deleted by any user including System Admin.
- Audit entries contain: timestamp (IST), actor name and role, action taken, and any comments.

#### FR-33: Workflow notifications
All workflow events (submission, approval, rejection, withdrawal, publication, opt-out received) trigger notifications to the relevant parties via in-app notification, email, and WhatsApp.

**Consequences (testable):**
- In-app notifications appear in a notification bell; unread count is shown.
- Email notifications are sent from `hello@fineintegrity.com`.
- WhatsApp workflow notifications are sent via the Platform's own WhatsApp integration (not the customer-facing Business WhatsApp number). `[ASSUMPTION: Fine Integrity will maintain a dedicated WhatsApp Business number for platform notifications.]`
- Notification delivery failure (e.g., WhatsApp not configured) does not block the workflow action; the action succeeds and the failed notification is logged.

---

### 4.6 Publishing & Delivery

**Description:** Approved Campaigns are published by the Admin, either immediately or at a scheduled time. WhatsApp delivery requires a Meta-approved WhatsApp Template before sending. The Platform handles Meta template submission automatically. Email is delivered via Azure Communication Services from `hello@fineintegrity.com`. Each customer in the target group receives the campaign once (deduplication across group membership). Failed messages are retried twice; persistent failures are reported to the Admin. Per-message delivery status is tracked and displayed.

**Functional Requirements:**

#### FR-34: Meta WhatsApp Template submission
When a Promotion is approved for a WhatsApp Campaign, the Platform automatically formats the Promotion as a Meta-compliant WhatsApp Template and submits it to Meta for approval via the WhatsApp Cloud API. The submission constructs a valid template payload including: a unique `name` (derived from Business slug + campaign title + timestamp), `category: MARKETING`, BCP-47 `language` code matching the Promotion's selected language, and a `components` array with parameterised `{{1}}`-style variable placeholders for dynamic fields (customer name, business contact, offer details). Template status is tracked in-app.

**Consequences (testable):**
- Meta Template submission is triggered automatically on Promotion approval; no separate user action is required.
- Template `name` is unique per WhatsApp Business Account (WABA); the Platform guarantees uniqueness by appending a timestamp suffix.
- Template status is visible on the Campaign detail view: Pending Meta Approval / Meta Approved / Meta Rejected.
- Meta approval typically takes a few hours; during peak festival season (Navratri, Diwali, Eid) it may take up to 48 hours. The UI communicates this range to the Admin.
- On Meta rejection: rejection reason code and description are displayed to Admin; the Promotion returns to **Draft** state (not Approved); the Admin can edit and resubmit, which triggers a new internal approval cycle (FR-27) before Meta resubmission. `[NOTE FOR PM: Consider whether Admin can self-approve the corrected version instantly (bypassing full re-approval) when the only change is Meta compliance wording — revisit after first pilot rejection experience.]`
- A Campaign cannot be published (or sent at scheduled time) until Meta approval is received.

#### FR-35: Campaign publishing
Business Admin can publish an Approved Campaign immediately ("Send Now") or let a previously scheduled send time fire automatically. Publishing dispatches the Campaign to all targeted, opted-in customers via the selected Channel.

**Consequences (testable):**
- "Send Now" is available only when Promotion state is Approved and (for WhatsApp) Meta Template is approved.
- Publishing triggers deduplication: if a customer appears in multiple targeted groups, they receive the message exactly once.
- Publishing is irreversible; a sent Campaign cannot be recalled.

#### FR-36: Email delivery
Email Campaigns are sent via Azure Communication Services from `hello@fineintegrity.com`. Every email includes an unsubscribe link (FR-12) and the Business's contact footer.

**Consequences (testable):**
- Email subject line is taken from the AI-generated template's subject field.
- Emails are sent as responsive HTML (renders correctly on mobile and desktop).
- Delivery receipt (sent / delivered / bounced) is captured per message within 60 seconds of dispatch.

#### FR-37: WhatsApp delivery
WhatsApp Campaigns are sent via the Meta WhatsApp Cloud API using the Branch's connected WhatsApp number and the Meta-approved WhatsApp Template. Only customers with `whatsapp_opt_in = true` and `whatsapp_opt_out = false` receive the message.

**Consequences (testable):**
- Messages not sent to opted-out customers (FR-11) or non-opted-in customers (FR-49) are counted as "Excluded (opt-out / not opted-in)" in Campaign analytics.
- WhatsApp delivery status (sent / delivered / read) is retrieved from Meta webhooks and stored per message.
- WhatsApp marketing messages (business-initiated template sends) are charged by Meta from message one at the India tier rate (approximately ₹1.00–1.25 per conversation); there is no free quota for marketing conversations. The Platform's plan-based message quota (FR-52) governs sending limits, not a Meta free tier.
- Admin is warned when the Branch has used 80% of its plan's included WhatsApp message quota for the month; Campaign publishing is blocked when 100% is reached until the next billing cycle or a plan upgrade.

#### FR-38: Delivery retry and failure handling
Failed message deliveries (invalid number, email bounce, WhatsApp delivery error) are retried automatically up to 2 times. After 2 failed retries, the message is marked as "Failed" and the Admin is notified in-app.

**Consequences (testable):**
- Retry interval: 10 minutes between first and second retry.
- "Failed" status is visible per message in the delivery status view (FR-45).
- Admin notification includes the customer name, mobile/email, and failure reason.

#### FR-39: Customer deduplication
If a customer belongs to multiple Customer Groups targeted by the same Campaign, they receive the Campaign message exactly once.

**Consequences (testable):**
- Deduplication is performed at send time on the merged recipient list.
- Deduplicated send count is reported in Campaign analytics ("X unique recipients").

#### FR-40: Per-message delivery status
Every message dispatched as part of a Campaign has an individual delivery status record visible to Admin and Executive (read-only). Statuses: Sent / Delivered / Read (WhatsApp) or Sent / Delivered / Opened (Email) / Failed.

**Consequences (testable):**
- Status is updated in near-real-time (within 5 minutes of status change) via Meta webhooks (WhatsApp) and Azure Communication Services delivery receipts (Email).
- Status view is filterable by status type.

#### FR-51: Tenant data isolation
All Business data (Customer Profiles, Promotions, Campaigns, analytics, brand assets) is scoped exclusively to the owning Business tenant. No cross-tenant data access is permitted at any layer.

**Consequences (testable):**
- All database queries are tenant-scoped; a query for Branch A's customer list cannot return data belonging to Branch B or any other Business.
- API endpoints reject any request where the authenticated session's Business ID does not match the requested resource's Business ID, returning HTTP 403.
- System Admin can view Business-level metadata (tenant name, plan status, usage counts) but cannot access Customer Profiles, Promotions, or Campaign content of any Business.

#### FR-52: Billing metering — send ledger
Every message dispatched (WhatsApp or Email) is recorded in an immutable send ledger with: timestamp (IST), Branch ID, Campaign ID, Channel, customer mobile/email (hashed for privacy), and delivery status. This ledger is the authoritative source for usage-based billing calculations.

**Consequences (testable):**
- Send ledger entries are written before message dispatch; a failed dispatch is recorded with status "Failed."
- Ledger records cannot be edited or deleted by any user including System Admin.
- Monthly usage totals derived from the ledger are accessible to Admin in Branch Settings; they match invoice line items exactly.
- Ledger is retained for a minimum of 24 months for billing dispute resolution.

---

### 4.7 Analytics & Reporting

**Description:** Campaign performance data is available to both Admin and Executive (Executive is read-only). Analytics cover delivery, engagement, and conversion (link clicks). A high-level dashboard gives trend visibility across all Campaigns. Side-by-side Campaign comparison supports iterative learning across festival seasons. Reports are exportable as CSV.

**Functional Requirements:**

#### FR-41: Campaign performance dashboard
A dashboard displays aggregate Campaign metrics for the selected Branch: total Campaigns sent (current month), total unique recipients reached, best-performing Campaign (by open/read rate), and a timeline chart of Campaign activity.

**Consequences (testable):**
- Dashboard is scoped to the active Branch context.
- Metrics update within 15 minutes of new delivery status events.
- "Best performing" is ranked by (Read count / Delivered count) for WhatsApp and (Opened / Delivered) for Email.

#### FR-42: Click tracking
All links embedded in Promotions are automatically wrapped with a Platform tracking redirect at generation time. Click events are captured per customer per Campaign when the redirect fires.

**Consequences (testable):**
- Original URLs are preserved for the customer; the redirect is transparent.
- Click count and list of clicking customers (name + mobile) are visible in Campaign analytics.
- Link tracking works for both email and WhatsApp CTA buttons.

#### FR-43: WhatsApp reply visibility
Customer replies to WhatsApp Campaign messages are captured and displayed in a Replies inbox within the Campaign detail view, visible to Admin.

**Consequences (testable):**
- Replies appear within 60 seconds of receipt via Meta webhook.
- STOP replies are automatically processed as opt-outs (FR-12) and not shown as conversational replies.
- Replies are read-only in v1; responding from the Platform is out of scope.

#### FR-44: CSV export
Admin can export Campaign analytics (all metrics: recipient list, delivery status, open/read status, click status) as a CSV file.

**Consequences (testable):**
- CSV export is available on any completed or active Campaign.
- CSV contains one row per recipient; columns: customer name, mobile, email, delivery status, read/opened status, clicked (Y/N), opt-out triggered (Y/N).
- Export file is generated within 30 seconds for Campaigns up to 5,000 recipients.

#### FR-45: Campaign comparison
Admin can select any two Campaigns and view a side-by-side comparison of their performance metrics (delivery rate, read rate, click rate, opt-out rate).

**Consequences (testable):**
- Comparison is available for any two Campaigns in any state (active, completed, archived).
- Comparison view shows absolute numbers and percentage rates for each metric.
- Campaigns on different channels (Email vs WhatsApp) can be compared; metrics are labelled per channel.

---

### 4.8 System Administration

**Description:** The System Admin (Fine Integrity) manages the Platform's shared configuration: the Industry list, the Festival Calendar, Business tenants, and Platform-wide monitoring. System Admin has a separate login at `admin.fineintegrity.com`.

**Functional Requirements:**

#### FR-46: Industry list management
System Admin can add, edit, deactivate, and reorder Industries in the Platform dropdown. At launch, 25 Indian-market Industries are seeded.

**Consequences (testable):**
- Active Industries appear in the registration dropdown and as AI generation context.
- Deactivating an Industry does not remove it from existing Business profiles; it only removes it from the dropdown for new registrations.
- Industry list is ordered alphabetically by default; System Admin can override display order.

**Seeded industry list (v1):**
Fashion / Apparel, Electronics & Mobile Retail, Home & Kitchen, Jewelry, Books & Stationery, Quick Service Restaurants (QSR), Bakeries & Confectionery, Cloud Kitchens / Food Delivery, Cafes & Dessert Shops, Specialty Food / Organic Products, Salons & Beauty, Health & Wellness, Restaurants & Dining, Travel & Tourism, Real Estate, B2B Manufacturing / Trading, Education & Coaching, Healthcare Clinics & Diagnostics, Automotive Services, Home Services, SaaS / Software Services, Digital Marketing Agencies, Logistics & Courier, Event Planning & Decorators, Insurance Agents.

#### FR-47: Business tenant management
System Admin can view all registered Business tenants, their subscription status, usage metrics (campaigns sent this month, WhatsApp messages used vs cap), and can suspend or reactivate a Business.

**Consequences (testable):**
- Suspended Business tenants cannot log in or send Campaigns; their subdomain shows a "Suspended" page.
- System Admin can view but not edit Business-level data (customer profiles, promotions).

#### FR-48: Festival Calendar management
System Admin can add, edit, and deactivate Festival Calendar entries, including setting the calendar date for each recurring festival annually.

**Consequences (testable):**
- Festival dates are updated at least annually before the festival season.
- Deactivated festivals are removed from the picker without affecting past Campaigns that referenced them.

---

## 5. Non-Goals (Explicit)

- **SMS channel** — Not in v1. WhatsApp and email cover the pilot use cases; SMS adds TRAI DLT registration complexity.
- **Native mobile application** — Not in v1. Mobile-responsive web covers the pilot. App store deployment is v2.
- **AI-generated images (DALL-E / Stable Diffusion)** — Not in v1. Pre-built graphic layouts + Unsplash stock photos deliver professional results at zero per-image cost.
- **Multi-language Platform UI** — Not in v1. UI is English only; AI-generated *content* supports Hindi and regional languages.
- **A/B testing of promotional variants** — Not in v1.
- **Two-way WhatsApp conversations / chatbot** — Not in v1. WhatsApp replies are visible (FR-43) but responding from the Platform is out of scope.
- **Custom email sending domain for businesses** — Not in v1. All email sends from `hello@fineintegrity.com`.
- **Advanced ML-based customer segmentation** — Not in v1. Customer Groups are rule-based and manual.
- **Third-party CRM or e-commerce integration** — Not in v1.
- **PDF campaign report export** — Not in v1. CSV only.
- **In-app payment processing** — Not in v1. Billing handled externally (Razorpay or equivalent) with plan managed by System Admin.
- **Multi-language regional UI beyond English** — Not in v1.

---

## 6. MVP Scope

### 6.1 In Scope (Pilot v1)

- Business self-registration (Gmail OAuth, email verification, mobile OTP)
- Auto-provisioned subdomain on `fineintegrity.com`
- Business profile with logo, brand colors, tagline
- Branch creation and management with WhatsApp number per branch
- Customer profile management (manual + WhatsApp Business contact sync)
- Customer Groups (manual + auto-segment by profile attributes)
- Opt-in / opt-out tracking and enforcement (WhatsApp + Email), TRAI-compliant
- AI promotion generation (Claude Haiku) — Email and WhatsApp formats
- Indian Festival Calendar (13+ occasions)
- Pre-built graphic layouts (50+, by industry + occasion)
- Hindi and English AI generation
- Up to 4 AI revision iterations per generation session
- Business profile data auto-injection into promotions
- Template Library (shared at Business level)
- Campaign creation with start/end date and scheduling
- Campaign clone and reuse
- 4-role system: System Admin, Business Admin, Business Executive, Viewer
- Approval workflow with rejection comments, withdrawal, and audit trail
- In-app + email + WhatsApp workflow notifications
- Meta WhatsApp Template submission and approval tracking
- Email delivery via Azure Communication Services (`hello@fineintegrity.com`)
- WhatsApp delivery via Meta Cloud API (within free tier, 1,000 messages/month/number)
- Delivery retry (2 attempts) + Admin failure notification
- Per-message delivery status (sent / delivered / read or opened)
- Campaign performance dashboard
- Click tracking on promotion links
- WhatsApp reply inbox (read-only)
- CSV export of campaign analytics
- Side-by-side Campaign comparison
- Pricing: flat monthly tiers — Starter ₹999, Growth ₹1,999, Business ₹3,499 per Branch/month; 1-month free trial (max 5 Campaigns per Branch, all features unlocked)
- Azure-hosted infrastructure (Central India / Pune region)
- 3-business pilot: AutoCare Services (automotive), Shree Ladies Garments, Fresh Basket (grocery)
- Meta Business Platform verification for all 3 pilot businesses must begin by September 1, 2026 (verification takes 2–7 days; required before any WhatsApp campaign can be sent)

### 6.2 Out of Scope for MVP

- SMS channel `[NOTE FOR PM: revisit if pilot businesses request it — TRAI DLT registration is the blocker, not platform complexity]`
- Native mobile app (iOS / Android)
- AI-generated images (DALL-E / Stable Diffusion)
- Additional regional languages beyond English and Hindi (Gujarati, Marathi, Tamil, Telugu) `[NOTE FOR PM: High-engagement opportunity — consider as first v2 feature based on pilot business language preferences]`
- A/B testing of Promotion variants
- Two-way WhatsApp conversation / chatbot flow
- Custom email sending domain per business
- Third-party CRM / e-commerce integration
- In-app billing and payment processing
- PDF report export
- Advanced ML-based segmentation

---

## 7. Success Metrics

**Primary**

- **SM-1:** Pilot activation rate — All 3 pilot businesses complete onboarding (profile, branch, WhatsApp connection, at least 1 customer imported) within 2 weeks of launch. Validates FR-1 through FR-7.
- **SM-2:** Campaign usage during free trial — Each pilot business publishes at least 2 Campaigns before free trial ends. Validates FR-16, FR-28, FR-35.
- **SM-3:** AI generation satisfaction — At least 70% of generated Promotions are submitted for approval without requiring more than 2 revision iterations. Validates FR-16, FR-18.

**Secondary**

- **SM-4:** WhatsApp delivery rate — At least 90% of dispatched WhatsApp messages reach "Delivered" status. Validates FR-37, FR-38.
- **SM-5:** Approval workflow completion — At least 80% of submitted Promotions reach Approved state within 48 hours. Validates FR-28, FR-29.
- **SM-6:** Link click-through rate — At least 10% of WhatsApp campaign recipients click the CTA link. Validates FR-42.
- **SM-7:** Free trial → paid conversion — At least 1 of 3 pilot businesses converts to paid plan after free trial. Validates overall product value.

**Counter-metrics (do not optimise)**

- **SM-C1:** Opt-out rate — Do not optimise for campaign volume at the cost of opt-outs exceeding 2% per Campaign. High opt-outs signal spam-like behaviour and risk WhatsApp account suspension.
- **SM-C2:** AI revision overuse — Do not optimise AI speed if it degrades output quality. If average iteration count climbs above 3, it signals the first-pass quality is insufficient.
- **SM-C3:** Meta Template rejection rate — Keep Meta Template rejections below 10%. High rejection rates delay campaigns and signal that the AI is generating non-compliant content.

---

## 8. Open Questions

1. **WhatsApp re-opt-in after STOP:** Meta prohibits messaging opted-out numbers for any reason, including re-consent requests. The only valid re-opt-in path is customer-initiated via a non-WhatsApp channel (the shareable opt-in link, FR-49, or in-store QR). The UI must make this clear to Admins — confirm this is the intended UX before architecture.
2. **Billing and payment integration:** Razorpay is the recommended payment gateway for UPI + card. Confirm before architecture begins. For the 3-business pilot, is manual invoicing (Razorpay payment link sent by email) acceptable until in-app billing is built?
3. **Meta Business verification for pilot businesses:** Will Fine Integrity provide hands-on onboarding support to guide all 3 pilot businesses through Meta's WhatsApp Business Platform verification (2–7 days)? This must start by September 1, 2026.
4. **Platform notification WhatsApp number:** Fine Integrity needs a dedicated WhatsApp Business number for sending workflow notifications (approval, rejection, publication alerts) to business users. This number is separate from any pilot business number. Confirm this is in place before build.
5. **Unsplash API rate limits:** Unsplash free API allows 50 requests/hour. What is the fallback imagery strategy if the limit is hit during a busy generation period? (Options: cached image library, placeholder branded imagery, paid Unsplash plan.)
6. **Festival Calendar date accuracy:** Who updates festival dates annually? System Admin task or automated from a public Indian calendar API? Recommend automating to avoid missed updates.
7. **Meta rejection re-approval path:** When Meta rejects a template post-approval (FR-34), the Promotion returns to Draft and must go through full re-approval. For pilot where Admin is likely the same person, should Admin be able to self-approve the corrected version instantly? Decide before build.

---

## 9. Assumptions Index

- **§4.5 FR-33 [ASSUMPTION]:** Fine Integrity will maintain a dedicated WhatsApp Business number for sending platform workflow notifications (approval, rejection, publication alerts) to business users. → *Confirm resource and operational plan (OQ-4).*
- **§4.3 FR-19 [ASSUMPTION]:** Hindi Devanagari script renders correctly in WhatsApp Template messages on common Indian Android devices (Samsung, Redmi, Realme). → *Confirm with device testing before pilot launch.*
- **§6.1 [ASSUMPTION]:** In-app billing and payment processing are out of scope for v1; the 3-business pilot will be managed with manual invoicing or a Razorpay payment link after the free trial period. → *Confirm before pilot free trials expire (OQ-2).*
- **§4.6 FR-37 [ASSUMPTION]:** WhatsApp marketing messages (business-initiated) are charged by Meta from message one at the India tier rate (~₹1.00–1.25/conversation). No free quota applies to marketing sends. → *Verify current Meta India pricing before finalising plan quotas in Appendix C.*
- **§4.3 FR-34 [ASSUMPTION]:** Meta WhatsApp Template approval takes a few hours under normal conditions and up to 48 hours during peak festival season. Platform UX and campaign scheduling must accommodate this range. → *Monitor actual approval times during pilot and adjust messaging.*

---

## Appendix A: Cross-Cutting NFRs

| Attribute | Requirement |
|---|---|
| Uptime | 99.5% (≤ 3.6 hours/month unplanned downtime) |
| Page load | < 3 seconds at P95 on a 4G mobile connection |
| AI generation | < 15 seconds P95 response time (FR-16) |
| Mobile | Fully responsive; all workflows operable on a 375px viewport (iPhone SE) |
| HTTPS | All traffic TLS 1.2+ enforced; HTTP redirected to HTTPS |
| Authentication | Session timeout after 30 minutes of inactivity; re-authentication required |
| OTP security | Max 3 OTP attempts per 15-minute window before lockout |
| Data encryption | All customer data encrypted at rest (AES-256) and in transit (TLS 1.2+) |
| Data residency | All data stored in Azure Central India (Pune) region |
| Tenant isolation | Business A's data is inaccessible to Business B at all layers; enforced at query and API levels (FR-51) |
| Scalability | Architecture supports 50+ Business tenants without rewrite |
| Secrets management | All API keys and secrets stored in Azure Key Vault; never in code or config files |

---

## Appendix B: Compliance & Regulatory

| Requirement | Applies To | Implementation |
|---|---|---|
| TRAI DND / Opt-out | Email + WhatsApp | Unsubscribe link in every email; STOP instruction in every WhatsApp message; opt-out processed within 60 seconds |
| WhatsApp Opt-in | WhatsApp delivery | Only opted-in customers receive WhatsApp Campaigns; opt-in status enforced at send time (FR-11, FR-37) |
| Meta WhatsApp Template policy | WhatsApp Campaigns | All promotional messages use Meta-approved templates; AI content generation guided to avoid common rejection triggers (excessive urgency, false claims, excessive caps) |
| Indian IT Act / Data Protection | Customer data storage | Customer PII stored in Azure Central India (Pune); no cross-border transfer; data deletion available on Business account closure |
| Email CAN-SPAM / Unsubscribe | Email Campaigns | One-click unsubscribe in every email footer; processed within 10 minutes |

---

## Appendix C: Pricing & Monetization

**Model:** Flat monthly subscription tiers per Branch. Indian SMBs require predictable costs; per-message pricing creates budget anxiety and positions the platform unfavourably against free WhatsApp Business broadcast.

| Tier | Monthly fee (per Branch) | Included WhatsApp msgs | Included emails | Overage (WhatsApp) | Overage (email) |
|---|---|---|---|---|---|
| **Free Trial** | ₹0 (1 month) | Up to 500 | Up to 2,000 | Not available | Not available |
| **Starter** | ₹999/month | 500 | 2,000 | ₹1.50/msg | ₹0.25/email |
| **Growth** | ₹1,999/month | 1,500 | 6,000 | ₹1.25/msg | ₹0.20/email |
| **Business** | ₹3,499/month | 3,500 | 15,000 | ₹1.00/msg | ₹0.15/email |

**Free trial:** 1 month from Branch activation; maximum 5 Campaigns published per Branch; all features fully unlocked. No payment method required to start.

**Billing unit:** Per Branch (each Branch billed independently).

**Payment methods:** UPI + credit/debit card (Razorpay — confirm gateway before architecture).

**Cost basis (indicative per WhatsApp message sent):**

| Component | Cost |
|---|---|
| Meta Cloud API (India marketing tier) | ~₹1.00–1.25/conversation |
| Platform overhead (hosting, amortised) | ~₹0.15 |
| AI generation (amortised per send) | ~₹0.02 |
| **Total cost** | **~₹1.17–1.42** |
| **Starter tier effective rate** (500 msgs @ ₹999) | **₹2.00/msg** — ~40–70% margin |

*Note: WhatsApp marketing messages (business-initiated template sends) are charged by Meta from message one; there is no free marketing conversation quota. The 1,000 free monthly conversations on the Meta Cloud API apply only to user-initiated (service) conversations.*

---

## Appendix D: Platform & Infrastructure

| Component | Choice | Rationale |
|---|---|---|
| Cloud | Microsoft Azure | Owner (Nilesh) has deep Azure expertise; operational efficiency outweighs ~10% cost premium vs AWS |
| Region | Central India (Pune) | Indian data residency; lowest latency for Indian users |
| App hosting | Azure App Service (B1) | Right-sized for pilot; scales to higher tiers without rewrite |
| Database | Azure Database for PostgreSQL Flexible Server (B1ms) | Managed, cost-effective, familiar |
| File storage | Azure Blob Storage | Logo, brand assets, generated template images |
| Email | Azure Communication Services | Native Azure integration; cost-effective for low-volume pilot |
| Secrets | Azure Key Vault | All API keys (Meta, Anthropic, Unsplash) |
| AI model | Anthropic Claude Haiku | Fastest, cheapest model suitable for creative promotional copy generation |
| WhatsApp | Meta WhatsApp Cloud API (direct) | No BSP intermediary; lowest cost; 1,000 free service conversations/month per number |
| Stock photos | Unsplash free API | Zero cost; high-quality photography; subject to 50 req/hour rate limit |
