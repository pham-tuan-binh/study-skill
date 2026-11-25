---
name: study
description: Run a literature study on a research area (e.g. "do a study on robot evals") and publish it as a public GitHub repo of paper write-ups with figures. Use when the user asks for a study, survey, landscape or deep-dive of papers/systems in a field, starting from a topic or a seed paper. Produces a hub README grouped by category in chronological order, one folder per strong paper (narrative write-up + images), and one-liners with links for weaker work.
---

# Study

A study turns a research area into a public GitHub repo:

```
<study-name>/
  README.md                 # hub: intro, categories, entries, other notable work
  <slug>/README.md          # one narrative write-up per strong paper
  <slug>/images/NN-name.ext # figures from that paper
  <slug>/images/hub.ext     # the figure shown on the hub
```

Work in this order. Use parallel subagents for research and writing (about two papers per agent), and keep the hub assembly in one place so edits never collide.

## 1. Scope

Identify the topic, any seed paper the user gave, and the repo name (default: kebab-case of the topic, e.g. `robot-eval-study`). Proceed with sensible defaults; ask only if the area is genuinely ambiguous.

If a seed paper is given, read it fully first (PDF via `pdftotext`, or the arXiv HTML) and note its related-work list, benchmarks and baselines. These become the first candidates.

## 2. Harvest candidates (be exhaustive)

A study is only as good as its coverage. The first pass in the original study missed most of the field, so always run all of these, in parallel:

1. **Seed references:** every system the seed paper compares against or cites as related work.
2. **Forward citations:** papers citing the seed and the 2-3 most central papers, via the Semantic Scholar API (`https://api.semanticscholar.org/graph/v1/paper/arXiv:<id>/citations?fields=title,year,externalIds&limit=100`, page with `offset`).
3. **Keyword sweeps:** arXiv and web searches over the area's vocabulary for the last 12-18 months.
4. **Leaderboards and benchmarks:** every entry on the area's public leaderboards; find the paper or repo behind each non-trivial entry.
5. **Curated lists:** awesome-lists and existing surveys; also use surveys to cross-check coverage.
6. **Industry posts:** lab and company blog posts that describe a concrete system.

For each candidate record: name, org, v1 release date, link, one-line description, headline result with benchmark and protocol, and evidence quality. See `references/search-playbook.md`.

## 3. Triage: strong vs weak

Only comparatively strong work gets a full page. Everything else gets a one-liner with a link. This applies to items already written too: demote them if better work appears.

- **Strong:** results on standard benchmarks with official or clearly stated protocols, real-robot or real-world evidence, external baselines, or a distinct idea the field has picked up.
- **Weak:** mock or LLM-judged evaluation, tiny or self-run evaluations with no outside baselines, gains only from same-task practice, or a narrow contribution.

Verify every headline number against the paper's full text before writing. Flag numbers measured on non-comparable protocols (self-evaluated subsets, privileged state, practice runs, demos, larger budgets) instead of putting them side by side.

## 4. Categorize

Derive categories from how the current field describes itself, using the terms recent papers and surveys use, not from the seed paper's framing. Re-check categories whenever the set changes. A category with one item, or one that mixes clearly different designs, is a sign the scheme is out of date. Put controlled studies and benchmarks in their own category.

Within each category, order entries chronologically by arXiv v1 date (or publication date), and put the month in each heading: `### 7. Name (Org) (Oct 2026)`. Number entries across the whole hub.

The comparison must be fair. No paper, including the seed or the user's favourite, is the reference point. Compare each paper against its own baselines and the field in general.

## 5. Write the paper pages

Each strong paper gets `<slug>/README.md` in the user's narrative format; see `references/writing-style.md` and `references/page-template.md`. In short:

1. A metadata line: Org · Date · Paper · Code · Models.
2. Then storytelling paragraphs, in order:
   1. the problem it solves;
   2. how it measures success, and the results against its baselines;
   3. how it works;
   4. the remaining gaps, as `1\)`, `2\)` … points;
   5. what to explore next, plus how another paper in the study could improve it.
3. A `# Sources` section.

Research each paper from the full text (arXiv HTML at `https://arxiv.org/html/<id>`), the project page and the README. Never invent numbers; say "not reported".

**Figures:**
- Use 4-5 per page, taken from the arXiv HTML `<img src>` (resolve relative paths), the project page or the repo README.
- Put the overview figure right after the opening paragraphs, then results charts, method diagrams and real-robot photos between the paragraphs they illustrate.
- Download them with `curl -sL` and check each is a real image.
- Crop large white margins with PIL (`ImageChops.difference` against white, then `getbbox`, plus about 20 px of padding).
- Write every caption as a complete sentence.

## 6. Assemble the hub

Build `README.md` from `references/hub-template.md`:

1. An intro paragraph saying what the study covers and how many works, plus one bullet per category.
2. `# <Topic>`, then H2 categories and H3 numbered entries. Each entry has:
   - the paper page's opening paragraphs, verbatim, up to its first figure;
   - `Read more: [Title](<slug>/)`, placed before the image so readers don't miss it;
   - the hub figure.
3. `# Other notable work`: one sentence explaining the list, then chronological bullets `**Name** (Mon YYYY): one line. [arXiv:id](url)`.
4. `## Notes`: figures belong to their authors, and numbers are as reported, with protocol caveats.

## 7. Privacy scrub

The repo is public. Before committing, grep for and remove:
- internal project names, company names and plans of the user's team;
- personal names and emails;
- internal URLs (Notion, Slack, Linear);
- local file paths.

Keep the user's own opinions only if they are about the papers.

## 8. Publish

```bash
cd <study-dir>
git init -b main && git add -A && git commit -m "Add <topic> study"
gh repo create <owner>/<study-name> --public --source . --push \
  --description "<one-line description>"
```

Check that every image path and folder link in the READMEs resolves (a small Python check over `![...](...)` and `](<slug>/)` links) before pushing. Report the repo URL, the counts (full pages, one-liners, figures) and any unverified claims.

## Optional: Notion mirror

If the user also wants Notion, see `references/notion-notes.md`. The main traps are full-page rewrites that misplace blocks, child-page blocks that must stay on the parent, and upload rate limits.
