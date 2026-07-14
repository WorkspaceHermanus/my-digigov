# MyDigiGovSA — Live site vs. Figma mockup

An audit of every deviation found between the deployed site and the `DigiGovSA Portal`
Figma prototype, and what was changed to close each one.

**Reference:** Figma frame `MyDigiGovSA` — **1920 × 10170**, content column **1200px**.
**Baseline audited:** `my-digigov.jakkies.workers.dev` (commit `ad6c74f` in this repo).

The two builds share the same 11-section skeleton in the same order, so this is drift
within a shared structure rather than a different site. Section IDs map 1:1:

| Live section | Mockup section |
|---|---|
| `#home` | Hero |
| `#vision` | Completing SA's Digital Government Vision |
| `#resources` | Today's Government Employee Experience |
| `.services` | One Secure Digital Workplace |
| `#how-it-works` | How Trusted Credentials Work |
| `#journey` | A day with Lerato Nkosi |
| `.integrations` | Built on DigiGovSA |
| `#impact` | Value that compounds across the system |
| `#roadmap` | Aligned to SA's Digital Transformation Roadmap |
| `#faqs` | Built as a practical demonstration |
| `#get-involved` | Final CTA |

---

## 1. Defects (not just drift)

These are bugs in the live build, independent of the mockup.

### 1.1 The hero headline was invisible — white text on a white background

The hero carried an inline style:

```html
<section class="hero section" style="background: url('assets/images/HERO-bg-design.jpg')
         no-repeat top center; background-size: contain;">
```

Two faults compounded:

1. The **`background` shorthand reset `background-color`**. `.hero` sets
   `background: var(--color-navy-dark)` in `sections.css`; the inline shorthand overrode it
   and reset the colour to `transparent` (confirmed: computed `background-color` was
   `rgba(0, 0, 0, 0)`).
2. **`background-size: contain`** on a **2181 × 941** image inside a **720px-tall** section
   cannot fill it. At a 1536px viewport the image renders 1536 × 662, leaving a bare strip.

The uncovered area therefore fell back to white — and `.hero__title` is `color: #fff`.
The `<h1>`, the lead paragraph and both buttons were effectively invisible. Only the green
`<span>Serve South Africa</span>` survived, which is why the live hero read so strangely.

**Fixed** by restoring the `.hero__background` layer (already present in `sections.css`, but
commented out of the HTML), pointed at the real photo with `cover` and a navy left-to-right
scrim.

### 1.2 The hero phone was dead markup

`assets/images/Lerato-profile-01.png` is already a pixel-match for the mockup's phone —
bezel, notch, and full app UI on a transparent background. It sat inside a block of
commented-out `<div class="device-phone__shell">` markup and never rendered as intended.
**Fixed:** promoted to a real `.hero__phone` element.

### 1.3 Nav links pointed at sections that did not exist

The `journey` and `roadmap` sections had **no `id`**. Any nav or footer link to them was a
dead anchor. **Fixed:** added `id="journey"` and `id="roadmap"`.

### 1.4 Mobile rules were silently dead

`responsive.css` targeted `.dashboard-card__body`, `.integration-row__meta`,
`.device-phone--hero`, `.hero__flag`, `.brand__name`, `.quick-grid` and
`.process-grid::before` — several of which no longer matched the markup — so those mobile
rules did nothing. `.footer__grid` also laid **4** footer columns into a **3**-column grid.
**Fixed:** retargeted onto the current markup and corrected the column count.

---

## 2. Global / design-system deviations

| # | Element | Live (before) | Mockup | Status |
|---|---|---|---|---|
| 2.1 | Section eyebrow | Plain green uppercase text | **Pill** — light grey, rounded, with a leading **green dot** | Fixed |
| 2.2 | Content container | `1180px` | **`1200px`** | Fixed |
| 2.3 | Header height | `4.4rem` (70.4px) | **78px** | Fixed |
| 2.4 | `h1` | capped at **68.8px** | **80px** / line-height 0.95 | Fixed |
| 2.5 | `h2` | capped at **53.6px** | **48px** / line-height 1.1 | Fixed |
| 2.6 | Em dashes | Every `—` replaced with `-` | `—` throughout | Fixed |

**2.1** is the highest-leverage of these — the eyebrow pill repeats on all 11 sections, so
one component fixed eleven places.

**2.4 / 2.5** mattered structurally, not just cosmetically: because the headings were too
large they wrapped onto more lines than the mockup (the hero `h1` ran to 4 lines instead of
3; "— not another system" ran to 3 instead of 2). The scale is now anchored so that each
ceiling **is** the design value and is reached by ~1232px — the width at which the 1200px
container stops shrinking. Above that, the content column is typographically identical to
the mockup.

---

## 3. Per-section deviations

### Header
- Nav was `Home · Our Vision · How It Works · Impact · Resources · FAQs · Get Involved`.
  Mockup: **`Vision · Benefits · How it Works · Demo · Roadmap · Executive Mandate`**.
- CTA was **"Launch App"**. Mockup: **"Request Demo →"** (with arrow).
- Logo was an unsemantic `<span>` wrapping an `<img>` at `width:50%`. Now a linked brand
  image at the design's 234px.

### Hero
- Lead: `"...verified identity, credentials, self-services and AI in one secure experience"`
  → mockup **`"...verified identity, credentials, HR services and AI into one secure experience"`**.
- Buttons: `"Explore the Platform"` / `"View How It Works"` →
  **`"Follow Nomsa's Journey →"`** / **`"Watch 3 Minute Demo"`**.
- Trust badges: `Verified credentials · Secure workplace · Connected services` →
  **`Mobile Accessible · Verified Credentials · Employee First`**.

### Vision
- Eyebrow `"One national platform"` → **`"Complete the human interface"`**.
- Lead named **MyMzansi before MyDigiGovSA**; the mockup names **MyDigiGovSA first**.
- Centre card titled `"Secure government layer"` → **`"Shared infrastructure"`**.
- Stack chips ended in a **green dot**; the mockup uses a **lock icon**.
- Cards were capped at `820px`; the mockup spans the full 1200px container.
- Side cards carried a description paragraph; the mockup shows
  **"Citizen Services" / "Employee Services"** plus a *"Connects to shared infrastructure"*
  connector link (arrow pointing inward on each side).

### The Problem
- Eyebrow `"The current state"` → **`"The problem"`**.
- The whole card was different: live had a tab strip (`HRMIS/Payroll/Email/Training/Leave`)
  above three tiles. The mockup shows a **chain of chips joined by arrows** —
  `PERSAL → Payroll → Email → Training → Policies → Manual HR → Physical Cards → Dept. Websites`.
- Stat values (12+, 8, 40%, Rare) were already correct.

### The Solution
- Eyebrow `"One secure workplace"` → **`"The solution"`**.
- Card 4 was **"Payroll"**; the mockup says **"Payslips"**.
- Four descriptions differed:
  - Verified Employee Card: "Portable, **trusted** proof" → "Portable, **tamper-proof** proof"
  - Leave: "in **moments**" → "in **seconds**"
  - Payslips: "Access payslips, tax records and deductions" → "**Instant access to earnings and tax documents**"
  - Training: "Learning and compliance pathways in one place" → "**Learning that updates your credentials automatically**"
  - Notifications: "**Critical updates for** everything that matters" → "**One trusted channel for** everything that matters"

### How Trusted Credentials Work
- Eyebrow `"A trust model"` → **`"How it works"`**.
- **All four steps were wrong.** Live: `Verify / Present / Match / Complete`.
  Mockup: **`Verify / Personalise / Work / Recognise`** — with entirely different body copy.
- Icon tiles were circles; the mockup uses **rounded squares**.
- A decorative connecting hairline across the four cards does not exist in the mockup.

### A day with Lerato Nkosi
- Eyebrow `"A day in service"` → **`"Day in the life"`**.
- Job title **"Senior Administration Officer"** → **"Senior Financial Systems Analyst"**.
- **The entire timeline differed** — 7 generic entries (07:30–17:00) replaced with the
  mockup's 6: `07:00 Open MyDigiGovSA`, `08:00 Verified access`, `11:00 DG Briefing`,
  `14:00 Review Implementation Risks`, `14:01 Credential updated`,
  `16:00 Recognition` (with a *Ten Years of Service · Badge issued* pill).
- **The phone showed the wrong screen** — a credential *wallet*. The mockup shows a
  **Training** screen: My Training / Completed tabs, two in-progress courses with progress
  bars (60% / 20%), a "Recommended for You" list, a CPD-points note, and a
  *"Continue learning"* button. Rebuilt.

### Built on DigiGovSA
- Eyebrow `"The trusted layer"` → **`"Architecture"`**.
- Lead: "MyDigiGovSA **does not replace what works**" → "MyDigiGovSA **doesn't replace what exists**".
- **Wrong component.** Live was a 6-row grid of named government systems
  (Government Identity, National Payments, HR and Payroll, …). The mockup is a
  **layered architecture stack joined by downward arrows**:
  `Human Interface → Trusted Channels → Digital Identity → Credential Wallet →
  Data Exchange → Payments → Government Systems` (last row filled green), each row carrying
  **tag pills** rather than a description and a status word.

### Benefits
- Eyebrow `"System-wide value"` → **`"Benefits"`**.
- **All 16 list items differed.** e.g. Public Servants was
  `Single sign-in / Recognition / Transparent status / Mobile-first access`;
  mockup is **`Supported / Recognised / Productive / Secure`**.
- Bullets were **dots**; the mockup uses **outlined check icons**.

### Roadmap
- Eyebrow `"Strategic alignment"` → **`"Executive alignment"`**.
- Card structure differed: live was *title + description*; the mockup is a small
  **"INITIATIVE"** label above a bold name.
- Names differed: live had `Government / Digital ID / Trusted Credentials / AI Enablement /
  Service Interoperability / Public Service Reform`. Mockup: **`Digital Public Infrastructure /
  Digital Identity / Trusted Credentials / IFMS Modernisation / Operation Vulindlela /
  Public Service Reform`**.

### Proof of Concept
- Heading used a hyphen: `"- not another system"` → **em dash `"— not another system"`**.
- Lead: "designed to be **explored, tested and refined** by government incrementally and
  safely" → "designed to be **adopted, extended and scaled** by government—incrementally
  and safely".
- **Six steps, not five.** Live: `Discover / Plan / Prototype / Test / Learn / Scale`
  (each with a sub-label). Mockup: **five adoption stages** —
  `Today / Pilot / Presidency endorsement / National rollout / Government-wide`,
  no sub-labels, first step filled green.
- Tags were `Identity, Education, Human affairs, Finance, Skills, LMS, …` →
  **`Health, Education, Home Affairs, Treasury, SARS, SAPS, Justice, Transport,
  Public Works, DPSA, Presidency`**.

### Final CTA
- Eyebrow `"Ready to collaborate?"` → **`"The next chapter of digital government"`**.
- Lead: "by **improving the everyday working experience of the people doing the work**" →
  "by **empowering the people who deliver public services every day**".
- Buttons: `"Request Executive Walkthrough"` / `"Explore Documentation"` →
  **`"Request Executive Mandate →"`** / **`"Schedule Demonstration"`**.

### Footer
- **Wrong columns entirely.** Live: `Product / Resources / Legal / Contact` (with a phone
  number, email and address). Mockup: **`Initiative / Government / Foundations`**.
- Brand was a text lockup; the mockup uses the **logo image on a white badge**.
- Blurb and bottom bar rewritten to the mockup's wording.

---

## 4. Known gaps — not yet closed

Stated plainly rather than left to be discovered:

1. **Exact colour, letter-spacing and shadow values are inferred, not read.** Figma Dev Mode
   inspection requires a signed-in Figma session, and no API token was available. Type sizes,
   the 1920 frame and the 1200 container were measured directly off the prototype at 100%
   zoom and are trustworthy; individual hex values and letter-spacing are matched by eye and
   may be a hair off. A read-only Figma token would let these be verified exactly.

2. **Lerato's avatar** in the journey section is still the generic `avatar-lerato.svg`
   illustration. The mockup uses a photograph.

3. **Hero background crop.** The mockup and the live build use the same photo, but the
   mockup's subject sits slightly further left. The scrim gradient is matched by eye.
