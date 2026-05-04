# grill-me-fix

A [Claude Code](https://claude.ai/code) skill that translates vague visual feelings into precise code changes.

Inspired by [mattpocock/skills/grill-me](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me) — the interactive grilling skill for validating plans. `grill-me-fix` extends that philosophy to a different problem: **describing what you want to change when you can't name the technical details.**

## The Problem

You see something wrong on screen — a container clips its content, a button is misaligned, the spacing feels off. You know what it *should* look like, but you can't pinpoint which file, which component, or which CSS property to change. So you write something like:

> "the container is too small and clips the content"

Sometimes the AI gets it right on the first try. Sometimes it takes 5-6 rounds of fixes — changing the wrong property, guessing at numbers, or misunderstanding what you meant.

## The Solution

`grill-me-fix` adds a **translation layer** between your vague description and the code change:

1. **You describe the feeling** — natural language, screenshot, or both
2. **AI locates the code** — searches the codebase for the relevant file and component
3. **AI translates into options** — turns "container too small" into 2-3 possible technical causes (e.g. `max-width`, `overflow: hidden`, `fixed width`) with plain-language explanations
4. **You pick the right one** — no need to know CSS property names
5. **AI confirms scope, then fixes** — shows you exactly what will change before touching code

Each fix also teaches you the property name and why it caused the problem — so over time, you build your own "effect → property" mental map.

## Install

Copy the skill into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/grill-me-fix
curl -o ~/.claude/skills/grill-me-fix/SKILL.md \
  https://raw.githubusercontent.com/dexuwang627-cloud/grill-me-fix/main/skills/grill-me-fix/SKILL.md
```

Or manually create `~/.claude/skills/grill-me-fix/SKILL.md` with the content from this repo.

## Usage

In Claude Code, type:

```
/grill-me-fix
```

Then describe what feels wrong, optionally with a screenshot.

### Example

```
You: /grill-me-fix
     the cards on the homepage feel too cramped

AI:   Located: src/components/Card.tsx:42 — <Card>

      Your description: "cards feel too cramped"
      This could be caused by:

      A) padding is too small — not enough breathing room inside the card
      B) gap between cards is too narrow — cards are squeezed together
      C) max-width on the container is limiting card width

      Which sounds closest?

You:  A

AI:   Change plan:
      - File: src/components/Card.tsx line 42
      - Change: padding: 12px → padding: 24px
      - Impact: All cards on homepage and list page get more spacing
      - Risk: Low (single value change)

      Confirm?

You:  yes

AI:   [makes the change]
      Changed padding from 12px to 24px. Padding controls the space
      between the card border and its content — too little padding
      makes content feel squeezed against the edges.
```

## How It Differs

| | grill-me | /bug | **grill-me-fix** |
|---|---|---|---|
| Starting point | You have a plan | You have a bug | You have a vague feeling |
| What AI does | Challenges the plan | Finds root cause | Translates feeling → technical description |
| What you provide | Plan details | Bug symptoms | Just a feeling or screenshot |
| Output | A better plan | Confirmed fix | Precise change instruction |

## Acknowledgments

- [grill-me](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me) by Matt Pocock — the interactive grilling philosophy that inspired this skill

## License

MIT