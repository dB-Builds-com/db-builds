# dB Builds — website build brief

Personal site for dB: portfolio, services, and hub for dB's tools. Audience is a general one, so copy stays plain and jargon-light.

The approved design lives in `design-reference/*.dc.html`. Those files are design-canvas exports, NOT production code: they use a custom template syntax (`<x-dc>`, `<sc-if>`, `<sc-for>`, `<dc-import>`, `{{holes}}`) and `/_blob/...` image paths. Use them only as the visual and copy reference. Match the layout, spacing, type sizes, and copy exactly; rebuild everything as clean Astro components.

## Stack

- Astro (latest stable), TypeScript, plain CSS with custom properties (no Tailwind needed)
- Hosted on Vercel at https://db-builds.com (domain bought through Vercel; DNS already handled there)
- Use `@astrojs/vercel` only if a server route is needed (see Contact form)
- Static output for every page
- Fonts: Geist and Geist Mono (self-host via `@fontsource` or the `geist` package rather than the Google Fonts link)

## Design tokens

Black and white only. No accent color, gradients, or shadows.

```
--ink:      #0A0A0A   /* text, buttons, borders that matter */
--muted:    #525252   /* secondary text */
--muted-2:  #A3A3A3   /* input borders */
--line:     #E5E5E5   /* dividers, card borders */
--chip:     #D4D4D4   /* outline tag borders */
--surface:  #F5F5F5   /* image placeholders */
--bg:       #FFFFFF
--radius:   6px (buttons, inputs) / 10px (cards, images)
--container: 1200px max, 32px side padding
```

Type: Geist for everything; Geist Mono for small uppercase labels (13px, letter-spacing 0.08em). Headings are bold with tight negative letter-spacing (about -0.03em to -0.05em). Hero headline uses `clamp(56px, 9vw, 128px)`.

Images (in `public/images/`):
- `about-photo.jpg` — About page, 4:5 frame, `object-fit: cover`
- `copybot-logo.jpg` — CopyBot logo on black. Home featured card: `object-fit: contain` on a #000 panel so the wordmark never crops. Projects feature card: same treatment.

## Shared layout

`src/layouts/Base.astro` with `<Header />` and `<Footer />`.

- Header: "dB Builds" wordmark (links home) on the left; Projects, Services, About links and a solid black "Start a project" button (to /contact) on the right. Bottom border. Current page link is underlined with `aria-current="page"`. Wraps on small screens.
- Footer: "© {year} dB Builds" left; X, Telegram, Discord, GitHub links right. Social URLs come from one config file (`src/config/site.ts`) so they're set once.

## Pages

1. `/` Home — mono label "Independent developer · dB"; H1 "Ideas in. / Products out."; subhead; buttons "See the work" (solid) and "Start a project" (outline). "What I build": three bordered cells (01 Trading bots & automation, 02 Websites & web apps, 03 Solana & token tooling). "Featured project": CopyBot card, text left with "In development" pill, black logo panel right. "How it goes": four steps (Idea, Plan, Build, Ship) with a 2px top rule. Black CTA band "Got an idea? Let's ship it." with a white button.
2. `/projects` — intro, large CopyBot feature card (pills: In development, Trading bot, Solana; button "Get early access"), then a grid of project cards. Drive projects from an Astro content collection (`src/content/projects/*.md`) with fields: title, category, description, image, status, featured, order. CopyBot is the featured entry; leave three placeholder entries.
3. `/services` — intro; three service rows, each with a number, title, pitch, and a 4-item list; black CTA band "Every project is quoted to fit." with "Get a quote". No pricing anywhere.
4. `/about` — 4:5 photo left; bio right; three fact cells (Focus / Also / Based in: United States); buttons "Work with me" and "See projects".
5. `/contact` — heading "Let's build something."; social link list (handle + ↗) on the left; form on the right in a black-bordered card: Name, Email, "What do you need?" select (Trading bot or automation / Website or web app / Solana or token tooling / Something else), message, "Send message". Success state: "Message sent." with "Send another".

Copy for every page is in the design-reference files; use it verbatim.

## Requirements

- Responsive down to 360px wide; grids use `repeat(auto-fit, minmax(...))`; two-column cards stack.
- Accessible: real `<a>`/`<button>`/`<label>`, visible focus styles, text contrast 4.5:1+, alt text on images.
- Use typographic apostrophes and quotes in copy.
- SEO basics: per-page `<title>` and description, Open Graph tags, favicon, sitemap (`@astrojs/sitemap`).
- Lighthouse 95+ across the board.

## Still to fill in

- Social handles and URLs (X, Telegram, Discord invite, GitHub)
- Three placeholder projects: name, category, description, image
- CopyBot screenshot (currently the logo stands in)
- Contact form backend: choose one, e.g. a form service (no server code) or an Astro server endpoint on Vercel that sends email via a provider, with the API key in Vercel env vars. Add basic spam protection either way.

## Deploy

- Site URL: https://db-builds.com (set `site` in astro.config.mjs)
- Hosting: Vercel, auto-deploys from GitHub repo `db-builds`
- `main` = live site. Never push to `main` unless I say "ship it".
- For changes: create a branch, commit, push, and give me the Vercel preview link.
- Run `npm run build` locally before every push; don't push a failing build.

### First-time setup (do once)

1. Confirm `gh auth status` shows I'm signed in to GitHub. If not, stop and ask me to run `gh auth login`.
2. Scaffold the Astro project, add a .gitignore covering `node_modules`, `dist`, `.vercel`, `.astro`, and `.env*`, and make the first commit. Keep `design-reference/` out of the build (it's reference only).
3. Create the repo and push: `gh repo create db-builds --private --source=. --remote=origin --push`
4. Tell me when it's pushed. I'll import it at vercel.com/new and attach db-builds.com under Settings → Domains.

