---
name: explain-diff-html
description: Use when the user asks for a rich explanation of a code change, diff, branch, or PR. Produces HTML output.
---

# Explain Diff

Please make me a rich, interactive explanation of the specified code change.

It should have these sections:

- Background: Explain the existing system relevant to this change. (You should broadly explore surrounding code for this.) We don't know how much the reader already knows, so include a deep background for beginners (note that it can be skipped if the reader is already familiar), and then a more narrow background directly relevant to the change.
- Intuition: Explain the core intuition for the code change. The focus here is to explain the essence, not the full details. Use concrete examples with toy data. Use figures and diagrams liberally.
- Code: Do a high-level walkthrough of the changes to the code, ordered like a tree: trunk changes first, then leaf changes.
  - Trunk code is code with a high blast radius: core infrastructure, entry points, shared abstractions and utilities, public interfaces, data models and migrations, configuration, and anything many other parts of the system depend on. A mistake here breaks things far away, so it needs deep, manual review.
  - Leaf code is isolated: a single component, page, script, or test whose failure stays local and that automated validation (tests, types, linters) can largely cover.
  - Classify each changed area by how much depends on it, not by file location. Check its callers or importers when it isn't obvious.
  - Split the walkthrough into two visibly labelled parts, "Trunk changes" and "Leaf changes", and mirror that split in the table of contents. Within each part, group/order the changes in an understandable way.
  - Spend most of the depth on the trunk: what it changes, who depends on it, and what could break downstream. Point out what a reviewer should check by hand. Leaf changes can be covered more briefly, noting how they build on the trunk changes and what tests or checks validate them.
  - If the diff has no trunk changes (or no leaf changes), say so in one line and skip that part.
- Final Quiz: Come up with five questions that test the reader's knowledge of this PR. This should be medium difficulty, difficult enough that you actually need to understand the substance of the PR to answer them, but not gotchas. The goal is to help the reader make sure that they've actually understood.

Quizzes:

- Quiz at every level of the outline:
  - End each **subsection** with a single interactive multiple-choice question checking the one idea that subsection just taught. This applies to every subsection you create — e.g. the deep beginner background and the change-specific background, each major idea or worked example in the Intuition, and each group of code changes in the Code walkthrough.
  - End each top-level **content section** (Background, Intuition, Code) with a short quiz of one or two questions covering the section as a whole.
  - Then present the five-question Final Quiz at the end.
- Keep the levels distinct so they don't feel repetitive. A subsection question is local: it checks the idea just explained, using the concrete example just given. A section-level question should test how the subsections fit together — a comparison, a consequence, or a case that needs two subsections at once — rather than restating a subsection question. If a section genuinely has no subsections, its section-level quiz is enough.
- All quizzes are interactive multiple-choice: when the user clicks an answer, tell them whether they were correct and give feedback.
- Vary the answer options so the correct one isn't guessable from its shape. In particular, do NOT make the correct answer reliably the longest or most detailed option — across all questions, the correct answer's length and position should be roughly evenly distributed, and distractors should often be as long or longer than the correct answer.

Format:

- Output a single self-contained HTML file which includes CSS and JavaScript. Make the whole thing one long page with section headers and a table of contents. Don't use tabs for the top-level structure. Basic responsive styling so you can view it on a phone is nice too. Save the file in `~/ai-artifacts/` (create the directory if it doesn't exist) so it persists outside the code repo and out of version control, and make sure the filename always starts with today's date in `YYYY-MM-DD-` format, because it helps keep the files time-sorted. For example: ~/ai-artifacts/2026-01-12-explanation-<slug>.html Do not open the file
  automatically (e.g. with `open`). Instead, end your reply with a clickable `file://` link
  to the saved file (the absolute path with `~` expanded to the home directory) so the user
  can open it themselves.
- **The table of contents belongs in a sticky sidebar, not in a block at the top of the page.**
  Lay the page out as two columns: a fixed-width left sidebar (~270px, `position: sticky; top: 0;
  height: 100vh; overflow-y: auto`) holding the contents, and the article in its own column beside
  it. Nest the subsections under their sections so the whole outline is visible at once, and
  highlight the entry for the section currently in view (a throttled scroll listener or an
  IntersectionObserver is plenty). On narrow screens (~920px and below) drop to a single column and
  turn the sidebar into a `<details>` disclosure at the top of the page, closed by default, so it
  costs one line rather than a screenful.
- Please write with the clarity and flow of Martin Kleppmann, making it engaging and written in classic style. Transitions between sections should be smooth.
- Some tips on diagrams. Ideally, you should pick a small number of diagram families that can be reused throughout the explanation to explain various cases. Some useful kinds of diagrams:
  - A very simplified version of the UI that the user sees in the app, to explain UI changes.
  - A system diagram showing data flow or communication between components. Make sure to include example data here!
- Don't use ASCII diagrams. Always use simple HTML designs for your diagrams, HTML lists for lists of things, etc.
  - For code blocks, always use `<pre>` tags. If you use a custom styled div instead, it **must** have
    `white-space: pre-wrap` in its CSS, or the browser will collapse all newlines into a single line.
    Before saving the file, scan each code block in the HTML source and confirm its CSS includes
    `white-space: pre` or `pre-wrap`.
- Use callouts for key concepts or definitions, important edge cases, etc.
