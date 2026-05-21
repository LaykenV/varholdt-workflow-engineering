# Varholdt AI Marketing Site

Astro static marketing app for the Varholdt AI launch page.

## Commands

```sh
npm install
npm run dev
npm run build
npm run preview
```

## Environment

Copy `.env.example` to `.env` when real production links are ready.

- `SITE_URL`: canonical production origin used by Astro and the sitemap
- `PUBLIC_CALENDLY_URL`: CTA booking link
- `PUBLIC_LOOM_URL`: OmniBid walkthrough embed/link once recorded
- `PUBLIC_CONTACT_EMAIL`: public contact email

## Design Handoff

The current homepage is intentionally plain. Replace the presentation in `src/pages/index.astro` and `src/styles/global.css`, keeping the content constants in `src/data/marketing.ts` unless the offer changes.
