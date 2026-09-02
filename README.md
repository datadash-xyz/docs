# docs

Mintlify documentation site for Datadash. `pnpm dev` runs a local preview.

## Blog

Posts live in `blog/`, are registered under the Blog tab in `docs.json`, and are
rendered by two components in `snippets/blog-ui.mdx`. Layout lives in
`style.css` under the "Blog template" heading. The top-level tab row is hidden
on every `/blog` page (also `style.css`).

### Adding a post

Create `blog/<slug>.mdx`:

```mdx
---
title: "Post title"
description: "One-line summary, reused as the lede."
mode: "custom"
---

import { PostLayout } from "/snippets/blog-ui.mdx";

<PostLayout
  category="Engineering"
  date="2 September 2026"
  readingTime="6 min read"
  title="Post title"
  description="One-line summary, reused as the lede."
  author="Datadash Engineering"
  role="Engineering"
>

Markdown body goes here.

</PostLayout>
```

`mode: "custom"` is required — it hands the page a blank canvas so PostLayout can
supply the masthead and both rails. The body keeps Mintlify's own typography,
code blocks and components, so anything valid on a docs page works here.

Then add the entry to `blog/index.mdx` (first in the list becomes the featured
post) and register the page in `docs.json` under the Blog tab.

Optional post fields: `avatar` (author image), `cover` (image for the index
card), `coverTitle` (text for the generated cover panel when there is no image),
`related` (a node rendered under the article).

### Working on the components

Mintlify's snippet loader imposes three rules that are easy to trip over. All
three fail as an unhelpful `Could not parse expression with acorn`, or as a
component that renders on the server and then vanishes on hydration:

1. An `export` block may not contain blank lines — a blank line ends the block.
2. An `export` block may not contain JS comments (`//` or `/* */`).
3. Exports cannot reference each other. Each component is evaluated in its own
   scope, so every one must be self-contained.

Notes that would otherwise be code comments live in `{/* ... */}` blocks between
the exports.
