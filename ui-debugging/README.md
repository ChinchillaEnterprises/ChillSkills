# UI Debugging Skill

**Clear visual communication protocol for UI/layout issues**

This skill establishes a standardized format for describing visual problems in screenshots, eliminating back-and-forth clarification questions.

## What This Skill Does

- ✅ Provides structured screenshot protocol
- ✅ Offers standardized layout issue template
- ✅ Includes visual inspection checklist
- ✅ Shows real-world examples
- ✅ References full-page screenshot methods

## When to Use

Use this skill when:
- Describing UI/layout bugs
- Sharing broken page screenshots
- Reporting visual issues in web apps
- Communicating responsive design problems
- Explaining missing or misplaced content

## Quick Start

1. **Take a full-page screenshot** (not just viewport if content extends below)
2. **Use the template** from SKILL.md → Mode 2
3. **Answer all sections**: What you see, what you expected, the problem
4. **Include context**: URL, platform, viewport size
5. **Share with Claude** - I'll understand immediately

## The Template

```
**URL:** [page location]
**Platform:** [web/mobile, browser, viewport]

**What I See:**
- [visible elements and colors]

**What I Expected:**
- [what should be there]

**The Problem:**
- [specific issue]

**Layout Layers (Top to Bottom):**
1. [section 1]
2. [section 2]
3. [cut-off content]
```

## Example

See SKILL.md → Mode 2 for a complete example (Predictor page layout issue).

## Files

- **SKILL.md** - Full skill documentation with all modes, templates, and examples

## How It Saves Time

| Without Protocol | With Protocol |
|---|---|
| "Page is broken" | "Black header visible, gray background below, white section cut off at bottom, crop buttons not rendering" |
| "Show me more details" | Understood immediately |
| "What exactly is missing?" | Ready to fix |
| (5+ clarification rounds) | (Direct solution) |

## Integration

Mention in your request:
```
"Use the ui-debugging skill to describe this..."
```

Or I'll automatically apply it when you share visual feedback.

---

**Made for:** Clear communication about UI/layout issues
**Works with:** Any project, any platform, any browser
**Maintained in:** ~/.claude/skills/ui-debugging/
