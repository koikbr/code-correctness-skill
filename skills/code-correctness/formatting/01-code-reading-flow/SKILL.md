---
name: code-correctness-formatting-01-code-reading-flow
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about formatting and the lesson "Code Reading Flow." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Formatting

## Lesson 01: Code Reading Flow

## Source
- Post folder: `post_2026-03-10_13-49-31`
- Post date: `2026-03-10 13:49:31`
- Theme folder: `formatting`
- Lesson folder: `01-code-reading-flow`

## Description
Highest-level function at the top, every definition below its caller, and related functions grouped by purpose.

## Hashtags
None

## Transcript
Your source file should read like a newspaper. The headline tells you what happened, and in your code, that headline is your highest-level function. Place it at the top of your file. In 3 lines, any developer knows what this code does. Fetch data, build a report, save it. That's your headline. And just like a newspaper, the next detail should sit right below where it's mentioned. Place every function just below its caller, so the code reads in a natural downward flow, from high high-level concepts down to low-level details. Look at how fetch user data, build report, and save report are defined in the same order they were called. This builds trust with every reader. When they see a function call, they know its definition is coming right below. But there's another reason to keep functions close. Not because one calls the other, but because they serve a similar purpose. Look at format_date and format_score. They both format values and share a naming pattern. Clean code calls this conceptual affinity. Group them together and your file naturally organizes into layers. The big picture at the top, supporting functions flowing downward in call order, and shared utilities grouped by purpose at the bottom. That's how your code reads like a newspaper. Follow for more clean code principles coming next.
