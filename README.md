# Overleaf MCP: check and compile your Overleaf paper from Claude Code, Codex or Cursor

Work on an Overleaf project with the AI agent you already use. Through one hosted MCP server, [hedwigai](https://www.hedwigai.com/overleaf-mcp), your agent can:

- **Check every reference in the `.bib`.** Each entry is looked up in Crossref, at doi.org and on the web. It comes back found, found with a different year or authors, or not found. A DOI that belongs to a different paper, and a paper that has been retracted, are flagged.
- **Check every sentence that cites a reference.** The cited work is read, and the sentence comes back supported, contradicted or unsupported, with the passage that settles it.
- **Compile the project to a PDF.** Real TeX (Tectonic), with BibTeX and journal classes such as IEEEtran, acmart and elsarticle, up to 4 minutes a run, with nothing to install. If it fails, the failing line comes back for the agent to fix.

Overleaf has no MCP server of its own. Most community servers reach a project through Overleaf's Git integration (a paid Overleaf plan) or your login. Some compile with a TeX you install yourself, and they match `\cite` keys to the `.bib`. None of them checks that a reference is real. This repository is the setup for doing that, and it works with Overleaf's free plan.

> hedwigai is not affiliated with Overleaf.

## 1. Get your Overleaf project onto your machine

Use any one of these:

| Overleaf plan | How |
| --- | --- |
| Free | In the editor: **Menu → Download → Source** (or **File → Download as source (.zip)**). Unzip it. |
| Paid | `git clone https://git.overleaf.com/<project id>` (the project's Git link), then `git push` your agent's changes back. |
| Paid | GitHub sync: pull the repository Overleaf syncs to. |

Open that folder in your agent.

## 2. Connect the MCP server

The server is `https://mcp.hedwigai.com`. You sign in once with OAuth. A new account starts with $1 of credit and needs no card.

**Claude Code**

```sh
claude mcp add --transport http --scope user hedwigai https://mcp.hedwigai.com
```

Then run `/mcp`, choose **hedwigai** and **Authenticate**. Or copy this repository's [`.mcp.json`](.mcp.json) into your project for a project-scoped server.

**Codex**

```sh
codex mcp add hedwigai --url https://mcp.hedwigai.com
codex mcp login hedwigai
```

**Cursor**: add this to `~/.cursor/mcp.json`, then click **Connect**:

```json
{ "mcpServers": { "hedwigai": { "url": "https://mcp.hedwigai.com" } } }
```

**Claude (web or desktop)**: Settings → Connectors → Add custom connector, and paste `https://mcp.hedwigai.com`.

**Skills (optional).** In Claude Code, `/plugin marketplace add un-seen/hedwigai-skills` then `/plugin install hedwigai@hedwigai` adds the `reference-checker` and `latex-papers` skills, which walk the agent through each step.

## 3. Ask

Copy [`AGENTS.md`](AGENTS.md) (Codex, Cursor) or [`CLAUDE.md`](CLAUDE.md) (Claude Code) into your project so the agent knows the workflow. Then ask:

```text
Check every reference in references.bib and tell me which ones are wrong.
Check that each sentence citing a paper says what that paper says.
Compile main.tex with render_document and fix any errors.
```

## Try it on the example

[`example/`](example) is a short paper whose `refs.bib` holds two planted problems:

- `pijoanmas2006` cites Pijoan-Mas (2006) with DOI `10.1016/j.red.2005.11.001`. That DOI is registered to Domeij and Flodén's paper in the same issue. The right one is `…11.002`.
- `wakefield1998` cites a *Lancet* paper that was retracted in 2010.

Open `example/` in your agent, connect the server and ask it to check the references. Both problems come back flagged. The paper also compiles as it stands.

## No agent? Check a .bib in the browser

These free tools need no account (3 checks a day, 25 entries each):

- [BibTeX checker](https://www.hedwigai.com/bibtex-checker): paste a `.bib` from Overleaf or Zotero.
- [Retraction checker](https://www.hedwigai.com/retraction-checker): find retracted papers in a reference list.
- [DOI checker](https://www.hedwigai.com/doi-checker): check that each DOI exists and names the paper you mean.
- [Citation checker](https://www.hedwigai.com/citation-checker): match every in-text citation to the reference list.

## What it costs

Compiling is free. Checking references and citing sentences is billed per reference from prepaid credit; a thesis with 80 references comes to a few dollars. See [pricing](https://www.hedwigai.com/pricing).

## Limits

- No live co-editing, track changes or history: those stay in Overleaf.
- Shell escape is off, so `minted` does not run (use `listings`).
- No biber (use BibTeX) and no LuaLaTeX.
- The TeX package set is TeX Live 2022's.
- "Not found" is a finding, not proof of fabrication: books, theses, reports and very new or non-English works are often missing from Crossref.

## More

- [Overleaf MCP for Claude Code](https://www.hedwigai.com/overleaf-mcp): the comparison with community Overleaf MCP servers.
- [A LaTeX MCP server](https://www.hedwigai.com/blog/latex-from-your-agent): a whole IEEE paper compiled from Claude Code.
- [hedwigai-skills](https://github.com/un-seen/hedwigai-skills): the agent skills.

MIT licensed. Issues and pull requests are welcome.
