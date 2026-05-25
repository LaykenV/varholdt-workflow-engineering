# Marketing Page Upgrade Plan

**Date:** 2026-05-24
**Target file:** `src/pages/index.astro`
**Source:** Synthesis of two independent reviews against the current page and `businessPlan.md` v6.
**Anti-pivot note:** This is a marketing-surface update (copy, ordering, trust signals). It does not touch ICP, offer structure, or pricing — so it does not trigger the Part 13 anti-pivot rule.

---

## Framing

**Who reads this page** (per businessPlan Part 4):
- Workflow owner at a 20–250 person professional services firm — proposal lead, ops manager, recruiting director, claims supervisor, partner at a CPA firm.
- Not technical. Skeptical of AI hype. Has already tried SaaS, the messy middle remains.
- Skim-reads. Decides in ~15 seconds whether to keep scrolling.
- The fear behind every section: *"Will this create a bigger mess than the one I have?"*

**The current page is a "founder operating plan exposed as a landing page."** It reads like it was written for another AI consultant or a CTO, not for a 52-year-old proposal director.

**What we're optimizing for:**
- Self-qualification within section 1–2 (so wrong-ICP readers leave fast, right-ICP readers commit early)
- Trust signals *before* pricing
- Concrete "I can see the agent doing my work" moment
- Low-friction next step

**Buyer journey we want the page to deliver:**
*pain recognition → concrete workflow → proof in motion → trust posture → low-risk first call*

---

## Final section order

| # | Section | Change |
|---|---|---|
| 1 | Hero | Rewrite — outcome-led, not category-led |
| 2 | Problem / Fit | **Moved up from #6** — segments self-qualify here |
| 3 | The Agents | Reorder — Document Intake first |
| 4 | Demo / Loom | **Moved down from #2** — lands harder after agents are concrete |
| 5 | Lives in your tools | Merge current intro point #01 + standalone Integrations section into one |
| 6 | How it works | Reduce retainer detail; plainer sprint copy |
| 7 | Proof + Founder | Reframe proof; soften founder line |
| 8 | Trust strip | **NEW** — human review, citations, audit logs |
| 9 | Final CTA | Tighten |

**Cuts:**
- Standalone Integrations section (chips move to #5 with new copy)
- Heavy retainer card detail (founding-rate justification, hosting cap explanation → move to the call)
- "Find the workflow worth automating" intro section — open question (see below)

---

## Phase 1 — Restructure (no copy decisions)

1. Move `<!-- Who it's for -->` block from current position (after Integrations) to immediately after Hero.
2. Move `<!-- Loom -->` block to after `<!-- What we build — the agents -->`.
3. Delete the standalone `<!-- Integrations strip -->` section header + lede. Move the chip grid into the existing intro point #01 ("Lives in the tools you already use") OR into a tightened single section after Loom.
4. Reorder agent cards in the `builds` array: Document Intake → RFP Response → Resume Screening → Claims Prep. (Drafting Workbench can stay or move to a secondary "also common" row.)
5. Add `Document Intake Agent` card — needs Reads/Does/Approves/Outputs spec from `businessPlan.md` Part 6 Agent #2, plus a new `agent-preview-N` visual.
6. Insert empty Trust Strip section between Founder and CTA (content in Phase 3).

Each step is independently shippable.

---

## Phase 2 — Copy rewrites

### Hero

| Before | After |
|---|---|
| **H1:** Install AI workflow agents without hiring an AI team. | **H1:** Turn document-heavy workflows into human-reviewed AI agents. |
| **Sub:** Human-reviewed agents that read your documents, draft and route the predictable work, and live in the tools your team already uses — Outlook, Slack, Teams, SharePoint, your CRM. Not another app. | **Sub:** We build agents that read RFPs, resumes, claims, emails, and source documents, then draft, route, and prepare the next step inside the tools your team already uses. Every important output stays human-reviewed, cited, and auditable. |

Keep both CTA buttons and the meta-row (free audit, $5K sprint credit, 2 founding spots).

### Problem / Fit (now section 2)

Replace current lede ("20–250 person teams with recurring document and email volume...tried-SaaS-but-the-messy-middle-remains energy") with:

> If your team is still reading PDFs, copying fields into spreadsheets, rewriting the same responses, or hunting through shared drives for source material, you probably don't need another AI app. You need one recurring workflow cleaned up, instrumented, and put under human control.

Keep the six segment cards unchanged.

### Agents section

| Before | After |
|---|---|
| **Title:** Four agents we ship over and over. | **Title:** Four agents already running in production. |
| **Lede:** Each one reads what it should read... ships one in your sprint, then expands. One at a time. No parallel scope creep. | **Lede:** Each agent reads what it should read, does what it should do, hands the decision back to a human, and writes the result into the tool you already use. We ship one in your sprint, then expand. |

Reorder cards. New first card content (from `businessPlan.md` Part 6 Agent #2):

```
{
  tag: "Intake",
  title: "Document Intake Agent",
  reads: "Inbound emails, attachments, scanned forms, source docs",
  does: "Classifies the document, extracts structured fields with confidence scores, routes to the right queue or system",
  approves: "Low-confidence extractions and exception cases",
  outputs: "CRM · ATS · claims system · accounting software · review queue",
  foot: ["Confidence-scored", "Exception routing"],
}
```

Visual: needs a new `ap-intake` preview component (suggest: inbound envelope → classified tags → extracted-field card with confidence bars).

### Demo / Loom

| Before | After |
|---|---|
| **Title:** Watch a real federal RFP get parsed in under five seconds. | **Title:** Watch an RFP become a reviewable compliance matrix. |
| **Lede:** Two minutes. One real public RFP from SAM.gov. Ingested, requirements extracted with citations, compliance matrix generated. The same engine ships inside our retainer engagements. | **Lede:** A real public RFP from SAM.gov: ingested, extracted with citations, compliance matrix generated. The same engine handles claims, resumes, and intake workflows shown above. |

(The "under five seconds" line is technically impressive but speed isn't the buyer's trust question. The buyer cares: *did it extract the right things, cite sources, and produce something a human can review?*)

### Lives in your tools (merged)

| Before | After |
|---|---|
| **Title:** No new app. Your tools. | (keep) |
| **Lede:** ...No Zapier in a trench coat. | **Lede:** We build the workflow around the tools your team already uses. One primary interface. One export destination. Clear human approval points. A lightweight review UI only where control specifically requires one. |

Keep the chip grid. Drop the standalone Integrations section wrapper after the merge.

### How it works

Reduce retainer detail. Keep:
- Price ($6,000/mo, 6-mo min, $10K standard strike)
- "1 core workflow shipped and tuned"
- "Monthly training + ROI report"
- "Quarterly roadmap reviews"

Cut from the card (move to the call):
- Hosting/token cap line
- "1–2 supporting automations or integrations"
- Founding-rate justification paragraph in the foot

Sprint card — replace consultant-speak:

| Before | After |
|---|---|
| Current-state map of one document-heavy workflow with stakeholder interviews | Map the workflow with your team |
| Working prototype of the highest-leverage slice — live, demo-able, not slideware | Build the part that matters most — live, not slides |
| 90-day roadmap and buy, build, or integrate memo with ROI assumptions | Recommendation: buy, build, or integrate — with ROI assumptions |
| 60-min leadership training, recorded for your team | 60-min team training, recorded |

### Proof

| Before | After |
|---|---|
| The portfolio proves we can ship. The founding sprints will prove we produce client ROI — we're shipping the second piece in public. | The proof today is shipped software: production systems, federal document workflows, and a live RFP parser. Founding engagements are priced to turn that implementation proof into client ROI case studies. |

### Founder

Keep almost as-is. Two small changes:
- Remove "Family of five." from the credibility paragraph (it's brand texture, not buyer-relevant proof). Optional: move to founder-stats footer as a "Based in / Family / etc." line.
- Drop the internal stack list ("Convex, TanStack Start, Vercel AI SDK, Gemini, Claude, Vapi, Stripe") — buyer doesn't know what these are. Replace with: *"production systems built on the same stack you'll see in the sprint."*

### Final CTA

| Before | After |
|---|---|
| **Title:** Thirty minutes. One workflow. A real conversation. | **Title:** Bring one workflow your team does every week. |
| **Body p1:** Free 30-minute workflow audit. We'll walk one workflow that slows your team down every week — who owns it, how often it runs, what you've already tried. If an agent makes sense, we'll scope a sprint on the call. | **Body p1:** In 30 minutes, we'll talk through what's automatable, what should stay human-owned, and whether a paid sprint makes sense. |
| **Body p2:** If a SaaS tool is the right answer, I'll tell you which one. No deck, no discovery framework, no pitch. | **Body p2:** If a SaaS tool is the right answer, I'll tell you which one. |

(Drop "no pitch" — the buyer knows it's still a sales call. Asserting otherwise reads as defensive.)

---

## Phase 3 — Trust strip (NEW section)

Insert between Founder and CTA. Same visual rhythm as the hero meta-row.

Title (small kicker, not a full H2): **What you get either way**

Five line items with simple icons:
- Human review by default
- Source citations where documents are used
- Audit logs for important actions
- No autonomous high-stakes decisions
- Sensitive workflows scoped before build

This directly answers the quiet fear: *"Will this thing send the wrong thing to a client or create compliance risk?"* That fear currently goes unanswered on the page.

---

## Vocabulary rules (apply throughout)

**Use more:** reviewable · cited · audit trail · inside Outlook/Teams/SharePoint/CRM · one workflow · human approval · working prototype · source documents · recurring · document-heavy.

**Use less:** Install · AI team (as a primary frame) · fractional (in the hero) · clever metaphors.

**Cut entirely from buyer-facing copy:** internal stack names (Convex, Vapi, Gemini, MCP, Vercel) · "Zapier in a trench coat" · "messy middle energy" · "no pitch" · "we ship over and over."

---

## What we're NOT changing

- Pricing structure ($5K sprint, $6K/mo founding, 14-day credit mechanic)
- ICP segments and weighting
- Agent four-line spec format (Reads / Does / Human approves / Outputs to)
- Layken's bio facts (veteran, DOL, Youngsville)
- The visual design system, color palette, typography
- The 10-day sprint timeline section

---

## Decisions (locked 2026-05-24)

1. **Find→Run interactive demo → demote into How it works.** Move the entire `<!-- Intro -->` section block (the findrun card + intro-points) out of position #2 and into the How it works section, framed as "How we pick the workflow during the sprint." Keep the supporting intro-points (lives in tools, buy/build/integrate, human review) — they relocate into the Trust Strip and Lives in your tools sections naturally.

2. **Document Intake Agent visual → design new now.** Build a new `ap-intake` preview matching the four existing agent visuals. Pattern: inbound envelope/email icon → classification tags being applied → an extracted-field card with confidence bars. Same layout grid, same animation rhythm as the existing `ap-claims` and `ap-rfp` components. Add to Pass 1 scope.

3. **Loom → re-caption only.** No re-recording. The new title ("Watch an RFP become a reviewable compliance matrix") + new lede ("...same engine handles claims, resumes, and intake workflows shown above") carry the cross-segment signal. Pass 2 scope.

4. **"Family of five" → remove entirely** from the page. Cut from the founder credibility paragraph; do not relocate. Reserved for outreach conversations where the personal context is earned. Pass 2 scope.

---

## Execution order

| Pass | Scope | Review before next? |
|---|---|---|
| 1 | Restructure: section reorder, demote Find→Run into How it works, cut redundant Integrations, add Document Intake card **with finished visual** | Yes |
| 2 | All Phase 2 copy rewrites (incl. Loom re-caption, remove "Family of five") | Yes |
| 3 | Trust strip section | Yes |

All four open questions are answered. No remaining decisions blocking execution.
