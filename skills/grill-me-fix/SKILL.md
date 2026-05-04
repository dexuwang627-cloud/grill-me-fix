---
name: grill-me-fix
description: Translate vague visual feelings or screenshots into precise code changes. The AI locates the code, translates your description into technical root causes you can pick from, then modifies only after confirmation. Use /grill-me-fix when you want to change something but can't pinpoint the exact file, element, or CSS property.
category: workflow
---

# /grill-me-fix — Vague Feeling → Precise Fix

You have a vague feeling or a screenshot, and you want to change something but can't describe it in precise technical terms. This workflow translates for you.

## Flow

### Phase 1: Receive
The user provides one of:
- Natural language description (e.g. "the container is too small and clips the content", "this button is in a weird spot")
- A screenshot
- Both

**Do NOT jump to editing code.** Proceed to Phase 2 first.

### Phase 2: Locate (AI does this)
Search the codebase to find the relevant files and components:
- Use grep/glob to find related component, page, and style files
- If the user provided a screenshot, read it and analyze the visual content
- Narrow down to the 1-3 most likely files and specific line numbers

Output format:
> **Located**: `<file_path>:<line_number>` — `<component_name>`

### Phase 3: Translate (AI offers options, user picks)
Translate the user's vague description into 2-3 possible technical root causes, explained **in plain language**:

> **Your description**: "the container is too small and clips the content"
>
> This could be caused by:
>
> **A)** `max-width` is too small — the container width is being limited, so content that doesn't fit gets clipped
> **B)** `overflow: hidden` — the container is actually big enough, but overflowing content is being cut off
> **C)** `width: fixed` hardcoded — the width is set to a fixed number and won't adjust to the content

Each option must include:
1. **Property name** (so the user gradually learns technical vocabulary)
2. **Plain explanation** (why this causes the feeling they described)
3. **Fix direction** (e.g. "widen max-width" or "remove overflow: hidden")

**Wait for the user to choose before proceeding to Phase 4.**

### Phase 4: Confirm scope
After the user picks a root cause, list the specific change:

> **Change plan**:
> - File: `src/components/Card.tsx` line 42
> - Change: `max-width: 320px` → `max-width: 480px`
> - Impact: Cards on homepage and list page will become wider
> - Risk: Low (single value change)

**Wait for user confirmation before making changes.**

### Phase 5: Fix + Verify
- Make the code change
- For frontend changes, remind the user to check the result visually
- Briefly explain what was changed and why (helping the user build their "effect → property" vocabulary over time)

## Rules

1. **Always translate before modifying** — never skip Phase 3 and jump to code changes
2. **Options in plain language** — every technical property must come with a plain explanation
3. **One problem at a time** — the user may describe multiple feelings; handle them one by one
4. **Teach as you go** — after each fix, explain what property was changed and why, helping the user build their "effect → property" mental map
5. **Screenshots first** — if the user provides a screenshot, treat it as the primary source; natural language is supplementary
6. **When in doubt, ask** — if you can't locate the file or there are multiple possibilities, ask the user directly; don't guess