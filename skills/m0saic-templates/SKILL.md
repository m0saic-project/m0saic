---
name: m0saic-templates
description: Write a new m0saic video or image template, a TypeScript program that returns a layout string plus one source per rectangle and renders to MP4 or PNG, in a template repo scaffolded by m0saic init. Use when the person wants a video or card no existing m0saic template makes, wants a reusable template for something they make every week or every release, or asks to build, fix or publish a template.
license: MIT
compatibility: Needs Node.js 18.17+ (20+ recommended for the MCP server), npm and a shell.
metadata:
  homepage: https://m0saic.io/developers
  version: "0.3.2"
---

# Write an m0saic template

A template is a TypeScript program. It returns an **m0 layout string** (rectangles on a canvas) and one
source per rectangle (text, an image, a video, a colour, another template), and the m0saic CLI compiles it
to MP4 or PNG. Templates are deterministic: the same props and canvas give the same pixels.

## 1. Set up the workspace

```sh
npm i -g m0saic
m0saic setup --yes
m0saic init "My Templates" && cd my-templates
npm install && npm run build
```

The workspace has `AGENTS.md`, `CLAUDE.md` and `.mcp.json` in it. **Read `AGENTS.md` and follow it from here
on**; it is the authoritative guide for that repo and its checks.

## 2. Connect the MCP server

It gives you real tools instead of guessing: `knowledge_search` / `knowledge_read` (the knowledge base),
`list_templates` / `template_props` (every existing template), `validate_m0` (the layout validator),
`render_still` (one frame), `doctor` (the publish checks), and a live link into Mosaic Desktop.

- Claude Code: the workspace's `.mcp.json` already registers it (or this plugin does).
- Codex: `codex mcp add m0saic -- npx -y m0saic mcp`
- Any other MCP client: run `npx -y m0saic mcp` as a stdio server.

MCP servers usually load when a session starts. If the tools aren't there yet, tell the person how to
restart you inside the workspace, and use the CLI until then.

## 3. Read before you write

Before writing code, read the knowledge base: `node_modules/@m0saic/knowledge/README.md` first, or
`knowledge_search` for the concept you need (splits, overlays, text, timing, quantization). Then look at the
example repos `AGENTS.md` lists for templates close to what the person wants, and copy their structure.
The layout rules are exact; don't invent grammar. Check every layout string with `validate_m0`.

## 4. Build the template

- Ask what they want, or use what they said. A small, specific template beats a general one: the canvas it
  will be shown on (1080x1920 for a phone, 1920x1080 for a screen), 3–6 props, sensible defaults so it
  renders with no props at all.
- Every word, number and colour the person might change is a prop.
- Loop on `npm run build` and `npm run verify` until both are green. Check frames with `render_still` as you go.

## 5. Render and look

```sh
m0saic make "<template id>" --template-repo . -o out.mp4      # or -o out.png for a still
```

Look at the output before you call it done. Exit code 3 means it rendered with an error card: read the
message and fix the cause. Run `m0saic doctor .` before anyone publishes the repo.

## 6. Hand it over

- Tell the person the template id and the exact render command, with their data in a props file.
- If they'll make this again, show them how to re-run it with new data (a cron job, their agent, or Mosaic
  Desktop).
- To edit it by hand: Mosaic Desktop (https://m0saic.io/download) → Templates → Add source → this folder,
  then open it in Make (`desktop_open_in_make` if the MCP server is connected).

## Rendering with existing templates

If an existing template already does the job, don't write a new one; see the `m0saic` skill.
