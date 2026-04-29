# wafoxworthy.com — Project Knowledge

## Owner

Alex Foxworthy, Eastern Shore Community College (ESCC), Onancock VA.

## Purpose

Personal academic and creative homepage. Houses essays, research, videos,
live environmental sensor data, and CV-style content. Also serves as a
unified information environment that AI tools can work with directly via
the repo — content is stored as Markdown so LLMs and agents have native
access to the full corpus.

## Tech stack

- **Site generator**: Astro (chosen for modern tooling, good Claude Code support, flexibility for interactive components later)
- **Hosting**: Cloudflare Pages (free tier, fast CDN, good caching for any future API proxy)
- **Domain**: wafoxworthy.com, registered through Cloudflare Registrar (~$10/year, at-cost pricing)
- **Repo**: public, on Alex's existing GitHub account, new repository
- **Theme starting point**: Astro Paper (or similar writer-focused theme); customization in later sessions
- **Content format**: Markdown-first

## Section structure (v1)

Six top-level sections:

1. **Home** — landing page; brief intro, pollen widget with explanatory framing, links to recent content from each section
2. **Writing** — essays, 3 Quarks Daily column links, reverse chronological
3. **Research** — academic publications, current/past funded projects, grant work
4. **Video** — YouTube embeds, organized by topic (microscopy, talks, lab walkthroughs)
5. **Environmental** — deep page for sensor data and analysis; pollen widget lives here too with more context, plus space for future correlations and visualizations
6. **About** — bio, contact info, CV download

Drop-down or horizontal nav. Six fits without scrolling. Don't add sections at start; let the structure grow into itself.

## Live data on home page

Pollen Sense widget only for v1.

- Embedded in a contextualizing card with an explainer paragraph
- Brief copy: "Live readings from the ESCC sensor in Onancock, VA. Scale: Low → Very High based on Pollen Sense's Misery Index."
- Link from the card to /environmental for the deeper view
- Subsections (pollen / mold / dust) supported by the widget's own UI; expand/collapse if the widget offers it
- Iframe is 310×310 fixed; design the card around that constraint

Future widgets (AirGradient indoor air, weather, sensor map, etc.) are
deferred. Redesign when there's a second real data source ready, rather
than designing speculatively for unknowns now.

## Key decisions made

- **Public-first architecture.** No separate "internal" layer — everything in the repo, with public/featured content polished and rest just present. AI tools get full corpus access by reading the repo.
- **Markdown over Notion or other CMS.** Plain text is the lingua franca for both humans and LLMs. No vendor lock-in.
- **One Pollen Sense widget on home, expanded view on /environmental.** Don't duplicate; let the home version be a teaser to the deeper page.
- **API keys never committed to repo.** Environment variables only, on the build/runtime side. The Pollen Sense widget uses an iframe with the key in the URL (vendor's design); that key is publicly visible by necessity and is a separate concern from data API keys.
- **wafoxworthy.com as the personal-identity domain.** Reserved easternshorescience.com is available as a future option for a separate institutional/collaborative project.

## Open questions for Pollen Sense vendor

- Is the widget key scope-limited, or is it the same as the data API key?
- Can the widget key be domain-locked (referrer/origin restricted)?

These need answers before the widget is embedded on a publicly-discoverable page.

## Companion local pipeline (already built, not part of website)

Three Python scripts live on Alex's Mac, in `~/Documents/Pollensense website/`:

- **pollen_test.py** — initial test harness; confirms API access, lists sensors/sites/categories
- **pollen_archive.py** — hourly pull from the v2 metrics-export endpoint; appends to `pollen_archive/misery_hourly.csv`; idempotent and gracefully handles missed runs via 3-hour fetch window
- **pollen_snapshot.py** — reads the archive, produces human-readable summary with current conditions, 24-hour context, and notable species callouts

These run locally and are independent of the website. Future possibility: surface archive-derived data on the /environmental page (e.g., "this week vs. last week" charts, peak species over the season). Not part of v1.

## Constraints

- Free hosting (no monthly fee)
- ~$10/year for domain only
- Minimize ongoing maintenance burden
- Markdown editing workflow preferred over WYSIWYG

## Workflow

- **Project chat (claude.ai)**: thinking, planning, architecture, content strategy, code review of components Claude Code wrote. Where decisions get made.
- **Claude Code (in VS Code)**: implementation, configuration, debugging, file editing, deploys. Where decisions get executed.
- **decisions.md** in the repo: running log of choices made and why, so future-self and future-Claude can recover context.

## Suggested first Claude Code session

Scope:

> Initialize an Astro project from the Astro Paper theme. Connect to a new public GitHub repo. Deploy to Cloudflare Pages connected to the wafoxworthy.com domain. End state: typing wafoxworthy.com in a browser shows a working theme-default homepage. No customization yet — that's separate sessions.

Time estimate: 2–4 focused hours. Resist scope creep. Customization, content migration, and the pollen widget integration come in subsequent sessions, each with its own clear scope.
