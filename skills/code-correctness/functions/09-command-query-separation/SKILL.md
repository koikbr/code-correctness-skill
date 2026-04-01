---
name: code-correctness-functions-09-command-query-separation
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Command Query Separation." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 09: Command Query Separation

## Source
- Post folder: `post_2026-02-02_11-59-52`
- Post date: `2026-02-02 11:59:52`
- Theme folder: `functions`
- Lesson folder: `09-command-query-separation`

## Description
Your function should either Do something or Answer something, but never both.

#cleancode #code #programming #developers

## Hashtags
#cleancode, #code, #programming, #developers

## Transcript
Look at this condition. We know it sets the attribute value, but what is it actually checking? Is it checking if the update was successful? Or if the attribute already existed? You can't really tell without digging into the implementation. And that's a problem. You might try renaming the function to fix it, but suddenly the name reveals the real issue. This function is doing two jobs. It's commanding an action, setting the attribute. And it's querying the status, checking if it existed. That's a mess. A major code smell. Because it mixes specific actions with questions, you can't safely use it. Calling it just to check something accidentally changes your data. The fix? Split them up. AttributeExists answers the question. SetAttribute performs the action. Now read the code, it flows like a conversation. Does it exist? No? Then set it. Commands change state. Queries return answers. Keep them separate. Follow for more clean code principles coming next.
