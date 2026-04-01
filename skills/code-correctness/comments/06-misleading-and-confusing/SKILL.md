---
name: code-correctness-comments-06-misleading-and-confusing
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about comments and the lesson "Misleading and Confusing." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Comments

## Lesson 06: Misleading and Confusing

## Source
- Post folder: `post_2026-02-25_11-58-23`
- Post date: `2026-02-25 11:58:23`
- Theme folder: `comments`
- Lesson folder: `06-misleading-and-confusing`

## Description
Comments should clarify, not confuse.

#cleancode #programming #code #developer

## Hashtags
#cleancode, #programming, #code, #developer

## Transcript
A comment doesn't have to be a lie to be dangerous. Sometimes, the worst comments are the ones that are technically true, but practically useless. Here is exactly what I mean. Take this function. The comment tells us it returns the discount for premium users. And sure enough, premium users get 20%. But if you actually read the implementation, non-premium users get a discount too. The comment gives you false confidence to stop reading. And a misleading comment is worse than no comment at all. Then there are comments that raise more questions than answers. Take this calculation. It says allow room for the header and padding. Okay, but which part is the header? Is it the times 4? The plus 12? Both? The whole purpose of a comment is to explain code that doesn't explain itself. If this code needs the help of a comment, but the comment needs its own comment to be understood, That's a pity. Finally, never write a comment about code you don't control. Look at this setter. The comment claims a default of 5,000 milliseconds. But this function doesn't decide that default. The default is decided somewhere else entirely. When someone changes that default in the other file, they'll never find this comment. If you write a comment, keep it about the code right next to it. So the rules are simple: be specific, be obvious, and stay local. If a comment forces the reader to guess or go hunting, it's not helping, it's hurting. Follow for more clean code principles coming next.
