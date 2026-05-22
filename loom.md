# Varholdt AI Loom Plan

## Decision

Record a 2-minute Loom anchored on OmniBid, but frame OmniBid as proof of the Varholdt AI workflow pattern, not as a SaaS product.

The viewer should leave thinking:

> This person can take one ugly document workflow in my company and turn it into a reviewable, traceable AI-assisted process.

Do not make this a portfolio reel. Do not explain the stack. Do not apologize for OmniBid being an MVP. The video has one job: make the free 30-minute workflow audit feel worth booking.

## Core Framing

OmniBid is the proof artifact. Varholdt AI is the offer.

Say:

> This is one workflow agent I built: an RFP Response Agent. It reads a public federal solicitation, extracts the requirements, traces each item back to the source document, and produces a compliance matrix for human review.

Do not say:

> This is my SaaS.
> This is a client system.
> This is still an MVP.
> This replaces a proposal manager.

If needed, say:

> This is a working build study and operator tool. The same workflow pattern is what I install for client teams during a sprint.

## Demo Page Details

Record the OmniBid portion from:

```txt
http://localhost:3000/demo/rfp-agent
```

Implementation notes from the OmniBid repo:

- Route file: `/Users/laykenvarholdt/projects/omnibid/src/routes/_authed/demo/rfp-agent.tsx`
- Main component: `/Users/laykenvarholdt/projects/omnibid/src/routes/_authed/demo/-rfpAgentDemo.tsx`
- Demo fixture/config: `/Users/laykenvarholdt/projects/omnibid/src/routes/_authed/demo/-demoData.ts`
- Route is implemented under the authenticated route tree, but TanStack exposes it as `/demo/rfp-agent`.
- App chrome is hidden on this route, so the Loom should only show the demo canvas.
- Sign in before recording. Do not show the sign-in flow.
- The queued public solicitation is Library of Congress solicitation `030ADV26R0008`.
- The page uses a 15.5-second replay-style workflow animation, then loads a pinned completed Convex run when available and falls back to fixture rows if not.
- The fixture has 19 requirements. Do not use the old "70+ requirements in under five seconds" line for this recording.
- The UI shows source references as visible pills in the matrix. There is no PDF source jump or citation drawer in the current demo page, so do not narrate one.
- The "Excel · Word · SharePoint" surface is a destination label, not a working export control. Do not click it or describe export as already shipped.

## 2-Minute Run Of Show

### 0:00-0:12 — Who This Is For

On screen: OmniBid demo page title state at `http://localhost:3000/demo/rfp-agent`.

Script:

> If your team spends hours reading PDFs, copying requirements into spreadsheets, or drafting the same documents over and over, this is the kind of workflow Varholdt AI builds.

### 0:12-0:25 — What Varholdt AI Does

On screen: stay on the clean demo page. Do not show a portfolio page.

Script:

> We build human-reviewed AI workflow agents for document-heavy teams. They read the source documents, extract the work that matters, draft or route the next step, and keep a person in the approval loop.

### 0:25-0:32 — Set Up The Example

On screen: demo page input panel with Library of Congress solicitation `030ADV26R0008` pre-filled.

Script:

> This example is OmniBid, an RFP Response Agent. I am using a real public Library of Congress solicitation from SAM.gov.

Click `Run Agent`.

### 0:32-1:28 — The Proof Moment

On screen: purpose-built demo workflow view.

Show these beats in order:

1. Ingest state: solicitation is resolved and source document is acquired.
2. Extraction state: requirements are pulled from the RFP.
3. Matrix state: requirement rows appear grouped by category.
4. Citation proof: pause on one row and point out the source reference pill.
5. Human review: show status controls or review queue language.

Script:

> The agent resolves the solicitation, reads the RFP, and turns the messy document into a structured compliance matrix. The important part is traceability: every requirement carries a source reference so a human reviewer can verify it before anything leaves the team.

Optional line if the matrix is visible and the count reads cleanly:

> This run produced 19 reviewable requirements across mandatory clauses, evaluation factors, personnel rules, deliverables, formatting, and certifications.

### 1:28-1:47 — Pattern, Not Product

On screen: matrix still visible. The footer says "Varholdt AI · Human-reviewed document workflow agents."

Script:

> The point is not that every client needs an RFP parser. The point is the workflow pattern: read messy documents, extract the fields and obligations that matter, draft or route the next step, and keep human approval where judgment matters.

Then say only three examples:

> For a staffing firm, that might be resumes against a requisition. For an insurance team, claims against a policy. For an accounting firm, client source docs into a review queue.

### 1:47-2:03 — Single CTA

On screen: Varholdt AI CTA or a final demo-page CTA strip.

Script:

> I am opening two founding engagements. The front door is a free 30-minute workflow audit. Bring one document-heavy workflow your team does every week, and I will tell you what is automatable, what probably is not, and whether a sprint makes sense.

## Recording Rules

- Use one story, one workflow, one CTA.
- Keep the talking-head intro under 15 seconds. Screen share is the proof.
- Pre-pick the RFP. Do not browse SAM.gov live.
- Preload `http://localhost:3000/demo/rfp-agent` and confirm the input shows `030ADV26R0008`.
- Hide browser bookmarks, notifications, account menus, and unrelated tabs.
- Do not show code, terminal output, Convex dashboards, API logs, or environment variables.
- Do not show OmniBid login/account UI.
- Do not use words like "maybe," "still building," "rough," or "eventually."
- Do not say this is a fresh live parse. The demo page uses a 15.5-second replay of a completed run for recording reliability.
- Do not promise Excel/PDF export, confidence scoring, source-document jump behavior, or a citation drawer. They are not visible in the current demo.
- Re-record until the final take is under 2:10. Ideal final length: 1:50-2:05.

## What To Show

Show:

- A clean input state with a public SAM.gov solicitation.
- A visible workflow progression: ingest, extract, review, output.
- Requirement rows grouped into clear categories.
- Source citations or section references.
- Human-review status controls.
- The destination label: Excel · Word · SharePoint.
- A final CTA to book the free workflow audit.

Avoid:

- Portfolio carousel.
- Multi-project montage.
- Civicly, TeachMagic, FoodTruckFlow, or Atlas cameos.
- Architecture diagrams.
- Stack lists.
- Pricing detail beyond "free 30-minute workflow audit" and, if necessary, "$5K sprint."

## Backup Script

Use this if recording needs to be read almost word-for-word:

> If your team spends hours reading PDFs, copying requirements into spreadsheets, or drafting the same documents over and over, this is the kind of workflow Varholdt AI builds.
>
> We build human-reviewed AI workflow agents for document-heavy teams. They read source documents, extract the work that matters, draft or route the next step, and keep a person in the approval loop.
>
> This example is OmniBid, an RFP Response Agent I built around public federal solicitations. This is a real Library of Congress solicitation from SAM.gov, packaged as a clean demo of the workflow.
>
> The agent resolves the solicitation, reads the source document, and extracts the requirements into a structured matrix. The important part is traceability: every row carries a source reference, so a human reviewer can verify the requirement before it becomes part of the proposal workflow.
>
> The point is not that every client needs an RFP parser. The point is the pattern: read messy documents, extract the fields and obligations that matter, draft or route the next step, and keep human approval where judgment matters. For a staffing firm, that might be resumes against a requisition. For an insurance team, claims against a policy. For an accounting firm, client source docs into a review queue.
>
> I am opening two founding engagements. The front door is a free 30-minute workflow audit. Bring one document-heavy workflow your team does every week, and I will tell you what is automatable, what probably is not, and whether a sprint makes sense.

## Pre-Recording Checklist

- Start the OmniBid dev server and open `http://localhost:3000/demo/rfp-agent`.
- Sign in before recording if the route redirects through auth.
- Confirm the demo page is full-screen and app chrome is hidden.
- Confirm the queued solicitation is `030ADV26R0008`.
- Confirm the requirement count reads `19`.
- Pick one row with a clean source reference to pause on during the recording.
- Confirm no CUI, private company data, or logged-in personal data appears.
- Clear browser chrome as much as possible.
- Turn on Do Not Disturb.
- Use a 1440px-wide browser window unless Loom capture looks better full screen.
- Run one silent click-through: idle state → `Run Agent` → workflow animation → matrix ready → approve one row.
- Record at least three takes.
- Use the tightest take, not the first complete take.

## Success Criteria

The Loom is successful if a buyer can answer all three questions within two minutes:

1. What does Varholdt AI build?
2. Can Layken actually ship a working document workflow?
3. What should I do next?
