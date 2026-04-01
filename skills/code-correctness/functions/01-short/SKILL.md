---
name: code-correctness-functions-01-short
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Short." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 01: Short

## Source
- Post folder: `post_2025-12-28_11-33-17`
- Post date: `2025-12-28 11:33:17`
- Theme folder: `functions`
- Lesson folder: `01-short`

## Description
Massive functions bury business logic under low-level details. Extracting them makes your code document itself.

#cleancode #code #programming #programming

## Hashtags
#cleancode, #code, #programming, #programming

## Transcript
Reading functions shouldn't feel like a mental workout. The problem is, long functions bury your logic under layers of noise. The more a function tries to do, the harder it is to see what's actually happening. That's why the first rule of functions is that they should be small. The second rule? They should be smaller than that. Look at this example. It uploads a file. If it's too large, it splits into chunks and uploads piece by piece. Otherwise, it uploads directly. Let's make it small enough that you don't need me to explain it. First, let's wrap these low-level details behind clear function names. Next, see this loop? It's the heaviest part. Let's move it out, and the complexity moves with it. But that's not all. Blocks inside if, else, while, and so on should be one line long. In this case, through a function call. This achieves two things: your function stays short, and the extracted code gets a name that speaks for itself. Now, your reader understands what the code does without getting bogged down in how it does it. And the function no longer demands effort; it simply tells a story. Follow for more clean code principles coming next.
