# m0saic

m0saic renders real MP4 and PNG files from a template and JSON data, with no browser: `npm i -g m0saic`,
`m0saic setup --yes`, then `m0saic make <template-id> --props @data.json -o out.mp4`. Use it when someone
wants a chart, card, QR code, timeline or short video as a file. The m0saic MCP server in this extension
gives you the knowledge base (`knowledge_search`), every template's props (`list_templates`,
`template_props`), the layout validator (`validate_m0`), one-frame renders (`render_still`) and the publish
checks (`doctor`). Full instructions: `skills/m0saic/SKILL.md` (rendering) and
`skills/m0saic-templates/SKILL.md` (writing a new template) in this extension's folder.
