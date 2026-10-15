# Working on this LaTeX project

This is an Overleaf project. The hedwigai MCP server (`https://mcp.hedwigai.com`) checks its references and compiles it.

## Check references

1. Turn the paper into markdown in one piece:
   - Render each `\cite{key}` as the text the compiled PDF would show.
   - Put the `.bib` entries under `## References`, one formatted entry per line, each with its DOI.
   - Do not fix entries first: the point is to check what was written.
2. Tell the user the check is a billed run (a few dollars for a long thesis) and get a yes.
3. Call `create_workbook` with `instruction` set to the whole markdown and `mode: "latticeroom"`.
4. Poll `read_workbook` until `live_run` is empty.
5. Read `inference`: one JSON line per reference and per citing sentence, with `verdict`, `url`, `quote` and `note`.
6. Report the counts first. Then list each reference that was not found or differs, and each citing sentence that is contradicted or unsupported.

Never call a reference fake: "not found in Crossref or on the web" is the finding. Take any replacement from a record the check found, never from memory.

For one or two references, skip the run: `biotech literature crossref "<DOI or exact title>"` in the hedwigai `bash` tool returns the registered record.

## Compile

Call `render_document` with `format: "latex"`:
- `source` is the main `.tex`.
- `files` holds the rest: each `.tex`, `.bib`, `.cls` or `.sty` as `{path, text}`, and each image as `{path, base64}`.

It returns a link to the PDF, or the compiler's message with the failing line. Fix the line and compile again.

Avoid `minted` (use `listings`), biber (use BibTeX) and LuaLaTeX.
