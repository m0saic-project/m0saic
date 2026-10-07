<p align="center">
  <a href="https://m0saic.io">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="assets/logo.svg" width="120">
      <source media="(prefers-color-scheme: light)" srcset="assets/lockup-light.png" width="380">
      <img src="assets/lockup-light.png" alt="m0saic" width="380">
    </picture>
  </a>
</p>

<h3 align="center">Write TypeScript. Compile to video. Built for agents.</h3>

<p align="center">
  <a href="https://www.npmjs.com/package/m0saic"><img src="https://img.shields.io/npm/v/m0saic?label=npm%20m0saic&color=EF7525" alt="npm"></a>
  <a href="https://m0saic.io"><img src="https://img.shields.io/badge/site-m0saic.io-EF7525" alt="m0saic.io"></a>
  <a href="https://app.m0saic.io"><img src="https://img.shields.io/badge/try%20it-in%20the%20browser-EF7525" alt="Mosaic Web"></a>
  <a href="https://discord.gg/ns58hGm6Mm"><img src="https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white" alt="Discord"></a>
</p>

<p align="center">
  <a href="https://x.com/qsbuilds"><img src="https://img.shields.io/twitter/follow/qsbuilds?style=social" alt="Follow on X"></a>
  <a href="https://mosaicengine.substack.com"><img src="https://img.shields.io/badge/Substack-subscribe-FF6719?logo=substack&logoColor=white" alt="Substack"></a>
  <a href="https://reddit.com/r/m0saic"><img src="https://img.shields.io/badge/Reddit-r%2Fm0saic-FF4500?logo=reddit&logoColor=white" alt="Reddit"></a>
  <a href="mailto:hi@m0saic.io"><img src="https://img.shields.io/badge/Contact-hi%40m0saic.io-888" alt="Contact"></a>
</p>

<p align="center">
  <img src="assets/hero.gif" alt="An m0 turning into rectangles, one tile at a time" width="800">
</p>

m0saic is a video compiler. A m0saic template is a TypeScript program that returns an **[m0](https://github.com/m0saic-dsl/m0)**
(rectangles on a canvas) string and one source per rectangle; m0saic compiles that to ffmpeg and hands you back
media (video or image). There's no browser in the pipeline and no timeline to drag. The same template, props
and canvas always produce the same output, so a render behaves like a build artifact you can test, diff
and re-run from a cron job or a coding agent.

```sh
npx m0saic hello-world
```

That's the whole first run. If you have no ffmpeg it says so, and `npx m0saic setup` fetches a pinned build
with your approval. (Node 18.17+; macOS, Windows and Linux.)

## Three surfaces, one compiler

Mosaic Desktop is the product. The CLI is what makes it production. Mosaic Web is where you look first.

| Mosaic Web | Mosaic Desktop | CLI |
|:-:|:-:|:-:|
| <a href="https://app.m0saic.io"><img src="assets/surfaces/web-make.png" width="300" alt="Mosaic Web: a template open in Make, in the browser"></a> | <a href="https://m0saic.io/download"><img src="assets/surfaces/desktop-make.png" width="300" alt="Mosaic Desktop: the Make canvas, the structure dock and the template settings"></a> | <code>npm i -g m0saic</code><br><code>m0saic make @m0saic/alpine/donut/v4 \</code><br><code>&nbsp;&nbsp;--props budget.json -o budget.mp4</code> |
| **[app.m0saic.io](https://app.m0saic.io)** · nothing to install | **[Download](https://m0saic.io/download)** · macOS and Windows | **[npm](https://www.npmjs.com/package/m0saic)** · anywhere ffmpeg runs |
| Open a template link and play with it: the live preview, the props, the canvas. The web-safe templates run as they are. Rendering is not here, so it hands you the CLI command or the Desktop link. It is the place to see the value before you install anything. | The complete workspace: the Make canvas where every bound rect is a handle, the structure of the layout, the timeline, your coding agent working in the room, templates from any repo, and local rendering with your own files. This is what makes m0saic a product. | Headless rendering: cron jobs, CI, servers, a build step inside your own product. The same templates and the same output with no UI. Powerful on its own, and built to be driven by scripts and agents rather than typed by hand. |

## Render something from your data

```sh
npm i -g m0saic
m0saic setup --yes
m0saic make @m0saic/alpine/donut/v4 --props @examples/budget/budget.json --output-kind image -o budget.png
```

<p align="center"><img src="examples/budget/budget.png" alt="A donut chart rendered from budget.json" width="560"></p>

Drop `--output-kind image` (and the `anim` prop) for the animated MP4. `-w 1080 -h 1920` re-lays the same
template for a vertical post; nothing is letterboxed. `m0saic list-templates` shows every template, and
[m0saic.io/llms-full.txt](https://m0saic.io/llms-full.txt) lists each one with its props.

## Give it to your coding agent

m0saic is built to be driven by agents: the m0 is one string a validator can check, and a m0saic template is
code your agent can write. This repo is a Claude Code plugin and a set of Agent Skills, both wired to the m0saic MCP server.

**Claude Code**, as a plugin (skills + the MCP server):

```
/plugin marketplace add m0saic-project/m0saic
/plugin install m0saic@m0saic
```

**Any agent that reads [Agent Skills](https://agentskills.io)** (Codex, Cursor, Gemini CLI, OpenCode, Copilot and more):

```sh
npx skills add m0saic-project/m0saic
```

**Just the MCP server** (the knowledge base, every template's props, the layout validator, one-frame renders, the publish checks, and a live link into Mosaic Desktop):

```sh
claude mcp add --scope user m0saic -- npx -y m0saic mcp     # Claude Code
codex mcp add m0saic -- npx -y m0saic mcp                   # Codex
gemini extensions install https://github.com/m0saic-project/m0saic   # Gemini CLI
```

Cursor, Windsurf, Claude Desktop and other `mcpServers` clients:

```json
{ "mcpServers": { "m0saic": { "command": "npx", "args": ["-y", "m0saic", "mcp"] } } }
```

Then ask for what you want: *"make a 15-second vertical video of this week's commits"*, *"turn this CSV
into a bar chart I can post"*, *"build me a template for my podcast's episode cards"*. The skills tell the
agent when to render with an existing template and when to write a new one.

<p align="center">
  <img src="assets/surfaces/desktop-agent.jpg" width="720" alt="Mosaic Desktop's agent session: Claude Code, Codex, Gemini CLI and others, building a workspace in the room">
  <br><sub>In Mosaic Desktop the agent works in the room: pick a harness, watch it build, and the template opens on the canvas when it lands.</sub>
</p>

## m0

```
3(1,1,1)          three equal columns
2(F,3[F,F,F])     a column beside three stacked rows
2[F,F]{F}         two rows, with a layer on top
```

`(` splits across, `[` splits down, `{}` layers content over a tile. Every rectangle gets integer pixel
bounds: a three-way split of 1920 is 640/640/640, and when a split doesn't divide, the remainder is handed
out the same way every time. `npx m0saic "3(1,1,1)" --anim` renders any m0 you type as an animated
wireframe. m0 is closed under composition: any template can nest any other, because it all resolves to
geometry. The language is open (Apache-2.0); the official implementation is [m0saic-dsl/m0](https://github.com/m0saic-dsl/m0),
and [m0saic.io/why](https://m0saic.io/why) is the case for rectangles.

## What you can make

Anything you can say as rectangles with known contents. Nine from the library, picked for range:

| | | |
|:-:|:-:|:-:|
| <img src="assets/gallery/screencap-grid.png" width="260"><br>**Contact sheet** from a video<br>`@m0saic/media/screencap_grid/v3` | <img src="assets/gallery/drop-calendar.png" width="260"><br>**Drop calendar** for a creator's month<br>`@m0saic-dev/creator/drop-calendar/v1` | <img src="assets/gallery/dvd-wrap.png" width="260"><br>**Print dieline** for a DVD wrap<br>`@m0saic-dev/print/dvd-wrap/v1` |
| <img src="assets/gallery/business-card.png" width="260"><br>**Business card** with a QR<br>`@m0saic/brand/business-card/v2` | <img src="assets/gallery/quote-card.png" width="260"><br>**Quote card** for a post<br>`@m0saic/social/quote-card/v2` | <img src="assets/gallery/highlights.png" width="260"><br>**Highlight clip** cut from a recording<br>`@m0saic/media/highlights/v2` |
| <img src="assets/gallery/snippet-morph.png" width="260"><br>**Code walkthrough**, line by line<br>`@m0saic/code/snippet-morph/v2` | <img src="assets/gallery/dual-sub.png" width="260"><br>**Bilingual subtitles** burned in<br>`@m0saic-dev/language/dual-sub/v1` | <img src="assets/gallery/pipeline-review.png" width="260"><br>**Leadership walkthrough** of a sales quarter<br>`@m0saic-dev/sales/pipeline-review/v1` |

Also in the library: charts and KPI cards, commit feeds and repo pulses, QR codes and barcodes, photo
collages, lyric reels, watermarks, blur regions, title cards, partner maps. More at
[m0saic.io/gallery](https://m0saic.io/gallery) and the [case studies](https://m0saic.io/case-studies), plus a
[community library](https://github.com/m0saic-project/m0saic-community-templates),
[templates written by agents](https://m0saic.io/templates-by-agent), and an agent that ships
[one template a day](https://github.com/m0saic-project/one-a-day).

## Why m0saic?

- **Geometry-native:** a video is an m0 (rectangles on a canvas) and one source per rectangle. No DOM, no React requirement. Because everything resolves to geometry, any template can render and nest any other. [Why rectangles](https://m0saic.io/why).
- **No browser in the pipeline:** a template outputs a media-intent IR; the compiler lowers that to an ffmpeg graph. Nothing is screenshotted, so a render costs what ffmpeg costs and runs anywhere ffmpeg does.
- **Deterministic by construction:** integer pixel bounds, seeded randomness, no wall clock. The same template, props and canvas give the same bytes.
- **Agent-friendly:** the m0 is one string a validator checks, a m0saic template is TypeScript an agent can write, and the MCP server and the skills ship in this repo. The CLI is non-interactive.
- **Data in, file out:** props are whatever the template declares: JSON rows, pasted CSV, numbers, colours, media files. A quarter's export, a commit log, a budget. Re-render on a schedule, in CI, or when the numbers change.
- **Open language, commercial engine:** the m0 language, its stdlib and file formats are Apache-2.0 and the template libraries are public. The engine is free to use with an attribution mark; Pro removes it.

## Remotion vs HyperFrames vs m0saic

All three make video from code. [Remotion](https://github.com/remotion-dev/remotion)'s bet is React components. [HyperFrames](https://github.com/heygen-com/hyperframes)' bet is plain HTML. m0saic's bet is geometry: an m0 and one source per rectangle, with no browser anywhere in the render.

| | Remotion | HyperFrames | m0saic |
|---|---|---|---|
| Authoring | React components | HTML + CSS + seekable animation | an m0, a TypeScript template, your props |
| Renders through | headless Chrome + FFmpeg | headless Chrome + FFmpeg | FFmpeg only |
| Build step | bundler required | none; `index.html` plays as-is | none to render; `tsc` to author |
| Agent handoff | JSX / React project | plain HTML files | one validated string, typed props, MCP + skills |
| Animation | frame-driven via `useCurrentFrame`; wall-clock libraries need care | seekable, frame-accurate via adapters | per-frame expressions in ffmpeg; no wall clock |
| Distributed rendering | Remotion Lambda | local and AWS Lambda | anywhere ffmpeg runs: local, CI, your servers |
| License | source-available Remotion License | Apache 2.0 | Apache-2.0 language; commercial engine, free with attribution |

If your source of truth is React or HTML, the first two are built for it. If it can be said as rectangles
with known contents, an agent can build it in m0saic too, and the render needs nothing but ffmpeg: data in,
file out, on a server, in CI, from an agent, the same way every time.

Related projects: [Revideo](https://github.com/midrender/revideo), [Motion Canvas](https://github.com/motion-canvas/motion-canvas),
[Editly](https://github.com/mifi/editly), [MoviePy](https://github.com/Zulko/moviepy), [Manim](https://github.com/ManimCommunity/manim).

## Everything else

| | |
|---|---|
| **CLI** | `npm i -g m0saic`: renders anywhere ffmpeg runs; `m0saic mcp` is the agent server. The rest is in `m0saic --help` |
| **Mosaic Desktop** | macOS and Windows: edit templates on a live canvas, watch your agent work, render locally. [Download](https://m0saic.io/download) · [all releases](https://github.com/m0saic-project/mosaic-desktop-releases/releases) |
| **Mosaic Web** | [app.m0saic.io](https://app.m0saic.io): try the web-safe templates in the browser; it hands you the CLI command for the render |
| **Write templates** | `m0saic init "My Templates"` scaffolds a repo with AGENTS.md and the MCP config already in it; starters: [full](https://github.com/m0saic-project/m0saic-template-repo-starter), [base](https://github.com/m0saic-project/m0saic-template-repo-starter-base) |
| **Packages** | the language, stdlib, file formats and template utilities are public: [m0saic-dsl/m0](https://github.com/m0saic-dsl/m0), [m0saic-packages](https://github.com/m0saic-project/m0saic-packages) |
| **For agents** | [m0saic.io/agents](https://m0saic.io/agents) · [m0saic.io/llms.txt](https://m0saic.io/llms.txt) · [m0saic.io/llms-full.txt](https://m0saic.io/llms-full.txt) |
| **Community** | [Discord](https://discord.gg/ns58hGm6Mm) · [r/m0saic](https://reddit.com/r/m0saic) · [Substack](https://mosaicengine.substack.com) · [@qsbuilds](https://x.com/qsbuilds) · the [Community M](https://m0saic.io/community), one tile per contributor |
| **Learn** | [app.m0saic.io/learn](https://app.m0saic.io/learn): guided tours of Make and m0, in the browser |

## Open and commercial, plainly

The m0 language, its stdlib and file formats are Apache-2.0. The template library, the community library,
the starters and this repo are public. The engine, the CLI and the apps are commercial. **Free is the whole
engine**, every template and every resolution, with no account, for personal and commercial work; free
renders carry a small QR attribution mark in a corner. [Pro](https://m0saic.io/pricing) removes it. The
full boundary, package by package, is at [m0saic.io/open-core](https://m0saic.io/open-core).

## Issues and questions

Open an issue here for bugs, template requests and questions; this is m0saic's public front door. If m0saic
is useful to you, a star helps other people find it.

<p align="center">
  <sub>Developed and maintained by <strong>m0saic LLC</strong> · <a href="mailto:hi@m0saic.io">hi@m0saic.io</a></sub>
</p>
