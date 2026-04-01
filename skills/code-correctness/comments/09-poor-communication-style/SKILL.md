---
name: code-correctness-comments-09-poor-communication-style
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about comments and the lesson "Poor Communication Style." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Comments

## Lesson 09: Poor Communication Style

## Source
- Post folder: `post_2026-03-06_12-48-49`
- Post date: `2026-03-06 12:48:49`
- Theme folder: `comments`
- Lesson folder: `09-poor-communication-style`

## Description
Comments should communicate, not confuse. If your team can't read it instantly, rewrite it.

#cleancode #programming #code #development

## Hashtags
#cleancode, #programming, #code, #development

## Transcript
Some comments technically exist but completely fail at communicating. They mumble, dump irrelevant details, or become unreadable in your editor. For example, look at this comment. Who is RJ? Which edge case? And what does "not sure if it works" even mean? If you're unsure, don't mumble about it. Write a clear to-do that explains the actual constraint. Now the next developer knows exactly what's wrong and what to look for. But sometimes the problem isn't It's saying way too much. This comment is basically a novel for a one-line function. Dave from 2018, an RFC number, a library bug from a dead dependency. This comment is a history lesson, and none of that helps you today. Just delete it. The code already tells you what it does without any need for a comment. And finally, there's the issue of formatting. Specifically using HTML tags. When rendered on a documentation site, these comments look beautiful. But in your editor, your eyes filter through tags just to read English. And if you need to change a word, you're editing around tags instead of text. The fix is simple: use plain text, which is readable everywhere. A good comment is clear, relevant, and readable Exactly where you write code. Anything else is noise. Follow for more clean code principles coming next.
