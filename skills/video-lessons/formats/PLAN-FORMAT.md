# PLAN.md Format

`PLAN.md` lives in each `videos/NNNN-<slug>/` folder. It is the lesson plan for that video (or batch of videos). It is a living document: `next` ticks lessons off, inserts repair lessons, and records skips.

## Template

```md
# Plan: {Video title, or batch name}

**Source**: [{title}](https://youtu.be/{id}) by {channel}, {duration}
**Mission link**: {one sentence: why this video matters for MISSION.md}

## Lessons

1. [x] **{Concept}**: {one-line win} ([lesson](lessons/01-{slug}.html)) · [0:37–2:34](https://youtu.be/{id}?t=37)
2. [ ] **{Concept}**: {one-line win} · [2:34–4:06](https://youtu.be/{id}?t=154) 🎬
3. [ ] **{Concept} (1/2)**: {one-line win} · [4:06–9:10](https://youtu.be/{id}?t=246)
4. [ ] **{Concept} (2/2)**: {one-line win} · [9:10–11:21](https://youtu.be/{id}?t=550)
5. [ ] *(optional, off-mission)* **{Concept}**: {one-line win} · [11:21–12:12](https://youtu.be/{id}?t=681)

## Skipped

- 0:00–0:37 intro
- 13:59–15:12 course announcement (promo)
- {Concept} at 6:45: already known (see [LR-0003](../../learning-records/0003-{slug}.md))

## Changes

- {date}: inserted lesson 3a to repair {misconception} after recall check
```

## Rules

- **One concept per lesson, ~10 minutes to read and quiz.** Video minutes don't set lesson size; concepts do.
- **Ranges can be non-contiguous** in a multi-video batch: `[A 2:10–4:00](...) + [B 0:30–3:15](...)`.
- **🎬** marks a lesson where the speaker relies on something only visible on screen. The lesson must rebuild it or flag it.
- **Everything in the video is either a lesson or a Skipped line.** Nothing silently disappears.
- **Don't rewrite history.** When the plan changes, edit the list and add a dated line under `## Changes`.
