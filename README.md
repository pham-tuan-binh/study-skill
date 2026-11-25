# study-skill

An agent skill that runs a literature study on a research area and publishes it as a public GitHub repo. The repo has a hub README organized by category in chronological order, a narrative write-up with figures for each strong paper, and a one-line summary with a link for weaker work.

## Install

```bash
npx skills add pham-tuan-binh/study-skill
```

## Use

Ask your agent for a study, for example "do a study on robot evals" or "study the papers around arXiv:2611.09892".

The skill runs eight steps:
1. Scope the topic.
2. Harvest candidates from citations, keyword sweeps, leaderboards, curated lists and industry posts.
3. Triage them into strong and weak.
4. Categorize them by the field's current vocabulary.
5. Write the paper pages in a fixed narrative format.
6. Assemble the hub README.
7. Scrub private details.
8. Publish with `gh`.

## Files

- `SKILL.md`: the workflow.
- `references/writing-style.md`: voice and formatting rules.
- `references/page-template.md`: the per-paper narrative skeleton.
- `references/hub-template.md`: the hub README skeleton.
- `references/search-playbook.md`: how to make the candidate search exhaustive.
- `references/notion-notes.md`: lessons for an optional Notion mirror.

Example output: [robot-harness-study](https://github.com/pham-tuan-binh/robot-harness-study).
