---
name: code-correctness-functions-08-side-effects
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Side Effects." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 08: Side Effects

## Source
- Post folder: `post_2026-01-30_12-33-58`
- Post date: `2026-01-30 12:33:58`
- Theme folder: `functions`
- Lesson folder: `08-side-effects`

## Description
A side effect occurs when a function modifies state outside its own scope or interacts with the outside world

#cleancode #programming #code

## Hashtags
#cleancode, #programming, #code

## Transcript
On the surface, this makes complete sense. If the password matches, starting the session feels like the obvious thing to do. But the function's name tells a different story. It promises to check, nothing more. And when code does more than its name promises, that's when bugs find their way in. What if your user is mid-session with items in their cart, and they decide to change their password? You confirm the current password by calling this function. It works. But it also re-initializes the session. Suddenly their cart is empty, their preferences reset. You just wanted to check something. Instead, you changed everything. Side effects can be even more subtle. Here, the function sets a property on the class. That's still a hidden change. So how do we fix this? One option is to rename it. Now the name is honest. But it's also ugly, and it still does two things. The better approach? Move the side effect out. Let the function do one thing— check the password. Then handle the session separately. Clear name, clear responsibility. Side effects make functions lie. And lies become bugs. Keep it honest. Follow for more clean code principles coming next.
