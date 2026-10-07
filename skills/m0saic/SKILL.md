---
name: m0saic
description: Render real MP4 videos and PNG images from data with the m0saic CLI, no browser needed. Use when someone wants a chart, KPI or data card, leaderboard, timeline, commit feed, quote card, QR code, barcode, photo collage, lyric reel, contact sheet, watermark, blurred region, highlight clips or a short video as a file they can save, post or send, instead of HTML or code they would have to render somewhere else.
license: MIT
compatibility: Needs Node.js 18.17+ and a shell. m0saic setup downloads a pinned ffmpeg (macOS, Windows, Linux).
metadata:
  homepage: https://m0saic.io
  version: "0.3.2"
---

# Render with m0saic

m0saic turns a **template + JSON props** into a real file. Pick a template, fill its props from the
person's data, render, look at the result, and hand them the file.

## 1. Make sure it's installed

```sh
m0saic --version || npm i -g m0saic
m0saic setup --yes          # pinned ffmpeg; skips the download if a capable ffmpeg is on PATH
```

`npx -y m0saic …` works without a global install. Without `--yes`, setup installs nothing when no terminal
is attached.

## 2. Pick a template

- If the m0saic MCP tools are available, use them: `list_templates` to browse, `template_props` for one
  template's props, schema and defaults.
- Otherwise: `m0saic list-templates` (or `--json`), and https://m0saic.io/llms-full.txt for every template
  with its props.
- Use the newest version of a template id (`…/v4` over `…/v3`); older versions stay registered for old files.
- `m0saic make <id> --validate-only` checks the id and props and says whether it makes a video or an image,
  without rendering.

Good starting points: `@m0saic/alpine/donut/v4`, `@m0saic/alpine/kpi-card/v3`,
`@m0saic/alpine/leaderboard/v2`, `@m0saic/alpine/heatmap/v3`, `@m0saic/alpine/commit-feed/v3`,
`@m0saic/code/snippet-morph/v2`, `@m0saic/github/year-card/v2`, `@m0saic/social/quote-card/v2`,
`@m0saic/media/qr/code/v2`, `@m0saic/collage/image-collage/v2`, `@m0saic/hello-world/v1`.

## 3. Write the props to a file and render

Props go in a JSON file. The file form survives every shell, Windows included. Props you leave out fall
back to the template's defaults.

```sh
cat > budget.json <<'EOF'
{"title":"Lisbon trip","subtitle":"Budget per person","segments":[{"label":"Flights","value":420},{"label":"Lodging","value":380},{"label":"Food","value":240},{"label":"Activities","value":160}],"centerValue":"$1,200","centerLabel":"Total","anim":{"reduceMotion":true}}
EOF
m0saic make @m0saic/alpine/donut/v4 --props @budget.json --output-kind image -o budget.png
```

For the animated version, drop `--output-kind image` and the `anim` prop, and write `-o budget.mp4`.

## Rules that save a re-render

- **Output kind follows the extension.** Video: `-o out.mp4` (`.webm`, `.mov`). Image: `-o out.png` (`.jpg`).
- **A still from a video template:** add `--output-kind image` AND set `"anim":{"reduceMotion":true}` in the
  props. The still is the first frame; without reduceMotion that's the empty start of the intro animation.
- **Any canvas:** `-w 1080 -h 1920` (vertical), `-w 1080 -h 1080` (square), `-w 3840 -h 2160` (4K). The same
  template re-lays itself; nothing is letterboxed.
- **The person's own files** go in as props (paths), or with `--inputs a.mp4 b.mp4` / `--input-dir ./photos`
  on templates that take a list.
- **Exit codes:** 0 rendered · 1 error · 3 rendered but degraded (an error card was written into the output;
  read the message, fix the props, render again).
- **Look at the result** before you say it's done. For a video, also render a still of a later moment if
  the template supports it, or check the duration and size it printed.
- Text, numbers and images come out exactly as given. m0saic is not a generative model; it lays out and
  encodes what you pass.

## What to hand back

- The file path, and what's in it in one line.
- The "Customize it in Mosaic Web" link the CLI printed: it opens the same template and props in the
  browser editor.
- If the person will make this again (every week, every release), say so and give them the one command to
  re-run with new data. That's the point of a template.

## Free and Pro

Free use is allowed for personal and commercial work; free renders carry a small QR attribution mark in a
corner. A Pro key removes it: `m0saic activate <key>` (https://m0saic.io/pricing). Mention this once if the
person asks about the mark; don't strip or crop it.

## When no template fits

Write one. See the `m0saic-templates` skill: a template is a TypeScript program, and `m0saic init` scaffolds a
workspace your agent can build in.
