---
name: code-correctness-comments-10-the-refactoring-principle
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about comments and the lesson "The Refactoring Principle." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Comments

## Lesson 10: The Refactoring Principle

## Source
- Post folder: `post_2026-03-08_13-18-12`
- Post date: `2026-03-08 13:18:12`
- Theme folder: `comments`
- Lesson folder: `10-the-refactoring-principle`

## Description
Stop commenting what your code does. Rename it, extract it, and let it explain itself.

#cleancode #code #programming #developer

## Hashtags
#cleancode, #code, #programming, #developer

## Transcript
Here's the problem with comments. Every comment you write is a second thing to maintain. Your code changes, and if you forget to update the comment, it becomes misinformation. The better approach: write code that doesn't need explaining. Take this condition, for example. It's complicated, so a comment seems necessary. But instead of describing the logic, let's name it, extract variables that explain each piece. Now the logic reads like plain English. No comment needed. But we can go Further, this entire block of logic is answering one question. Instead of dropping a comment above it, extract it into a function. Now the code reads like a sentence. This applies to variables too. This name tells you nothing, leaving the comment to fill the gap. A well-chosen name eliminates that gap entirely. And then there are magic numbers. A raw number in your code rarely explains its purpose. So naturally you add a comment. But a well-named constant does a better job. It ties the meaning directly to the value permanently. So the next time you're tempted to write a comment, take a pause and see if you can refactor instead. Clean code should always speak for itself. Follow for more clean code principles coming next.
