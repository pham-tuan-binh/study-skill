---
name: study
description: Literature study of research papers. Surveys the papers in a research area (e.g. "do a study on robot evals") and publishes them as a public GitHub repo of paper write-ups with figures. Use when the user asks for a literature study, survey, landscape or deep-dive of papers/systems in a field, starting from a topic or a seed paper. Not for studying for an exam or learning a subject. Produces a hub README grouped by category in chronological order, one folder per strong paper (narrative write-up + images), and one-liners with links for weaker work.
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

Identify the topic, any seed paper the user gave, and the repo name (default: kebab-case of the topic, e.g. `robot-eval-study`). Proceed with sensible defaults; ask only if the area is genuinely ambiguous. (Step 3 still requires a check-in on the shortlist, and step 8 a confirmation before publishing.)

If a seed paper is given, read it fully first (PDF via `pdftotext`, or the arXiv HTML) and note its related-work list, benchmarks and baselines. These become the first candidates.

## 2. Harvest candidates (be exhaustive)

A study is only as good as its coverage. The first pass in the original study missed most of the field, so always run all of these, in parallel:

1. **Seed references:** every system the seed paper compares against or cites as related work.
2. **Forward citations:** papers citing the seed and the 2-3 most central papers, via the Semantic Scholar API (`https://api.semanticscholar.org/graph/v1/paper/arXiv:<id>/citations?fields=title,year,externalIds&limit=100`, page with `offset`). To fetch abstracts for many papers at once, `/paper/batch` is a **POST**, not a GET: `curl -s -X POST 'https://api.semanticscholar.org/graph/v1/paper/batch?fields=title,year,abstract,externalIds' -H 'Content-Type: application/json' -d '{"ids":["arXiv:2401.01234","arXiv:2402.05678"]}'` (up to 500 ids). Passing `?ids=` returns an error.
3. **Keyword sweeps:** arXiv and web searches over the area's vocabulary for the last 12-18 months.
4. **Leaderboards and benchmarks:** every entry on the area's public leaderboards; find the paper or repo behind each non-trivial entry.
5. **Curated lists:** awesome-lists and existing surveys; also use surveys to cross-check coverage.
6. **Industry posts:** lab and company blog posts that describe a concrete system.
7. **Lab and org release sweep:** for every major lab and company in the area, check its blog, GitHub org, Hugging Face org and model launch pages for the last 18 months. Launch pages of frontier models now report robot and agent results, and many releases (datasets, frameworks, leaderboards) never get a paper or get one months later.
8. **Work hidden inside other papers:** datasets, benchmarks and harnesses are often side releases of a model or method paper whose title doesn't name them (e.g. MolmoBot-Data inside a sim-to-real model paper). Go through the area's main model papers and record anything they release.
9. **Follow parents and siblings:** when a candidate belongs to a larger project (same org, same name prefix, same repo), look up the parent project and its other releases.
10. **Hubs outside arXiv:** Hugging Face datasets/models sorted by downloads and by likes, GitHub search and stars, PyPI, and X announcements quoted in press. Release counts and download numbers are evidence of adoption.

Citation sweeps must not stop at the first page. Sweep every central hub of the area (the seed, the main benchmarks, the most-cited datasets and systems), not just 2-3 papers, and rank citers by citation count rather than paging by date. OpenAlex does this in one request (`https://api.openalex.org/works?filter=cites:<openalex id>,from_publication_date:<YYYY-MM-DD>&sort=cited_by_count:desc&per-page=200&cursor=*`, following `meta.next_cursor` until it is null); Semantic Scholar throttles heavily.

Before triage, list the adjacent categories the brief does not name (for robot evals, e.g.: embodied reasoning, safety, memory, competitions, evaluation frameworks) and decide explicitly whether each is in scope, so nothing falls out by omission.

For each candidate record: name, org, v1 release date, link, one-line description, headline result with benchmark and protocol, and evidence quality. See `references/search-playbook.md`.

## 3. Triage: strong vs weak

Only comparatively strong work gets a full page. Everything else gets a one-liner with a link. This applies to items already written too: demote them if better work appears.

- **Strong:** results on standard benchmarks with official or clearly stated protocols, real-robot or real-world evidence, external baselines, or a distinct idea the field has picked up.
- **Weak:** mock or LLM-judged evaluation, tiny or self-run evaluations with no outside baselines, gains only from same-task practice, or a narrow contribution.

Verify every headline number against the paper's full text before writing. Flag numbers measured on non-comparable protocols (self-evaluated subsets, privileged state, practice runs, demos, larger budgets) instead of putting them side by side.

**Stop and check in with the user here.** Show the ranked shortlist, one line per item (name, date, headline result, strong/weak call), and let them decide which papers get full pages before writing any. This is the most expensive step to get wrong, so do not skip it even when running with defaults.

## 4. Categorize

Derive categories from how the current field describes itself, using the terms recent papers and surveys use, not from the seed paper's framing. Re-check categories whenever the set changes. A category with one item, or one that mixes clearly different designs, is a sign the scheme is out of date. Put controlled studies and benchmarks in their own category.

Within each category, order entries chronologically by arXiv v1 date (or publication date), and put the month in each heading: `### 7. Name (Org) (Oct 2026)`. Number entries across the whole hub.

The comparison must be fair. No paper, including the seed or the user's favourite, is the reference point. Compare each paper against its own baselines and the field in general.

## 5. Write the paper pages

Each strong paper gets `<slug>/README.md` in the user's narrative format; see `references/writing-style.md` and `references/page-template.md`. Every page answers the user's five reading questions, critically, as storytelling paragraphs (not Q&A):

1. **Domain:** what domain or sub-domain it solves (name both when it is a sub-domain).
2. **Measurement:** how it measures results, what they are, and whether that is how the domain usually measures them. A non-standard metric or self-made benchmark is how you spot a weak paper; say so.
3. **Method:** how it achieves the results, and what is actually novel.
4. **Gaps:** what it cannot solve, and the vectors of improvement, as `1\)`, `2\)` … points, plus how another paper in the study could improve it.
5. **Reimplementation:** what is worth experimenting with or reimplementing, and what that requires (code, weights, data, hardware, compute).

Use the template that fits the work: method or algorithm, eval or benchmark, or dataset. Every page starts with a metadata line (Org · Date · Paper · Code · Models) and ends with a `# Retrospective` section followed by `# Sources`.

Research each paper from the full text (arXiv HTML at `https://arxiv.org/html/<id>`), the project page and the README. Never invent numbers; say "not reported".

**Figures:**
- Use 4-5 per page, taken from the arXiv HTML `<img src>` (resolve relative paths), the project page or the repo README.
- Put the overview figure right after the opening paragraphs, then results charts, method diagrams and real-robot photos between the paragraphs they illustrate.
- Download them with `curl -sL` and check each is a real image. If arxiv.org is blocked from the shell, open the image URL in a browser and take an element screenshot instead. When the arXiv HTML hides raster file names, `https://ar5iv.labs.arxiv.org/html/<id>` lists the real asset paths.
- Crop large white margins with PIL. Flatten onto white first: arXiv figures are often transparent PNGs, and a diff against white misses transparent margins.
  ```python
  from PIL import Image, ImageChops
  im = Image.open(path)
  if im.mode in ("RGBA", "LA", "P"):
      im = im.convert("RGBA")
      bg = Image.new("RGBA", im.size, (255, 255, 255, 255))
      im = Image.alpha_composite(bg, im)
  im = im.convert("RGB")
  bbox = ImageChops.difference(im, Image.new("RGB", im.size, "white")).getbbox()
  if bbox:
      pad = 20
      l, t, r, b = bbox
      im = im.crop((max(l - pad, 0), max(t - pad, 0), min(r + pad, im.width), min(b + pad, im.height)))
  im.save(path)
  ```
- When a figure has no image file (PDF-only, drawn inline in HTML, or split across several files), crop it from the rendered PDF page instead of dropping it (see `references/notion-notes.md` for a URL-based way).
- Write every caption as a complete sentence.

## 5b. Write the retrospectives

Write each page's `# Retrospective` only after every page in the study exists, because it judges the paper against the whole field as it stands now. Place it directly before `# Sources`, never on the hub and never as a callout. In 2 to 4 short paragraphs:

1. **Verdict first:** open with *As of Mon YYYY.*, since the verdict goes stale as new work appears. Then: is the work superseded, still the reference for something specific, or current? Say which, plainly ("Mainly of historical interest", "Still the only harness built for distillation", "Current").
2. **What came after:** name the entries or one-liners in the study that beat or extend it, with the comparison numbers, and flag protocol differences instead of lining up non-comparable numbers.
3. **What is still worth reading for:** the part that has not been superseded, and any caveats such as unreleased data or self-reported results.

Open with "Since then" only when something really came after; don't use it as a stock opener. Use only numbers already verified on the study's own pages, or verify new ones. Reflect the user's own comments on the page or hub when they exist.

Use the retrospectives to re-triage: work that is fully superseded and has nothing left to read for should be demoted to a one-liner (ask the user before deleting pages), and the hub numbering redone.

## 6. Assemble the hub

Build `README.md` from `references/hub-template.md`:

1. An intro paragraph saying what the study covers and how many works, plus one bullet per category.
2. `# <Topic>`, then H2 categories and H3 numbered entries. Each entry has:
   - the paper page's opening paragraphs, verbatim, up to its first figure (never the Retrospective);
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

Check that every image path and folder link in the READMEs resolves (a small Python check over `![...](...)` and `](<slug>/)` links). Then **confirm with the user before pushing**: show the repo name, owner, that it will be public, and the privacy-scrub hits you removed. The scrub relies on grep and can miss things; publishing is hard to undo.

```bash
cd <study-dir>
git init -b main && git add -A && git commit -m "Add <topic> study"
gh repo create <owner>/<study-name> --public --source . --push \
  --description "<one-line description>"
```

After pushing, report the repo URL, the counts (full pages, one-liners, figures) and any unverified claims.

## Optional: Notion mirror

If the user also wants Notion, see `references/notion-notes.md`. The main traps are full-page rewrites that misplace blocks, child-page blocks that must stay on the parent, and upload rate limits.
