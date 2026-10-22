# Human Touch

A Claude skill that rewrites AI-sounding prose so it reads like a person wrote it, without changing what it says.

## Why use it

LLM drafts fall into the same habits: throat-clearing openers, "not X, it's Y" contrasts, em dashes, forced groups of three, inflated significance, bold labels on every bullet, and leftover "I hope this helps!" lines. Readers spot these fast.

Human Touch runs two cleanup passes in one call:

1. **Style pass (strict).** Cuts filler, adverbs, jargon, binary contrasts, passive voice, and em dashes, and fixes rhythm. Based on [stop-slop](https://github.com/hardikpandya/stop-slop).
2. **Pattern pass (careful).** Checks the result against 26 patterns from Wikipedia's "Signs of AI writing," confirms no facts were added or lost, and restores the writer's voice where the first pass cut too hard. Based on [humanizer](https://github.com/blader/humanizer).

The skill finishes by scoring the text out of 50 and revising again if it scores below 35.

Use it for emails, cover letters, blog posts, papers, reports, PR descriptions, and anything else you plan to put your name on.

## What's inside

```
human-touch/
├── SKILL.md              # Workflow, voice rules, scoring, output modes
├── references/
│   ├── phrases.md        # Pass 1: phrases to cut
│   ├── structures.md     # Pass 1: structural patterns to avoid
│   ├── examples.md       # Pass 1: before/after rewrites
│   └── patterns.md       # Pass 2: 26-pattern catalog with examples
├── CHANGELOG.md
├── README.md
└── LICENSE
```

## Install

**Claude Code (personal skill):**

```bash
git clone https://github.com/nazmulislam-lab/human-touch ~/.claude/skills/human-touch
```

On Windows, clone into `%USERPROFILE%\.claude\skills\human-touch`. For a single project, clone into `.claude/skills/human-touch` inside that project instead.

**Claude.ai Projects:** Upload `SKILL.md` and the files in `references/` to project knowledge.

**API:** Put `SKILL.md` in your system prompt and load the reference files when you need them.

## Usage

Claude picks the skill up when you draft or edit prose. You can also call it by name:

> Use human-touch on this paragraph.

> Run human-touch on README.md.

> Here's a sample of my writing. Use human-touch on my draft and match my voice.

**What you get back**

- **Pasted text:** the rewrite, a list of the patterns it fixed, and the score.
- **A file:** the file rewritten in place. Code, paths, links, and YAML stay untouched.
- **Another task's output** (such as a PR description): just the final text.

## Scoring

| Dimension | Question |
|-----------|----------|
| Directness | Does it state things or announce them? |
| Rhythm | Do sentence lengths vary? |
| Trust | Does it respect the reader's intelligence? |
| Authenticity | Does it sound like a person? |
| Density | Can anything be cut? |

Each is scored 1 to 10. Below 35 out of 50 means another pass.

## Credits

Built by [Md Nazmul Islam](https://github.com/nazmulislam-lab) by combining two MIT-licensed skills:

- [stop-slop](https://github.com/hardikpandya/stop-slop) by [Hardik Pandya](https://hvpandya.com): style rules, phrase and structure lists, scoring.
- [humanizer](https://github.com/blader/humanizer) by Siqi Chen: the 26-pattern catalog, fact-preservation checks, voice matching, and output modes. Its patterns draw on Wikipedia's ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing).

## License

MIT. See `LICENSE`, which keeps both original copyright notices as the license requires.
