---
name: code-correctness-functions-11-dont-repeat-yourself
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Don't Repeat Yourself." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 11: Don't Repeat Yourself

## Source
- Post folder: `post_2026-02-08_12-32-57`
- Post date: `2026-02-08 12:32:57`
- Theme folder: `functions`
- Lesson folder: `11-dont-repeat-yourself`

## Description
DRY: Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

#cleancode #code #programming #developer

## Hashtags
#cleancode, #code, #programming, #developer

## Transcript
Duplication may be the root of all evil in software. Think about it. We normalize databases to eliminate redundant data. We use inheritance to concentrate shared logic in base classes. We build components so we can reuse them everywhere. Every innovation since the subroutine has fought the same battle. Write once, not twice. Let me show you why. Check out these two API functions. They look solid, right? Now imagine adding a timeout to 20 of them. Updating them all is hectic, but even worse? If you miss just one, you're left with a silent bug waiting to crash your app. Here's the solution: instead of repeating yourself, extract it. One function handles fetching, error checking, and yes, the timeout. Now getUsers and getPosts just call it with their endpoint. Need to change the timeout? One place, done. This is the DRY principle: Don't Repeat Yourself. Or more precisely, every piece of knowledge must have a single, unambiguous, authoritative representation within a system. One truth, one place. Follow for more clean code principles coming next.
