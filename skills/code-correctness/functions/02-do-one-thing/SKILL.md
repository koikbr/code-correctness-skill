---
name: code-correctness-functions-02-do-one-thing
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Do One Thing." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 02: Do One Thing

## Source
- Post folder: `post_2026-01-03_14-39-08`
- Post date: `2026-01-03 14:39:08`
- Theme folder: `functions`
- Lesson folder: `02-do-one-thing`

## Description
Every function should be transparently obvious and tell a story that leads the reader naturally to the next step. If your function is divided into sections like "initialization" or "logic," it’s an obvious symptom of doing more than one thing. 

#cleancode #programming #code #developers

## Hashtags
#cleancode, #programming, #code, #developers

## Transcript
A function doing multiple things has multiple reasons to change, multiple reasons to break, multiple types of tests to write, and multiple ways to confuse the next developer who reads it. But a function that does one thing? It has one reason to change, one focused test, one clear purpose. It becomes a building block you can trust. So how do we know if a function is doing one thing? Let me show you two ways to find out. First, a function doing one thing cannot be divided into sections. If you can label chunks of your code with different names, each chunk is a separate responsibility. Second, a function is doing more than one thing if you can extract another function from it with a name that is not merely a restatement of its implementation. Take this function. It decides whether to upload a file in multiple parts or as a single upload based on size. But look here, these lines handle the low-level details of multi-part uploading. That's a separate concept we can extract and name meaningfully. Now the function reads more clearly. It makes a decision and delegates the details. But can we extract further? If we pull out the remaining logic, What would we call it? Upload based on size? That's just restating what the code already does. The extraction test fails. This function is now doing exactly one thing. Follow for more clean code principles coming next.
