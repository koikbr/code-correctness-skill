---
name: code-correctness-meaningful-names-02-avoid-disinformation
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about meaningful names and the lesson "Avoid Disinformation." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Meaningful Names

## Lesson 02: Avoid Disinformation

## Source
- Post folder: `post_2025-12-08_17-08-13`
- Post date: `2025-12-08 17:08:13`
- Theme folder: `meaningful-names`
- Lesson folder: `02-avoid-disinformation`

## Description
Trust in code starts with honest naming. If a name promises something it doesn't deliver—like calling a Map a List—you are creating disinformation. Misleading names break developer trust, force extensive tracing, and are a massive source of subtle bugs.

Which of these naming traps have you fallen into before? Let us know! 👇
#cleancode #softwareengineering #codingbestpractices #developer #programming #refactoring #codequality

## Hashtags
#cleancode, #softwareengineering, #codingbestpractices, #developer, #programming, #refactoring, #codequality

## Transcript
This code is lying to you. The word "list" means something specific to programmers. We expect arrays with indexes, yet this is actually a Map. Clean code demands names that don't mislead. So let's fix it using proven patterns, making our focus for today: avoiding disinformation. A variable name should never contain disinformation, nor should it promise something it doesn't deliver. Trust in code starts with honest naming. No surprises, no confusion. Let me show you Here are two more examples. First, watch out for names varying in small ways. Look at these two classes. Can you spot the difference instantly? You will autocomplete the wrong one half the time, guaranteed. Similarly, another source of disinformation comes from using characters that look identical. Characters like lowercase L and the number 1, or uppercase O and 0, are visually indistinguishable. This creates disinformation because the actual code may differ from what developers perceive, making the code lie. If it's not a list, don't call it a list. If names are similar, make them distinct. If characters look identical, use better names. That's it.
