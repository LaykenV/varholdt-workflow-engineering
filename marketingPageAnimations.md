# Marketing Page Animations — Implementation Handoff

This is the implementation spec for adding motion and visual anchors to the marketing page at `src/pages/index.astro`. It exists because the page is currently text-heavy and the hero demo is too niched-down (RFP-specific) when the actual product is broader.

The goal is to land **three** new visual moments, in a single consistent motion language. Not six, not ten. Resist scope creep.

---

## Reference

- **Design vibe:** [cyndra.ai](https://www.cyndra.ai/) (AI employees connected to tools, live agent activity), [t3.codes](https://t3.codes/) (concrete app panels, tangible product surface, no marketing prose).
- **Current page:** `src/pages/index.astro`
- **Current styles:** `src/styles/global.css` (already has `prefers-reduced-motion` blocks starting around line 1577 — extend the pattern, don't re-invent it)
- **Marketing copy data:** `src/data/marketing.ts`

---

## Design language — non-negotiable

**Vibe:** *Serious operator, quiet magic, visible systems.* Not cartoony AI mascots. Not SaaS confetti. Not cyberpunk neon. One motion language across the whole page:

> Work enters → gets understood → gets reviewed → lands back in the right tool.

Every animation on this page should be a literal or abstract expression of that sentence. If a proposed animation doesn't fit it, don't ship it.

---

## Tech constraints

- **Astro project.** No new framework runtime. Astro's "zero JS by default" isn't violated by small vanilla `<script>` tags — that's the islands model. The existing page already ships ~1.5 KB of vanilla JS (nav scroll state, IntersectionObserver reveals, KPI tickers, pipeline cycler). Match that pattern.
- **CSS-first.** Two of the three builds are 100% CSS keyframes + SVG. Only the hero tab-switcher needs JS (~30 lines vanilla).
- **No new dependencies.** No Framer Motion, no GSAP, no Anime.js, no Lottie. Native CSS animations, SVG `stroke-dasharray` / `offset-path`, and a small click handler.
- **GPU-only animated properties.** `transform` and `opacity` only. Never animate `width`, `height`, `top`, `left`, `margin`, `padding`.
- **`will-change`** sparingly — only on the hero beam paths and any continuously-animated SVG.

---

## Animation primitives — use these exactly

### Easing curves
```css
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);    /* refined, smooth */
--ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);   /* default for this page */
--ease-out-expo:  cubic-bezier(0.16, 1, 0.3, 1);    /* confident, decisive */
```

**Forbidden:** bounce (`cubic-bezier(0.34, 1.56, 0.64, 1)`), elastic, anything overshoot. They feel dated and undermine the "serious operator" vibe.

### Durations by purpose
| Purpose | Duration |
|---|---|
| Instant feedback (button press, toggle) | 100–150 ms |
| State change (hover, tab swap) | 200–300 ms |
| Layout change (accordion, modal) | 300–500 ms |
| Entrance (hero, scroll reveal) | 500–800 ms |
| Ambient loop (beam packet, card preview) | 3000–6000 ms |

Exit animations = ~75% of the matching entrance duration.

### Accessibility — required, not optional
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```
Plus per-component fallbacks:
- **Hero beam:** static gradient lines, no packet animation.
- **Workflow triage:** show the final "Sprint candidate ✓" state immediately, skip scoring sequence.
- **Agent card previews:** show the final frame as a static illustration.

---

## Build 1 — Hero

**Status of current hero:** the demo dashboard (`src/pages/index.astro` lines 205–334) is too dense and reads as RFP-specific. Replace it with a hybrid: signature beam above + slimmer scenario demo below.

### 1a. Signature animated beam (above the headline area, replaces dashboard top half)

Three-column SVG composition:

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  INPUTS      │         │              │         │  OUTPUTS     │
│              │         │              │         │              │
│  • Slack     │ ─path─→ │   ┌──────┐   │ ─path─→ │  • Word      │
│  • Outlook   │ ─path─→ │   │AGENT │   │ ─path─→ │  • Sheets    │
│  • SharePnt  │ ─path─→ │   │ ◉    │   │ ─path─→ │  • Salesforce│
│  • Gmail     │ ─path─→ │   └──────┘   │ ─path─→ │  • Teams     │
│              │         │              │         │              │
└──────────────┘         └──────────────┘         └──────────────┘
```

- **Left column:** 4 source chips, stacked vertically. Reuse the existing `simple-icons` SVGs from the integration chips data.
- **Center:** a single "Agent" disc — soft pulsing concentric ring (slow 4s breath), slowly rotating dashed border (~30s rotation, almost imperceptible). Label underneath: "Agent · reviewing".
- **Right column:** 4 destination chips.
- **Paths:** 4 curved SVG paths from each left chip → center, and 4 from center → each right chip. Soft 1px stroke, low-opacity color.
- **Packets:** small glowing dots travel along each path. Use CSS `offset-path: path("...")` matching the SVG path `d` attribute. Stagger packet emission: ~600ms between each launch on a 6s cycle. Packets should appear to "merge" at the center disc (fade out as they reach it, fade in on the other side at the same moment for the center→output leg).
- **Path-draw entrance:** on hero load (after `is-ready` class fires), each path animates its `stroke-dasharray` from `0, totalLength` to `totalLength, 0` over 800ms with `ease-out-quint`, staggered 80ms between paths.
- **Headline overlay copy stays as-is.** The beam visual sits where the dashboard currently does.

### 1b. Slimmer scenario demo (below the headline, replaces dashboard bottom half)

A single card with a 4-tab strip at the top. Clicking a tab swaps the entire card body — not just labels. The card body has 4 zones in a row, each separated by an arrow:

```
┌────────────┬────────────┬────────────┬────────────┐
│  INPUT     │  AGENT     │  HUMAN     │  OUTPUT    │
│  arrives   │  acts      │  approves  │  lands     │
└────────────┴────────────┴────────────┴────────────┘
```

**The 4 tabs and their content:**

| Tab | Input | Agent | Human approves | Output |
|---|---|---|---|---|
| **RFP Shred** | SAM.gov notice received | Extracting L/M/C requirements with citations | PM approves requirements interpretation | Compliance matrix → Excel · SharePoint |
| **Claims Intake** | Claim submission arrived | Matching loss event to policy clauses | Adjuster approves coverage summary | Coverage memo → claims system · adjuster inbox |
| **Resume Match** | 24 resumes hit careers@ | Scoring against open requisition rubric | Recruiter approves shortlist | Top candidates → ATS · hiring channel |
| **Drafting Bench** | RFI received from agency | Drafting response in house voice | SME approves every paragraph | Draft response → Word · proposal folder |

**Interaction:**
- Default state: `RFP Shred` is active.
- Click another tab → card body crossfades (200ms fade-out + 250ms fade-in, slight 6px upward translate on entrance) using `ease-out-quint`.
- Active tab gets the existing primary accent treatment. Inactive tabs are muted.
- Optional ambient hint: every 8s, if the user hasn't interacted with a tab yet, briefly pulse the next-most-relevant tab to indicate it's clickable. Stop the hint as soon as one click happens.

**JS scope:** ~30 lines vanilla — tab click handler that toggles an `active` class on the tab and the matching `[data-scenario="..."]` body. No state library, no framework.

### Hero acceptance criteria
- [ ] Beam paths draw in on hero load, staggered.
- [ ] Packets travel continuously, merge at center disc.
- [ ] Center disc has a slow breath pulse + slow dashed rotation.
- [ ] 4 tabs are clickable, body crossfades.
- [ ] `prefers-reduced-motion`: beam shows static paths, packets removed, center disc static, tab swap instant.
- [ ] Mobile: beam stacks vertically (inputs top, agent middle, outputs bottom) OR scales down cleanly. Tab strip becomes horizontally scrollable if it overflows.

---

## Build 2 — Intro / "Find the workflow worth automating"

**Current state:** `src/pages/index.astro` lines 354–396. Two columns: a paragraph + 3 numbered points. Visually inert.

**Add a right-column visual: workflow triage.**

This visualizes the section's literal thesis — you don't automate randomly, you pick the workflow with leverage.

### Composition

```
        Candidates              Scoring lane            Result
      ┌─────────────┐      ┌──────────────────────┐    ┌──────────────┐
      │ Email triage│ ───→ │ Vol  Rep  ROI        │ ───┐│              │
      │ RFP response│ ───→ │ ███  ██   █████      │    ││  Sprint      │
      │ Expense cls │ ───→ │ ██   ███  ██         │    ││  candidate ✓ │
      │ Status rpts │ ───→ │ █    █    █          │    ││              │
      │ Resume scrn │ ───→ │ ████ ████ ███        │    │└──────────────┘
      └─────────────┘      └──────────────────────┘
```

### Animation sequence (single loop, ~6s total, restarts)

1. **0.0s — Candidates enter.** 5 workflow tiles slide in from the left, staggered 100ms apart. Each is a small card with a workflow name and a workflow-type icon.
2. **1.0s — Scanner pass.** A thin horizontal gradient line sweeps top→bottom across the scoring lane over ~1.2s.
3. **As the scanner crosses each row, three score bars fill** for that row: `Volume`, `Repeatability`, `ROI`. Bars animate from 0 → final width over 300ms with `ease-out-quint`.
4. **2.5s — Dimming pass.** The 4 weaker candidates fade their opacity from 1 → 0.3 over 400ms.
5. **3.0s — Winner advances.** The remaining candidate (RFP response) slides right out of the scoring lane into a "Sprint candidate ✓" badge card. Small green checkmark draws in.
6. **5.5s — Reset.** Everything fades out over 300ms. Loop restarts at 0.0s.

### Candidate list (exact)
- Email triage — Vol █ / Rep █ / ROI █ → dims
- **RFP response — Vol ████ / Rep ████ / ROI █████ → WINS**
- Expense classification — Vol ██ / Rep ███ / ROI ██ → dims
- Status reports — Vol █ / Rep █ / ROI █ → dims
- Resume screening — Vol ████ / Rep ████ / ROI ███ → dims (close runner-up, important for credibility)

The point: the visual literally shows the deliberation. It's not random.

### Layout
- Keep the existing left column (`.intro-statement` + `.intro-points`).
- The triage visual replaces nothing — it's an addition to the right side of the existing grid, or becomes a third column if space allows. Reflow gracefully on mobile (visual stacks below the points, or hide entirely if vertical space is constrained).

### Intro acceptance criteria
- [ ] 5 candidates enter on stagger, scanner pass sweeps the lane, score bars fill.
- [ ] 4 candidates dim, winner advances to "Sprint candidate ✓".
- [ ] Loop runs continuously, ~6s cycle.
- [ ] `prefers-reduced-motion`: show the final state (4 dimmed candidates + 1 winner) statically, no scanner, no bar animation.
- [ ] Section title `Find the workflow worth automating.` still reads first; visual supports it, not vice versa.

---

## Build 3 — Agent cards / "Four agents we ship over and over"

**Current state:** `src/pages/index.astro` lines 432–479. Four `.build` cards, currently text-only. Lines 6–43 define the data — keep that structure.

**Add a small "preview screen" at the top of each card** — a ~120px tall illustrated zone showing the agent doing its thing in a short loop. Pause on hover so users can study it.

### Per-card preview specs

#### Card 1 — RFP Response Agent
**Composition:** a stylized document page (rounded rectangle, light fill). Inside: 6 horizontal text-line bars (gray, varied widths).

**Loop (~4s):**
1. 0.0s — All gray.
2. 0.4s — Line 2 highlights (yellow underlay). Tiny citation marker `[¹]` pops in at the right end (scale 0 → 1, 200ms).
3. 0.8s — Line 4 highlights. Marker `[²]` pops in.
4. 1.2s — Line 5 highlights. Marker `[³]` pops in.
5. 3.6s — Highlights and markers fade out over 400ms.
6. 4.0s — Restart.

#### Card 2 — Drafting Workbench Agent
**Composition:** a document page with 4 "section blocks" stacked vertically. A blinking cursor sits at the top.

**Loop (~4.5s):**
1. 0.0s — All section blocks empty (light gray placeholders). Cursor blinks at top.
2. 0.5s — Cursor jumps to section 1. Section 1 fills with text bars left-to-right (scaleX 0 → 1 with `transform-origin: left`) over 600ms. Tiny "House voice ✓" pill appears next to it.
3. 1.5s — Same for section 2.
4. 2.5s — Same for section 3.
5. 3.5s — Cursor returns to top, sections empty out (reverse fill, faster — ~300ms).
6. 4.5s — Restart.

#### Card 3 — Resume Screening Agent
**Composition:** 3 stacked resume cards on the left (small avatars + name bars), one big circular match-score gauge on the right.

**Loop (~4.5s):**
1. 0.0s — Resume 1 highlighted (subtle border). Gauge animates 0 → 87% (arc draws via `stroke-dasharray`, number ticks 0 → 87) over 800ms.
2. 1.2s — Resume 2 highlighted. Gauge animates 87 → 92.
3. 2.4s — Resume 3 highlighted. Gauge animates 92 → 73.
4. 3.6s — Top 2 resumes get a small "✓" badge (the shortlisted ones).
5. 4.5s — Restart.

#### Card 4 — Claims Prep Agent
**Composition:** a claim form (left, ~60% width) with 4 field rows. A small "Policy" sidebar on the right (~40%) with 4 clause rows.

**Loop (~4.5s):**
1. 0.0s — Static.
2. 0.4s — Clause 2 in the sidebar gets a green underline (scaleX 0 → 1, `transform-origin: left`, 300ms).
3. 0.7s — A thin SVG line draws from clause 2 to field 1 in the form (stroke-dasharray draw, 400ms). Field 1 highlights briefly.
4. 1.5s — Clause 3 underlines. Line draws to field 3.
5. 2.5s — Clause 1 underlines. Line draws to field 4.
6. 4.0s — Lines and underlines fade out.
7. 4.5s — Restart.

### Implementation notes
- Each preview is a self-contained SVG. Aim for 4 separate small components or inline SVGs in the Astro file (whichever keeps `index.astro` readable — if it balloons, extract to `src/components/AgentPreview*.astro`).
- All animations are CSS keyframes. No JS per card.
- Use the existing card color tokens — don't introduce new palette.
- **Pause on hover:** `.build:hover .agent-preview { animation-play-state: paused; }` on every animated element inside the preview.

### Card acceptance criteria
- [ ] All 4 cards have distinct, on-message preview animations.
- [ ] All previews loop continuously and pause on card hover.
- [ ] Card text content (Reads / Does / Approves / Outputs) is unchanged.
- [ ] `prefers-reduced-motion`: previews show the final/peak frame statically.
- [ ] No layout shift — previews have a fixed height (~120px desktop, can scale on mobile).

---

## Explicitly NOT in scope

These were considered and **cut**. Do not silently add them back.

| Section | Why cut |
|---|---|
| **Integrations chip wall (`03b — Lives in your tools`)** | Static chips already read fine. Adding orbital motion duplicates the hero's beam metaphor and dilutes the signature moment. Leave as-is. |
| **10-day sprint timeline rail** | Existing cards are clean and scannable. A rail behind them is decoration, not communication. |
| **Proof project tile grid** | The 3 stat cells punch hard. Adding 8 tiles forces real screenshots to be maintained, and placeholder thumbnails would look weaker than the current stats. |
| **AI cartoon illustrations anywhere** | Off-brand. Would undermine credibility. |
| **Confetti, particle effects, scroll parallax, cursor effects** | Off-vibe. |
| **Any new JS framework or animation library** | Astro project, vanilla only. |

If, after the 3 builds ship, the page still feels under-animated, revisit individually. Don't bundle.

---

## Build order

Ship in this order, **each as a separate commit**, so we can preview after each step and back out anything that doesn't land:

1. **Hero** — replace the existing demo dashboard with the beam + scenario demo. Biggest signature moment, sets the tone for the rest.
2. **Agent cards** — add preview SVGs to each card. Biggest section-level visual upgrade.
3. **Intro workflow triage** — adds an anchor to the text-heaviest section.

After each step, verify in browser at the relevant viewport sizes (320, 768, 1024, 1440).

---

## Global checklist before calling it done

- [ ] All 3 builds ship and pass their per-section acceptance criteria.
- [ ] `prefers-reduced-motion: reduce` produces a coherent static page — no broken layouts, no missing context.
- [ ] No layout properties animated (`width`, `height`, `top`, `left`, `margin`, `padding`).
- [ ] No bounce/elastic easings anywhere.
- [ ] Page weight: new JS adds <2 KB minified. New CSS adds <8 KB minified.
- [ ] 60fps on a mid-range laptop in Chrome devtools, all loops.
- [ ] Mobile: beam stacks/scales, scenario tabs scroll horizontally if needed, agent previews scale with cards, triage visual stacks or hides gracefully.
- [ ] Nothing blocks user interaction during animations (no full-screen overlays, no disabled buttons during transitions).
