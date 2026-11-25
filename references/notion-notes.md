# Optional Notion mirror: lessons learned

- **Page structure:** create one hub page, with each paper as a child page. List the children inside a collapsed "All pages" toggle at the bottom of the hub. A child page's block must stay somewhere on its parent, or Notion deletes the subpage.
- **Editing:** prefer targeted `update_content` and `insert_content` edits over `replace_content`. Full rewrites of a long page have misplaced images into the wrong entry and shuffled child-page blocks. If you do a full rewrite, re-fetch the page afterwards and diff it against the intended content.
- **Concurrency:** never run a full-page rewrite while the user may be editing the same page, because edits collide and blocks move.
- **Image uploads:**
  - upload with `create-attachment` and a `source_url`, one at a time;
  - on a rate limit, wait the stated number of seconds and retry;
  - SVG and WebP upload fine;
  - the filename extension must match the file type.
- **Numbered points:** escape them as `1\)` so Notion keeps them as plain paragraphs instead of turning them into numbered lists.
- **Auto-linking:** Notion turns words like `env.py` or `robo.show` into links. Wrap them in code formatting.
- **Comments:** keep the `<span discussion-urls>` wrappers when rewriting, so the user's comments stay anchored. Check afterwards with `get-comments`.
- **Callouts and tables:** don't use them. The user dislikes both.
