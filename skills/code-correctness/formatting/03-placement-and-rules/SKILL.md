---
name: code-correctness-formatting-03-placement-and-rules
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about formatting and the lesson "Placement and Rules." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Formatting

## Lesson 03: Placement and Rules

## Source
- Post folder: `post_2026-03-17_13-56-44`
- Post date: `2026-03-17 13:56:44`
- Theme folder: `formatting`
- Lesson folder: `03-placement-and-rules`

## Description
Declare variables close to their usage. Group class properties in one place. Let the team decide the rest.

#cleancode #code #programming #development

## Hashtags
#cleancode, #code, #programming, #development

## Transcript
Every variable declared too early is baggage your brain carries until it's finally used. For example, these two variables are declared right at the top, but skipped_log is only used inside a nested block, and report isn't needed until the last few lines. When reading this code, we carry these variables in our mental stack throughout the function just to use them in two small places. So why not just move them there? #skippedLog moves inside its block, and #report moves right where it's used. Now you only encounter a variable when you need it. If this works for local variables, should it work for class properties too? For example, this validator is declared right above the function where it's used. Seems logical. But it's a class property for a reason, most likely because other functions rely on it too. And sure enough, export users uses the same validator right here. By design, class properties are shared. Placing them near one function only makes them harder to find for the rest. Instead, keep them in one designated place. Some languages place them at the bottom, but most prefer the top of the class. What matters is which standard the team decides to follow. And this applies to everything. Do your braces go on the same line or the next? Are these tabs or spaces? Does the team use single quotes or double? These are team decisions. The result is a codebase where formatting never gets in the way of understanding. Follow for more clean code principles coming next.
