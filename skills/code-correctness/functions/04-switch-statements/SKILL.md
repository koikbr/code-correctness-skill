---
name: code-correctness-functions-04-switch-statements
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Switch Statements." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 04: Switch Statements

## Source
- Post folder: `post_2026-01-11_10-21-45`
- Post date: `2026-01-11 10:21:45`
- Theme folder: `functions`
- Lesson folder: `04-switch-statements`

## Description
Keep your high-level logic clean: bury the switch.

#cleancode #programmer #code #developers

## Hashtags
#cleancode, #programmer, #code, #developers

## Transcript
By their very nature, switch statements do N things. That's a problem because clean functions should do one thing. Here, we're switching on employee type to calculate pay. But we also have to switch on type for benefits, for scheduling, for reporting, and so on. The same switch copied everywhere. Now imagine adding a new employee type. You'd have to hunt through every function, find every switch, and add a new case to each one. The solution is to use polymorphism. You Create an interface that each employee type implements. The switch gets buried in a factory. It creates the object, nothing more. After creation, switches disappear. You call the function and polymorphism handles the dispatch. The Fulltime class runs its logic, the Contractor class runs its own. Each object already knows what to do. This keeps your functions clean. One call, one responsibility. The way functions should be. Follow for more clean code principles coming next.
