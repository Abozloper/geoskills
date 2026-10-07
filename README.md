# geoskills

A snapshot of 16 installed Generative Engine Optimization (GEO) skills, with both Claude and Codex variants, five Claude agent definitions, Python helpers, and JSON-LD schema templates. Captured on 2026-10-07.

Based on [Zubair Trabzada's geo-seo-claude](https://github.com/zubair-trabzada/geo-seo-claude). Original copyright and MIT license are preserved in [LICENSE](LICENSE). This is an independent snapshot of installed files, not an upstream release or official fork. Local adaptations may differ from upstream.

## Contents

- `claude/skills/`: 16 Claude skill folders, including helper scripts and schema templates under `geo/`.
- `claude/agents/`: five specialist agent prompts used by the audit orchestrator.
- `codex/skills/`: 16 locally installed Codex-adapted skill folders and their supporting files.
- `MANIFEST.csv`: SHA-256 hashes of all copied files for snapshot integrity.
- `requirements.txt`: packages imported by the Python helpers; versions are not locked.

## How the toolkit works

These are instructions for an AI coding assistant, supported by small Python utilities. They are not a standalone search-ranking engine. The main `geo` skill routes requests to specialist skills. A full audit discovers the business type and key pages, divides analysis among five specialists, then combines their findings into a score and prioritized report.

The five agent prompts cover AI visibility, platform analysis, technical SEO, content quality, and structured data. A host with delegation support can run them in parallel; otherwise an assistant can perform those stages sequentially. The copied Codex prompts still contain Claude-style tool names, agent references, and legacy paths; host-specific adaptation is required.

The documented composite score weights AI citability/visibility at 25%, brand authority at 20%, content quality at 20%, technical foundations at 15%, structured data at 10%, and platform optimization at 10%. Scores are diagnostic heuristics, not probabilities or guarantees of AI citations. The main workflow specifies a 50-page crawl cap, 30-second fetch timeout, robots.txt compliance, request pacing, and duplicate filtering; individual helpers do not necessarily enforce all orchestration rules.

## Skill catalog

Each link opens the complete Claude workflow. The same named folder exists under `codex/skills/`.

| Skill | How it works and what it produces |
| --- | --- |
| [geo](claude/skills/geo/SKILL.md) | Routes audit, page, quick, and specialist requests; defines discovery, orchestration, scoring, and deliverables. |
| [geo-audit](claude/skills/geo-audit/SKILL.md) | Coordinates the full audit and synthesizes `GEO-AUDIT-REPORT.md` with a prioritized action plan. |
| [geo-brand-mentions](claude/skills/geo-brand-mentions/SKILL.md) | Examines brand/entity presence across citation-relevant platforms; produces authority findings and recommendations. |
| [geo-citability](claude/skills/geo-citability/SKILL.md) | Evaluates passage structure, factual specificity, and answer readiness; supplies scores and rewrite suggestions. |
| [geo-compare](claude/skills/geo-compare/SKILL.md) | Compares baseline and current audits, score changes, and completed actions for a monthly progress report. |
| [geo-content](claude/skills/geo-content/SKILL.md) | Assesses experience, expertise, authority, trust, and content structure for an improvement plan. |
| [geo-crawlers](claude/skills/geo-crawlers/SKILL.md) | Checks robots.txt, meta directives, and headers for AI crawler access; builds an access map. |
| [geo-llmstxt](claude/skills/geo-llmstxt/SKILL.md) | Validates an existing llms.txt or generates one from site content and links. |
| [geo-platform-optimizer](claude/skills/geo-platform-optimizer/SKILL.md) | Assesses readiness separately for Google AI Overviews, ChatGPT, Perplexity, Gemini, and Bing Copilot. |
| [geo-proposal](claude/skills/geo-proposal/SKILL.md) | Converts audit findings into service packages, pricing, timeline, and a client proposal. |
| [geo-prospect](claude/skills/geo-prospect/SKILL.md) | Tracks leads, status, notes, deal values, and audit history in a local JSON CRM. |
| [geo-report](claude/skills/geo-report/SKILL.md) | Consolidates findings into a business-facing `GEO-CLIENT-REPORT.md`. |
| [geo-report-pdf](claude/skills/geo-report-pdf/SKILL.md) | Describes a pandoc-to-HTML-to-Chrome PDF workflow; required report templates are missing from this installation. |
| [geo-schema](claude/skills/geo-schema/SKILL.md) | Detects structured data, checks completeness, and generates JSON-LD using bundled templates. |
| [geo-technical](claude/skills/geo-technical/SKILL.md) | Checks crawlability, indexability, security, URLs, mobile behavior, rendering, and performance. |
| [geo-update](claude/skills/geo-update/SKILL.md) | Compares and copies upstream updates into an installation; it targets the original upstream repository, not this snapshot. |

## Setup and usage

Clone this repository, then choose the matching assistant variant. For a fresh Claude installation, copy the folders inside `claude/skills/` into `~/.claude/skills/`, and the files inside `claude/agents/` into `~/.claude/agents/`. Back up existing matching folders before replacing them. The saved Codex variant came from `~/.agents/skills/`; its internal paths and delegation instructions need review before use on another machine. Agent prompts are included under `claude/agents/` for reference, not as a verified Codex agent configuration.

Install helper dependencies in a Python virtual environment:

```sh
python -m venv .venv
# Activate .venv using your shell's activation command, then:
python -m pip install -r requirements.txt
```

In an assistant configured to load these skills, example requests are:

```text
/geo audit https://example.com
/geo citability https://example.com/article
/geo crawlers https://example.com
/geo llmstxt https://example.com
/geo report https://example.com
/geo prospect list
/geo compare example.com
```

Slash commands are assistant instructions, not shell commands. If the host does not expose this slash-command syntax, explicitly ask it to read the appropriate SKILL.md and follow that workflow for your URL.

A usual sequence is audit, review recommendations, implement selected changes, generate a client report, and later compare a new audit with the baseline. Prospect tracking and proposals are optional agency workflows.

## Python helpers and templates

Under each variant's `skills/geo/`:

| File | Role |
| --- | --- |
| `scripts/fetch_page.py` | Fetches HTML and extracts text, metadata, headers, and structured data. |
| `scripts/citability_scorer.py` | Applies passage-level citation-readiness heuristics. |
| `scripts/brand_scanner.py` | Supports brand-presence research. |
| `scripts/llmstxt_generator.py` | Produces an llms.txt representation of a site. |
| `scripts/crm_dashboard.py` | Displays the local prospect pipeline using Rich. |
| `scripts/webapp/app.py` | Runs the Flask CRM interface locally on port 5050, with bundled HTML templates. |
| `schema/*.json` | Six JSON-LD templates for article/author, local business, organization, products, SaaS, and website search. |

For example, from the repository root:

```sh
python claude/skills/geo/scripts/fetch_page.py https://example.com
python claude/skills/geo/scripts/crm_dashboard.py --help
python claude/skills/geo/scripts/webapp/app.py
```

CRM state is stored outside the repository under `~/.geo-prospects/`. No live prospect database, client audits, account settings, or credentials were copied into this snapshot.

## Snapshot limitations

- The PDF skill references `geo-report-style.css` and `geo-report-template.html`; neither is present locally. Restore compatible templates before using that workflow. Its Chrome path is macOS-specific and needs adjustment on Windows/Linux.
- Legacy `~/.Codex/skills/` references do not match the observed `~/.agents/skills/` source location. Copied source files are preserved verbatim; adapt execution paths to your installation.
- Upstream market statistics, platform claims, and scoring rules are retained as source material, not independently verified current facts.
- The updater can overwrite local adaptations. Review its diff before applying updates.
- Packaging validation checks copied-file integrity and JSON parsing. Live audits, assistant delegation, Python execution, and PDF rendering have not been tested as part of this export.
