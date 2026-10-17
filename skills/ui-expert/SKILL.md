---
name: ui-expert
description: Use when the user runs /ui-expert <screen> and wants UI design options for one app screen, researched from Mobbin (or any connected UI-reference MCP) and presented as phone-frame mockups they pick from. Not for building the chosen design.
disable-model-invocation: true
---

# UI Expert

Turn one screen name into a page of pick-able design options. The user decides
by looking, not by reading. Every round is its own page. Mockups show only data
the app can really display. This skill writes **no app code**; other project
skills (Expo, framework setup) are not needed here.

Command: `/ui-expert [persona] <screen> [app name] [wide]`
e.g. `/ui-expert captain My Deliveries`, `/ui-expert settings`, `/ui-expert
onboarding step 2 Duolingo wide`. A leading word that matches a persona / role
group in the project (`(captain)`, `(sender)`, `admin`…) is the persona; the
rest up to a known app name or `wide` is the screen. An attached image is a
reference screenshot.

Slug = `<persona>-<screen>` in kebab-case (`captain-my-deliveries`).
Page title = `<Persona> · <Screen> — round N` (`Captain · My Deliveries — round 1`).

## The flow

```
0 ask once  →  1 audit today  →  2 research  →  3 options page  →  4 ask for pick
                                       ↑                                 │
                                       └──── 5 next round (new page) ────┘
                                                                         │
                                                             6 log + stop at the pick
```

### 0. Ask once, then go

Send exactly this, in one message, and wait:

> Two quick things before I research **<screen>**:
> 1. Any app or screenshot you like for this? (name it, paste it, or say *none*)
> 2. Quick (3–5 apps) or wide (8+)? Default quick.

Skip a part the command already answered (an app name or `wide`, an attached
image). Never ask anything else before the first page. A pasted screenshot is
the strongest input: list its parts to yourself (header, card, body, actions,
colors) and treat them as the brief.

### 1. Audit today's screen (before any drawing)

Read, in this order, and keep a private **facts list**:

1. **Project rules**: `CLAUDE.md` / `AGENTS.md` (design conventions, token file,
   i18n, component dirs). Where the rules and the repo disagree, the repo wins.
2. **The route file** for the screen (expo-router `app/**`, Next `app/**/page.tsx`,
   or the project's equivalent). Screen does not exist yet → read the **closest
   existing screen** (same persona, same list / detail / form shape).
3. **The components that draw pixels**: the route's imports and their children
   down to the leaf components (cards, rows, pills, icons). Skip hooks and
   utils. Note which components were built to match the design system; the
   mockups must reuse their language (same card, pill, divider, icon shapes).
4. **The data shape**, three tiers:
   - **usable**: fields on the app-side type the screen reads (domain type /
     adapter output), plus anything the screen already derives from them
     (payout, distance, countdown, next-step text).
   - **`api`**: fields the backend returns that the app has not mapped yet.
     Usable, drawn with a small `api` tag.
   - **`new`**: everything else. Allowed only for a new screen or when the
     user asks for a fact that does not exist. Drawn with a small `new` tag.
5. **The strings** the screen uses (`strings/*.json`, `messages/*.json`, …) and
   whether the app is bilingual / RTL.
6. **The design tokens** (colors, spacing, radii, font families) from the file
   the project calls the single source of truth, and the font registration
   (which family serves which language).
7. **Sample rows**: the project's mock / seed / fixture data when it exists;
   else write 3–5 realistic rows yourself from the usable fields (real cities,
   plausible prices). Never lorem, never placeholder names.
8. `docs/design-rounds/<slug>.md` if it exists: continue its round numbering
   and start from the last pick.
9. If the project names Figma as the visual source of truth and the Figma MCP is
   connected, pull the screen's current node screenshot for the Today frame.

### 2. Research by the screen's job

Search by what the screen **does**, not by the app's industry. A date-and-time
picker looks at booking apps; a wallet at fintech; an order list at any app with
orders; a courier job card at gig-work apps.

- Tool: `mcp__mobbin__search_screens` (`search_flows` when the screen is one
  step of a journey; `search_sections` for web pages). Load with ToolSearch if
  deferred. `platform`: `ios` for mobile, `web` for web. `mode: "deep"`.
  `output_destination: "design_tool"`. Keep `task_intent` and
  `output_destination` identical across all calls.
- User named a reference app → the first query names that app.
- Quick: 2–3 queries, `limit` 8. Wide: 5–8 queries, `limit` 15. Each query
  describes **one** screen in plain words ("courier job list with earnings and
  next-stop card"). No negations, no style adjectives.
- Look at the images, not the metadata. Write 4–8 **patterns**: one line each,
  `<pattern> — <app> (<mobbin_url>)`. A pattern is a reusable idea ("status strip
  above the card", "stacked stops with a dotted line"), not a description.
- Mobbin not connected or needs auth → say so in one line, use any other
  connected UI-reference MCP, else continue from your own knowledge and say so.
  Do not stop the flow for it.
- Never embed Mobbin `image_url`s in the page (they expire). Link `mobbin_url`.

### 3. The options page (one round = one page)

Load the host's artifact design guidance (the `artifact-design` skill when
present) and publish **one HTML page with the Artifact tool** (hosted,
shareable URL). No Artifact tool → write `docs/design-rounds/<slug>-round-N.html`
and open it in the browser. Either way: **a new file and a new URL per round**;
never republish over an earlier round.

Contents, top to bottom:

1. **Phone frames in a row** (desktop) / stacked (phone width). First frame is
   **Today** (round 1) or **Base** = the user's last pick with their requested
   changes applied (round ≥ 2). Then **3–4 options labelled A, B, C, D**.
2. Each option is a **full screen**, not a crop: the project's tokens as CSS
   variables; fonts from Google Fonts when the family is there, else the
   project's fallback stack; the project's own SVG icons inlined, third-party
   icon sets (lucide, Iconsax…) from their CDN or inline path, never hand-drawn
   lookalikes; copy from the strings file; rows from step 1.7.
3. **One line under each frame**: what changed and why (≤ 15 words). No essays.
4. **Switches**: language (EN/AR, flipping to RTL) on every frame if the app is
   bilingual. State switch (empty / loading / each status) **only for states
   that change the layout**, and only from round 2 on the Base and options;
   round 1 draws every frame in its main state.
5. **Patterns found** at the bottom: the 4–8 lines from step 2 with links.

Spread of options in round 1: **evolution of today's screen** by default —
options differ in layout and emphasis, not in paradigm. Include **at most one**
bold option. Go wide on paradigms only when the user asks ("explore", "something
different", `wide`). Rounds ≥ 2 vary only the thing the user commented on.

### 4. Ask for the pick

One message, nothing else:

> Round N is up: <link>. Which one — A, B, C or D? My pick: B (<5 words why>).
> Or tell me what to change.

"I don't like where X is" → next round shows **2–3 placements** of X, not one
guess. A screenshot pasted mid-flow → it is the new reference; rebuild the
options around it.

### 5. Next round

New page, round N+1. Base = the pick + their changes. 3–4 variations of exactly
what they commented on. Earlier rounds stay reachable through the log.

### 6. Log, then stop at the pick

Keep `docs/design-rounds/<slug>.md` in the project up to date **after every
round** (create the folder if missing):

```markdown
# Captain · My Deliveries — design rounds

Facts: <usable fields, one line> · api: <list or none> · new: <list or none>
Reference: <app / screenshot description / none>

## Round 1 — <YYYY-MM-DD> — <page url or file>
Today · A <one line> · B <one line> · C <one line> · D <one line>
Pick: B — "move the photo to the end"

## Round 2 — …
```

When the user confirms a final pick ("C is good", "go with B"), write
`Final: <option> (round N)` plus the `api` and `new` field lists the backend
and adapter must add, and end with one line: *"Next: build <pick> from round N
in a build session."* **Do not write app code.** The build has its own rules
and its own session.

## Talking to the user

Short messages. One question at a time, A/B/C choices of one line each, your
pick first. No design vocabulary ("affordance", "hierarchy") — say "the button
is easier to find". They answer with a letter; accept it as is.

## Red flags — stop and fix before publishing

| You are about to… | Instead |
|---|---|
| Draw a field that is not usable / `api` / `new` | Remove it or tag it |
| Republish to an old round's URL or file | New file, new page, round N+1 |
| Write a paragraph of rationale per option | One line ≤ 15 words |
| Offer 8 new paradigms when they asked to tweak | Vary only what they commented on |
| Research delivery apps for a calendar strip | Search by the screen's job |
| Paste Mobbin images into the page | Link `mobbin_url` only |
| Read titles instead of the screen images | Open the images; patterns come from pixels |
| Guess the leaf components from two files | Read down to the components that draw |
| Hand-draw a lookalike of a real icon | Inline the real SVG or load the icon set |
| Start the research before asking step 0 | Ask once, then go |
| Start coding the pick | Log it, say "next: build", stop |

## Quick reference

| Step | Output |
|---|---|
| 0 | One message: reference? quick/wide? |
| 1 | Private facts list: usable / `api` / `new` fields, strings, tokens, components, rows |
| 2 | 4–8 pattern lines with Mobbin links |
| 3 | Page `<Persona> · <Screen> — round N`: Today/Base + A–D + switches + patterns |
| 4 | One message: link + "A, B, C or D?" + my pick |
| 5 | New page per round, Base = last pick |
| 6 | `docs/design-rounds/<slug>.md` after every round; stop at the pick |
