---
name: code-correctness-comments-05-redundance-and-noise
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about comments and the lesson "Redundance and Noise." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Comments

## Lesson 05: Redundance and Noise

## Source
- Post folder: `post_2026-02-22_15-12-30`
- Post date: `2026-02-22 15:12:30`
- Theme folder: `comments`
- Lesson folder: `05-redundance-and-noise`

## Description
If the code already says it, the comment is noise.

#cleancode #code #programming #developers

## Hashtags
#cleancode, #code, #programming, #developers

## Transcript
If there are no items in the cart, return early. The comment communicated nothing the code hadn't already. That's a redundant comment, a comment that restates what the code already says. It adds words, not understanding. Similarly, noise comments offer nothing of value. They restate the obvious, offer no new information, and exist as labels over your code that repeat what the names already tell you. And sometimes this clutter is forced on you. Some teams have a rule that that every function must have a doc comment. So developers write comments that exist only to satisfy that rule. This clutter adds nothing and serves only to obfuscate the code and create the potential for lies and misdirection. Now, here are the actual consequences. First, it creates clutter. You double the lines you have to scan without adding any understanding. Second, it's maintenance. Every time you change the code, you have to update a useless comment or risk it becoming a lie. And third, it creates blindness. When comments say nothing new, developers learn to skip all of them. So when you finally write a comment that actually matters, nobody reads it. So the solution is simple. Respect your reader's time. They can read the code. Only interrupt them with a comment when you have something to say that the code can't say for you. Follow for more clean code principles coming next.
