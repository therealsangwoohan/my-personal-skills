---
name: explain-concept-html
description: Use when the user asks for a rich explanation of any concept — a whole repository or codebase, a system's architecture, an algorithm, a technical question a colleague asked, a tool, a protocol, etc. Produces HTML output. For explaining a specific code change/diff/branch/PR, use explain-diff-html instead.
---

# Explain Concept

Please make me a rich, interactive explanation of the specified concept. The concept can be
anything: how a whole repository or codebase works, a system's architecture, an algorithm or
data structure, a protocol, a tool or library, or a technical question a colleague asked
("how does our auth actually work?", "what is a Merkle tree?", "why do we use X over Y?").

First, figure out what you're explaining and gather the material:

- If the concept lives in code (a repo, a subsystem, a module), explore it broadly first —
  read the entry points, the key modules, configuration, and how the pieces talk to each
  other. Don't guess; ground the explanation in what's actually there, citing files and
  paths where it helps.
- If it's a general technical question not tied to this codebase, rely on your own knowledge
  (and any docs the user points you at). Still ground it in concrete, correct specifics.
- If the scope is ambiguous (e.g. "explain the repo" — all of it? one service?), pick the
  most useful interpretation and say what you scoped it to, rather than stalling.

It should have these sections. Adapt the headings to the concept, but keep the spirit:

- Background: Explain the surrounding context the concept sits in. We don't know how much the
  reader already knows, so include a deep background for beginners (note that it can be
  skipped if the reader is already familiar), and then a more narrow background directly
  relevant to the concept at hand.
- Intuition: Explain the core intuition. The focus here is to explain the essence, not the
  full details. Use concrete examples with toy data. Use figures and diagrams liberally.
  This is the most important section — a reader who reads only this should come away with the
  right mental model.
- Details: Do a structured walkthrough of how it actually works. For a codebase, walk the key
  components/files and how data and control flow between them, grouped/ordered in an
  understandable way. For an algorithm or protocol, walk the mechanism step by step. For a
  design question, lay out the real trade-offs and why the current answer is what it is.
- Final Quiz: Come up with five questions that test the reader's understanding. Medium
  difficulty: hard enough that you actually need to understand the substance to answer them,
  but not gotchas. The goal is to help the reader confirm they've actually understood.

Quizzes:

- Quiz at every level of the outline:
  - End each **subsection** with a single interactive multiple-choice question checking the
    one idea that subsection just taught. This applies to every subsection you create — e.g.
    the deep beginner background and the concept-specific background, each major idea or
    worked example in the Intuition, and each component or step in the Details walkthrough.
  - End each top-level **content section** (Background, Intuition, Details) with a short quiz
    of one or two questions covering the section as a whole.
  - Then present the five-question Final Quiz at the end.
- Keep the levels distinct so they don't feel repetitive. A subsection question is local: it
  checks the idea just explained, using the concrete example just given. A section-level
  question should test how the subsections fit together — a comparison, a consequence, or a
  case that needs two subsections at once — rather than restating a subsection question. If a
  section genuinely has no subsections, its section-level quiz is enough.
- All quizzes are interactive multiple-choice: when the user clicks an answer, tell them
  whether they were correct and give feedback.
- Vary the answer options so the correct one isn't guessable from its shape. In particular, do
  NOT make the correct answer reliably the longest or most detailed option — across all
  questions, the correct answer's length and position should be roughly evenly distributed,
  and distractors should often be as long or longer than the correct answer.

Format:

- Output a single self-contained HTML file which includes CSS and JavaScript. Make the whole
  thing one long page with section headers and a table of contents. Don't use tabs for the
  top-level structure. Basic responsive styling so you can view it on a phone is nice too.
  Save the file in `~/ai-artifacts/` (create the directory if it doesn't exist) so it
  persists outside the code repo and out of version control, and make sure the filename always
  starts with today's date in `YYYY-MM-DD-` format, because it helps keep the files
  time-sorted. For example: ~/ai-artifacts/2026-01-12-explanation-<slug>.html Do not open the file
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
- Please write with the clarity and flow of Martin Kleppmann, making it engaging and written
  in classic style. Transitions between sections should be smooth.
- Some tips on diagrams. Ideally, you should pick a small number of diagram families that can
  be reused throughout the explanation to explain various cases. Some useful kinds of diagrams:
  - A very simplified version of any UI the reader sees, to explain user-facing behavior.
  - A system diagram showing data flow or communication between components. Make sure to
    include example data here!
  - For algorithms/data structures, a state-over-time diagram showing the structure evolving
    step by step on a toy input.
- Don't use ASCII diagrams. Always use simple HTML designs for your diagrams, HTML lists for
  lists of things, etc.
  - For code blocks, always use `<pre>` tags. If you use a custom styled div instead, it
    **must** have `white-space: pre-wrap` in its CSS, or the browser will collapse all
    newlines into a single line. Before saving the file, scan each code block in the HTML
    source and confirm its CSS includes `white-space: pre` or `pre-wrap`.
- Use callouts for key concepts or definitions, important edge cases, etc.
