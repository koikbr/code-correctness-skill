---
name: code-correctness-functions-10-prefer-exceptions
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Prefer Exceptions." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 10: Prefer Exceptions

## Source
- Post folder: `post_2026-02-05_14-03-42`
- Post date: `2026-02-05 14:03:42`
- Theme folder: `functions`
- Lesson folder: `10-prefer-exceptions`

## Description
Avoid error codes, use try/catch instead.

#cleancode #code #programming #developer

## Hashtags
#cleancode, #code, #programming, #developer

## Transcript
This is how error codes start. One operation, one check. But let me show you where this leads. Add a profile, then an email, and suddenly you're buried in nested checks. That's problem one. But the caller of this function has their own challenge now. When a function returns an error code, the caller can't ignore it. They have to handle it right there, or the error disappears silently. So you end up with another chain of checks at every call site. And those error codes? They usually live in an enum that every file imports. Change it once, and you break half your codebase. So what's the alternative? Use try/catch instead. Your code now reads like a list of steps. No more nesting. No more forcing the caller to handle errors immediately. And no shared enum tying your codebase together. But we're not done yet. Try/catch blocks confuse the structure. Normal processing and error processing are sitting side by side. It's better to extract each into its own function. Now we have clean separation between logic and error handling. That's the principle: prefer exceptions to returning error codes. Your code stays flat, focused, and free of clutter. Follow for more clean code principles coming next.
