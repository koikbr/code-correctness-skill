---
name: code-correctness-comments-08-visual-clutter-and-formatting
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about comments and the lesson "Visual Clutter and Formatting." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Comments

## Lesson 08: Visual Clutter and Formatting

## Source
- Post folder: `post_2026-03-04_14-42-45`
- Post date: `2026-03-04 14:42:45`
- Theme folder: `comments`
- Lesson folder: `08-visual-clutter-and-formatting`

## Description
When comments apologize for structure, the structure needs to change.

## Hashtags
None

## Transcript
Some comments are apologies for poor code structure. They attempt to compensate for code that has become too large, too complex, or lacks proper modularity. For example, closing brace comments appear when a function gets so long or deeply nested that you can no longer see where a block ends. Sure, they help you track which brace closes which block. But these comments are symptoms of the problem, not the solution. The solution is to write clean functions that do one thing. Do that, and you'll never need a comment to tell you where a block ends. The same applies for position markers. These separators try to organize a file that's doing too much. Authentication here, database queries there, file handling below. But drawing lines between sections doesn't reduce complexity. It just makes the mess look tidy. Now, there are rare cases where a position marker might be acceptable. For example, if a framework requires boilerplate that naturally splits into distinct groups, a marker can help. But only when the benefit is clear. Overuse them, and they become the very noise you're trying to fix. So remember, if your comments are apologizing for the code, the code needs to change, not the comments. Clean structure speaks for itself. Follow for more clean code principles coming next.
