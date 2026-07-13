# Handoff: RS Tech Solutions marketing site

## Overview
A four-page marketing website for **RS Tech Solutions** — a UK IT-support business offering
friendly, plain-English tech help for homes and small businesses. The site markets 12
services, tells the founder's story, previews the blog, and captures enquiries through a
contact form. Tagline: **"Learn it. Master it. Or let us handle it."**

Pages: **Home, Services, About, Blog.**

## About the Design Files
The files in this bundle are **design references created in HTML** — working prototypes that
show the intended look, layout, copy, and behavior. They are **not production code to ship
directly.**

There are two representations of the same design in the bundle:

- **`dc_source/` — the authoritative design reference.** Four `.dc.html` files authored in a
  component runtime (the `<x-dc>` / `support.js` system). This is where the real content,
  structure, and logic live. Read these to understand intent.
- **`site/` — a self-contained static export** of the same design (plain `index.html`,
  `services.html`, etc. loading React/Babel from a CDN at runtime). Useful to open in a
  browser and see the design live without any tooling, but it is a runtime-compiled export,
  not clean source.

**Your task:** recreate these designs in the target codebase's environment using its
established patterns and libraries. If there is no existing environment yet, the most natural
fit here is a small static-site generator or a plain React/Next.js/Astro project — this is a
content marketing site with one interactive piece (the contact form), so it does **not** need
a heavy SPA. Whatever you choose, reproduce the layout and styling faithfully (see Fidelity).

## Fidelity
**High-fidelity (hifi).** Final colors, typography, spacing, copy, and interactions are all
decided. Recreate the UI pixel-accurately. All visual values come from the **Nocturne design
system** (see Design Tokens) — wire those tokens into the target codebase rather than
hard-coding hex values, and reuse the design system's component classes/components where the
codebase allows.

## Design System — Nocturne
Every visual comes from the Nocturne design system, a **quiet, compact dark UI**: a
near-neutral blue-grey ground, Inter type at medium weight, soft 8px radii, and a single
blurple accent used as a line and a glow — never as a flooded fill. The one exception is the
Home stat band, which lifts to a saturated deep-indigo "section" ground.

Key principles to preserve:
- Left-aligned, asymmetric layouts. Flush-left headings, content hugging the left with
  whitespace on the right.
- **Outlined** primary buttons (1px accent border on transparent), never solid-filled.
- Low chroma everywhere except the accent; contrast comes from tonal ramps, not saturation.
- No pure black / pure white — every value is a ramp step.
- Headings never bolder than weight 500; hierarchy is size + space.
- Icons are **Phosphor** (regular + duotone). https://phosphoricons.com

The full stylesheet + token sheet is bundled at
`site/_ds/nocturne-…/styles.css`. Link it (or port its `:root` variables) so
`var(--color-*)`, `var(--font-*)`, `var(--space-*)`, `var(--radius-*)`, `var(--shadow-*)`
all resolve. The component classes referenced below (`.btn`, `.card`, `.tag`, `.field`,
`.input`, `.nav`, `.lighten`, `.elev-sm/md/lg`) are defined there.

## Global layout & chrome

### Page shell
- Max content width **1240px**, centered. Horizontal padding `clamp(20px, 5vw, 64px)`.
- Body background is layered radial gradients over `--color-bg`:
  `radial-gradient(1100px 640px at 86% -180px, <accent-900 tint>, transparent 60%)`,
  `radial-gradient(1000px 760px at -8% 104%, <black 34%>, transparent 55%)`, then `--color-bg`.
- Body text color `--color-text`, font `--font-body`. `scroll-behavior: smooth` with
  `scroll-padding-top: 78px`; disabled under `prefers-reduced-motion`.

### Nav (sticky, on every page)
- `position: sticky; top: 0; z-index: 30`, 1240px max width, padding `12px clamp(20px,5vw,64px)`.
- `backdrop-filter: blur(12px)`, background `color-mix(--color-bg 78%, transparent)`, 1px bottom
  divider (`--color-divider`).
- Left: brand — a 30×30 rounded-8px box with 1px accent border holding a `ph-shield-check`
  icon (accent color), then text "RS Tech Solutions".
- Center/right: text links **Home · Services · About · Blog** (14px, muted; active link is
  accent-colored; hover → full `--color-text`).
- Right: primary outlined button **"Get in touch"** (`ph-phone-call` icon) linking to
  `#contact` on Home.

### Footer (on every page)
- 1240px, top area is a flex row: left = brand mark + one-line description
  ("Friendly, reliable IT support for homes and small businesses. Learn it, master it — or let
  us handle it."); right = two link columns: **Pages** (Services, About, Blog, Contact) and
  **Get in touch** (01234 567 890, hello@rstechsolutions.co.uk).
- 1px divider under that row, then copyright: `© <current year> RS Tech Solutions. All rights
  reserved.` (year is computed at runtime).

## Screens / Views

### 1. Home (`index.html` / `RS Tech Solutions - Home.dc.html`)
Sections top to bottom:

**Hero** — two-column grid `minmax(0,1.35fr) minmax(0,1fr)`, gap `clamp(32px,5vw,64px)`,
vertically centered; padding `clamp(48px,8vw,104px) 0 clamp(40px,6vw,64px)`.
- Left: an uppercase accent eyebrow with a 32px accent rule ("Friendly IT support, done
  properly"); H1 `clamp(38px,5.4vw,68px)`, line-height 1.04, letter-spacing -0.022em,
  **"Behind every fix is a foundation of knowledge."**; a lead paragraph
  `clamp(17px,1.7vw,21px)`; two buttons (primary "Get a free callback" + secondary "Explore
  services"); a small reassurance line with a `ph-clock-countdown` icon ("We reply to every
  enquiry within one working hour.").
- Right: a `figure.lighten` wrapping a 4:5 image slot (rounded `--radius-lg`, `--shadow-lg`,
  1px divider border). In production this is a hero photograph — a workspace / kit / team shot,
  ideally on a dark background (Nocturne `.lighten` blends dark imagery into the page).

**Stat band** (full-bleed, toggleable) — background is the saturated `--color-section` ground
with an accent-glow radial. Four stats in an auto-fit grid: **24/7** "Remote support cover",
**< 1 hr** "Average response time", **12+** "Services under one roof", **No jargon**
"Plain-English advice". Numbers `clamp(30px,3.4vw,46px)`; labels 12px uppercase, tracked,
muted. This section is behind a boolean prop `showStats` (default **true**) — see State.

**Services preview** — section header row (eyebrow "What we do", H2
`clamp(28px,3.6vw,40px)` "Everything your tech needs, from one team", intro paragraph) with a
right-aligned secondary button "All 12 services". Below: an auto-fill card grid
`minmax(280px,1fr)`, gap 16px, showing **6** of the 12 services as cards. Each card is an
`<a>` (`.card.elev-sm`) linking to `services.html#<id>`, containing: a 44×44 rounded-10px icon
tile (background `--color-accent-900`, icon `--color-accent-300`, duotone Phosphor icon),
`.card-title`, `.card-body` description, and a ghost "Learn more →" button. The 6 shown:
24/7 Remote Support, Website Development, Website Hosting, Cyber Essentials, Home Network
Installations, Office 365 Support.
- Hover state (all service cards): `transform: translateY(-3px)` and a ring shadow
  `0 0 0 1px var(--color-accent), 0 10px 30px rgba(0,0,0,.5)`; transition `.18s ease`.

**Blog preview** — same header pattern (eyebrow "From the blog", H2 "Practical IT advice, in
plain English", secondary button "Read the blog"). Auto-fill grid `minmax(300px,1fr)`, gap
20px, 3 post cards. Each post card is an `<a target="_blank">` (`.card.elev-sm`, padding 0,
overflow hidden): a 16:10 thumbnail image, then a body block with a meta row (a `.tag.tag-accent`
category chip + date, 11px uppercase tracked muted), an 18px title, and a 14px excerpt.
- Hover: same lift + ring shadow as service cards, plus the thumbnail image scales to 1.05
  (`transition: transform .4s ease` on the img).
- The three posts link out to live articles on rstechsolutions.co.uk (Best Android Launchers
  2025 / Mobile / 23 Jun 2025; Quick Assist — Built-in Remote Software / IT Support / 12 May
  2025; Why Network Speed Isn't Just About Bandwidth / Networking / 11 May 2025). Thumbnails are
  hot-linked from that domain.

**Contact** (`#contact`) — full-bleed band, top divider, background
`color-mix(--color-surface 60%, --color-bg)` with an accent-glow radial. Two-column grid
`minmax(0,6fr) minmax(0,5fr)`.
- Left: eyebrow "Contact", H2 "Avoid IT headaches — talk to us today", paragraph, then a
  vertical list of contact rows each with a 44×44 accent icon tile: **Call us** 01234 567 890
  (`tel:` link), **Email** hello@rstechsolutions.co.uk (`mailto:`), **Hours** "Mon–Fri 8am–6pm
  · 24/7 for contract clients", **Coverage** "[Your area] & remote across the UK".
- Right: a `.card.elev-md` holding the enquiry form (see Interactions): fields Name, Email,
  Phone (optional), "How can we help?" textarea, a full-width primary submit "Send enquiry", and
  a fine-print line "We reply within one working hour. No spam, ever." On submit it swaps to a
  success state (check-circle icon, "Thank you! ✨", confirmation copy, "Send another" button).

### 2. Services (`services.html` / `…- Services.dc.html`)
- **Hero**: eyebrow "Services", H1 `clamp(34px,4.8vw,60px)` **"Twelve services, one team you can
  trust."**, lead paragraph, primary button "Get a free callback" → `index.html#contact`.
- **Service list**: all **12** services rendered from a data array (see Content below). Each is a
  full row/block with an anchor `id` (so `#remote-support` etc. deep-link from Home): a number
  (`01`–`12`), an uppercase accent kicker, an H2 title `clamp(24px,3vw,34px)`, a tagline
  paragraph, and a bulleted feature grid (`list-style:none`, auto-fill `minmax(230px,1fr)`).
- **CTA band**: H2 "Not sure which service you need?" + supporting copy + button.

### 3. About (`about.html` / `…- About.dc.html`)
- **Hero**: eyebrow "About", H1 **"Hi, I'm Rui."**, two intro paragraphs (a passionate tech
  enthusiast; dedicated to helping individuals and small businesses). A headshot is referenced
  from rstechsolutions.co.uk.
- **Story**: two-column `minmax(0,5fr) minmax(0,7fr)` — left H2 "Learn it. Master it. Or let us
  handle it.", right the narrative paragraphs (RS Tech Solutions started from a simple idea…).
- **Values**: eyebrow "What you can expect", H2 "Three things I never compromise on", a
  three-card auto-fit grid of value cards.
- **Process**: eyebrow "How working together goes", H2 "From first message to fully sorted", a
  numbered 4-step grid — **Get in touch → Diagnose → Fix or plan → Ongoing support**, each with a
  numbered badge, H3, and description.
- **CTA band**: H2 "Let's sort your tech, together." + copy + button.

### 4. Blog (`blog.html` / `…- Blog.dc.html`)
- **Hero**: eyebrow "Blog", H1 **"Practical IT advice & technology insights."**, lead paragraph.
- **Post grid**: cards identical in styling to the Home blog preview, linking out to the live
  articles on rstechsolutions.co.uk.
- **CTA band**: H2 "Got a tech question of your own?" + copy + button.

## Interactions & Behavior
- **Nav**: sticky; active page link is accent-colored; hover lightens links to full text color.
- **Service & post cards**: hover lift (`translateY(-3px)`) + accent ring shadow; post thumbnails
  additionally zoom to 1.05. Transitions `.18s` (cards) / `.4s` (thumb).
- **Contact form** (Home): client-side only in the prototype. On submit, `preventDefault()` and
  swap the form for a success panel; "Send another" resets it. **In production, wire the submit
  to a real backend / form service** (email, Formspree, serverless function, etc.) — the
  prototype does not send anything.
- **Deep links**: Home service cards link to `services.html#<service-id>`; nav "Get in touch"
  and CTA buttons link to `index.html#contact`. Preserve these anchors.
- **Focus**: keyboard focus uses Nocturne's `:focus-visible` accent ring (2px solid accent,
  2px offset) — do not fall back to the browser default.
- **Reduced motion**: smooth scroll is disabled under `prefers-reduced-motion: reduce`.
- Responsive: all sizes use `clamp()` and auto-fit/auto-fill grids, so layouts reflow to a single
  column on narrow screens without explicit breakpoints.

## State Management
Minimal — only the Home contact form holds state:
- `submitted: boolean` (default `false`). Submit sets it `true` (shows success panel); "Send
  another" sets it `false`.
- Home also exposes a design prop **`showStats: boolean`** (default `true`) that toggles the stat
  band. Treat as a build-time/config flag if useful, or drop it and always show the band.
- `year` is `new Date().getFullYear()` for the footer.
No data fetching beyond loading external images (blog thumbnails, About headshot) and, in the
static export only, the React/Babel CDN.

## Design Tokens
All from Nocturne (`site/_ds/nocturne-…/styles.css`). Reference by variable; representative
values:
- **Colors**: `--color-bg` #161826 (dark ground) · `--color-text` #e9e9ed · accent #9184d9
  (blurple). Each role carries a 100–900 OKLCH ramp (`--color-accent-100…900`, neutral ramp,
  etc.). On this dark ground: 700–900 for tinted fills/hovers/subtle borders (e.g. icon tiles use
  `--color-accent-900` bg + `--color-accent-300` icon), 500 as base, 100–300 for text on tints.
  `--color-surface`, `--color-divider`, and the saturated `--color-section` / `--color-section-glow`
  (stat band + CTA grounds). Use ramp steps, not ad-hoc `color-mix()`, for fills.
- **Type**: `--font-heading` and `--font-body` are both **Inter**. Headings max weight 500.
  Heading sizes are fluid `clamp()` (see each section). Body 16px; small/meta 11–14px, uppercase
  labels tracked ~.06–.09em.
- **Spacing**: compact scale `--space-*` (density 0.70×). Section padding via `clamp()`.
- **Radius**: `--radius-*` (base 8px); cards ~10–12px, hero image `--radius-lg`.
- **Shadow**: `--shadow-sm/md/lg`, tuned for the dark ground (edge + ambient darkness, not heavy
  drop shadows).

## Assets
- **Icons**: Phosphor Icons (regular + duotone), loaded from
  `unpkg.com/@phosphor-icons/web@2.1.1`. Swap for the codebase's icon system if it has one; icon
  names are in the markup (`ph-shield-check`, `ph-headset`, `ph-code`, `ph-cloud-check`,
  `ph-wifi-high`, `ph-microsoft-outlook-logo`, etc.).
- **Hero photo (Home)**: a drag-drop image slot in the prototype — supply a real hero photograph
  (workspace / kit / team, dark background preferred for the `.lighten` blend).
- **Blog thumbnails**: hot-linked from `rstechsolutions.co.uk/wp-content/uploads/…`. Re-host these
  locally for production reliability.
- **About headshot**: referenced from rstechsolutions.co.uk. Re-host locally.
- **Nocturne stylesheet + bundle**: `site/_ds/nocturne-14b57070-…/`.
- The site requires an internet connection in the static export (CDN scripts + external images).

## Content — the 12 services (Services page, exact copy)
Each: `id` (anchor) · number · kicker · title · tagline · bullets.
1. `remote-support` · 01 · Support · **24/7 Remote Support** — "Round-the-clock remote IT support
   tailored to your needs. With a support contract, expert assistance is just a click away — no
   waiting for an onsite visit." · 24/7 remote assistance (contract); One-off remote sessions;
   PC, Mac & mobile troubleshooting; Software install & configuration; Network diagnostics;
   Security & patch management.
2. `website-development` · 02 · Web · **Website Development** — "Responsive, modern websites that
   reflect your brand and goals — from a personal blog to a complex business portal." · Custom
   design & development; WordPress, HTML, CSS & JS; SEO optimisation; E-commerce stores; Site
   audits & redesigns.
3. `website-hosting` · 03 · Hosting · **Website Hosting** — "Secure, reliable hosting for any size
   site — from simple static pages to complex e-commerce systems." · Shared, VPS & dedicated;
   Daily backups & uptime monitoring; SSL certificate installation; Email hosting; Managed hosting
   with support.
4. `cyber-essentials` · 04 · Security · **Cyber Essentials** — "Guided certification that protects
   your business and helps you win more work — we handle the whole process with you." · Gap
   analysis & compliance checks; Remediation guidance; Documentation preparation; Self-assessment
   or Plus support; Post-certification support.
5. `home-network` · 05 · Networks · **Home Network Installations** — "Optimise your home
   connectivity with professional installation and support that just works, in every room." ·
   Wi-Fi design & installation; Ubiquiti UniFi setup; Router & access point install; Ongoing home
   support; Troubleshooting & speed optimisation.
6. `office-365` · 06 · Microsoft 365 · **Office 365 Support** — "Expert support for Microsoft 365
   environments to improve productivity and security across your team." · 365 setup &
   administration; SharePoint sites & permissions; Power Automate configuration; Email migration &
   support; User training & documentation.
7. `phone-lines` · 07 · Telephony · **Phone Line Installations** — "Commercial and professional
   phone line installation and hosting for small to medium-sized businesses." · Multi-line
   commercial systems; SIP & VoIP solutions; Hosted PBX services; Number porting & call routing;
   Call recording & analytics.
8. `cabling` · 08 · Infrastructure · **Structured Cabling** — "Structured cabling for residential
   and business environments, using the latest standards and fully certified." · RJ45 termination;
   CAT5e & CAT6 installations; Patch panel setup; Network diagnostics; Testing & certification.
9. `telephony` · 09 · Voice · **Telephony Solutions** — "Reliable and scalable voice solutions for
   businesses of all sizes, built around how your team actually works." · Cloud-hosted VoIP;
   Mobile integration; Microsoft Teams telephony; IVR setup; Telephony security & monitoring.
10. `game-servers` · 10 · Hosting · **Game Server Hosting** — "Host your gaming community with
    low-latency, high-performance servers for a range of popular platforms." · Minecraft, ARK,
    Rust & more; Customisation & mods; Anti-DDoS protection; Control panel access; Auto backups &
    monitoring.
11. `buy-back` · 11 · Sustainability · **IT Equipment Buy-Back** — "Sustainable disposal and
    recycling of your unwanted IT hardware, with secure certified data destruction." · Purchase of
    decommissioned kit; Secure data destruction & certificate; Asset auditing; Environmentally
    responsible recycling.
12. `home-assistant` · 12 · Smart Home · **Home Assistant Installer** — "Take control of your smart
    home with a tailored Home Assistant setup that ties all your devices together." · Tailored
    installation; Device & sensor integration; Automations & dashboards; Voice control; Ongoing
    support.

## Contact details used throughout
- Phone: **01234 567 890** (placeholder — replace with the real number).
- Email: **hello@rstechsolutions.co.uk**.
- Hours: Mon–Fri 8am–6pm · 24/7 for contract clients.
- Coverage: "[Your area] & remote across the UK" (placeholder — fill in the real area).

## Files in this bundle
- `dc_source/RS Tech Solutions - Home.dc.html` — authoritative Home source (+ contact-form logic).
- `dc_source/RS Tech Solutions - Services.dc.html` — Services (12-service data array in its script).
- `dc_source/RS Tech Solutions - About.dc.html` — About.
- `dc_source/RS Tech Solutions - Blog.dc.html` — Blog.
- `site/` — self-contained static export (open `site/index.html` in a browser to see it live).
- `site/_ds/nocturne-…/styles.css` — the Nocturne design-system stylesheet + token sheet.
