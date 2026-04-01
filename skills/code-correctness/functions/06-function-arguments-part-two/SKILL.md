---
name: code-correctness-functions-06-function-arguments-part-two
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Function Arguments Part Two." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 06: Function Arguments Part Two

## Source
- Post folder: `post_2026-01-21_09-58-55`
- Post date: `2026-01-21 09:58:55`
- Theme folder: `functions`
- Lesson folder: `06-function-arguments-part-two`

## Description
Asking, Transforming and Handling events are the main reasons a function should take a single argument.

#programming #coding #code #cleancode

## Hashtags
#programming, #coding, #code, #cleancode

## Transcript
Let's interrogate this function. What does this boolean mean? True for what? False for what? Looking at this call gives us nothing. We have to dig through the implementation to understand what's happening. Now, booleans aren't always the enemy. Passing true to setVisible, that's data. The true is the actual value you need. But when it controls which code path runs, that's a flag. And flags are a problem because they force one function to do two completely different jobs. But booleans aren't our only suspect. Output arguments break expectations too. Output arguments modify their inputs instead of returning results. But we expect data to flow in through arguments and out through return values. So what does a clean argument look like? Single arguments serve 3 clear purposes. A function might be asking a question. You pass something in and it gives you an answer back. Or a function might be transforming something. You pass in an input and it returns something new. Or a function might be handling an event. Something just happened in your system and the function needs to react to it. When your arguments follow these patterns, your code has nothing to hide. Follow for more clean code principles coming next.
