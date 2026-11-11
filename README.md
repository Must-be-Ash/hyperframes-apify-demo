# Agent‑session video — Apify endpoints, paid per call on Base

> **Not maintained.** x402 and the services this uses may have changed since it was built; check the latest x402 docs before relying on it.

A short [HyperFrames](https://hyperframes.heygen.com) video that simulates an AI agent (Claude‑desktop style) doing real work by calling **Apify** actors and paying **per tool call** in **USDC on Base**, then running an interactive, approval‑gated outreach. The whole thing renders from a single `index.html`.

**1080×1080 · ~26s · silent motion graphic.** Demo render: [`demo/apify-deal-finder-1080.mp4`](demo/apify-deal-finder-1080.mp4)

## What it shows

- A chat composer where the user types a real request: *"Find me a duplex under $400k in Columbus that could cash‑flow as a rental."*
- A **"Working"** timeline that streams the agent's tool calls one at a time. Each call is an **Apify actor** — the Apify logo is the row icon, with a source chip (Zillow + Realtor, Zillow, Airbnb, Google Maps) and a **Base payment chip**: a blue square + `"$0.0x USDC"`. It pays **per tool**, and a running total ticks up in the header.
- The agent reports the find in plain language, then **asks** whether to reach out, shows a **draft** message, the user **picks a channel** ("email it"), and the agent **sends** it (also a paid call on Base) and confirms.
- All names and contacts are **mock**; the market numbers (price, rent, cash‑flow) are realistic.

> Not affiliated with Apify, Base, Zillow, Airbnb, or Google. Demo only — verify live x402 pricing/behaviour before relying on any endpoint.

## Run it

Requirements: **Node.js ≥ 22** and **FFmpeg**. HyperFrames itself runs through `npx`, nothing to install.

```bash
npm run dev      # live preview in the browser
npm run check    # lint + validate + inspect
npm run render   # render to renders/  (or: npx hyperframes render --quality high --fps 30)
```

## Get the HyperFrames skill (build your own with an agent)

HyperFrames ships **agent skills** so an AI coding agent (e.g. Claude Code) can author videos like this for you. Install them, then restart your agent session:

```bash
npx skills add heygen-com/hyperframes
```

Docs: <https://hyperframes.heygen.com> · skill entry point in your agent: `/hyperframes`.

## Prompt guide — make a video like this for *any* Apify workflow

Drop your brand asset(s) in `assets/` (an `apify-logo.svg` is already here), then give your agent this prompt and swap in your workflow:

```text
Build a HyperFrames video (1080x1080, MP4) that simulates an AI agent working
inside a chat UI and promotes Apify endpoints paid PER CALL in USDC on Base.

STYLE: a realistic, minimal Claude-desktop-style session on a warm near-black
background (#1B1A17). No marketing headlines, no voiceover. Self-host Inter (UI)
and JetBrains Mono (chips) as woff2. Use a standard loading spinner for the
"Working" state — not a brand logo.

FLOW:
1. Home: a chat composer. The user types one real request for THIS workflow:
   <describe the task, e.g. "find investment properties", "monitor my
   competitors", "research what's winning on TikTok in <niche>">.
2. A "Working" timeline streams the agent's tool calls one by one. Each call is
   an Apify actor: use assets/apify-logo.svg (inline it as an SVG <symbol>) as
   the row icon, a short source chip (e.g. Zillow, Google Maps, TikTok, Amazon),
   and a Base payment chip = a small blue rounded square (#1A14F5) followed by
   "$0.0x USDC". PAY PER TOOL: every call shows its own cost and a running total
   ticks up in the header.
3. The agent reports what it found in plain language, then asks whether to take
   the final action; shows a drafted message; the user replies choosing a
   channel; the agent performs the send (also a paid call on Base) and confirms.
   Auto-scroll the conversation as it grows.

DATA: use mock names/identities (no real people or contacts). If you show
figures (prices, counts, rents), do a quick web search first so they're
realistic. Keep it ~20-30s.

BUILD: single index.html, one paused GSAP timeline registered on
window.__timelines, fully deterministic (no Date.now/Math.random). Run
`npx hyperframes lint && npx hyperframes validate && npx hyperframes inspect`,
fix every finding, then render high quality.
```

## How it's built (the recipe)

- **Single `index.html`** — one paused GSAP timeline on `window.__timelines["main"]`, rendered deterministically (no `Date.now`/`Math.random`).
- **Look** — warm near‑black chat UI; self‑hosted **Inter** (UI) + **JetBrains Mono** (chips). Greeting/voiceover intentionally omitted.
- **Apify branding** — the logo is inlined once as an SVG `<symbol id="apify">` and reused (`<use>`) as the icon on every tool‑call row.
- **Pay‑per‑tool on Base** — each call carries a blue rounded square (`#1A14F5`) + `"$0.0x USDC"`; a header total counts up via a seek‑safe `onUpdate` proxy.
- **Interactive turns** — the approval‑gated outreach (ask → draft → channel → send → confirm) lives in an auto‑scrolling `#thread` that translates upward as content grows.
- **Typewriter / state text** — driven by full‑duration `onUpdate` tweens so they're correct under the renderer's frame‑seeking.

## Layout / files

```
index.html        # the composition (source of truth)
fonts/            # self-hosted Inter + JetBrains Mono (woff2, latin subset)
assets/           # apify-logo.svg
demo/             # the rendered MP4
meta.json package.json hyperframes.json
```
