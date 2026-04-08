---
name: code-correctness-objects-and-data-03-law-of-demeter
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about objects and data and the lesson "The Law of Demeter." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Objects and Data

## Lesson 03: The Law of Demeter

## Source
- Post folder: `post_2026-04-05_15-30-47`
- Post date: `2026-04-05 15:30:47`
- Theme folder: `objects-and-data`
- Lesson folder: `03-law-of-demeter`

## Description
Stop chaining through strangers.

## Hashtags
#cleancode, #programming, #code, #programming

## Transcript
The Law of Demeter has one rule: talk to friends, not to strangers. A method should only call methods on the object it belongs to, objects it creates, objects passed as arguments, and objects it holds as fields. Never call methods on objects that those calls return. This line does exactly that. The function reaches through context to options, through options to Scratch directory, through the directory to an absolute path. 3 objects deep. That's a lot of structural knowledge for one line of code. But whether this violates Demeter depends on whether these are objects or data structures. When it's just data, there's no violation. But add getters, and the same access looks like a Demeter violation. If they're objects hiding behavior, it is one. If they're data structures, Demeter doesn't apply. Data is meant to expose its internals. Getters on data structures create false ambiguity. So if these are real objects, you shouldn't chain through them. What if we just collapse the chain into one method? The name encodes every object from that chain. Now every different combination needs its own method. The structural knowledge didn't disappear, it just moved. The real solution is to ask why you need that path. Here, to create a scratch file. So tell the object what you need. The chain collapses to a single call, and your method knows nothing about the structure behind it. That's the Law of Demeter. Follow for more clean code principles coming next.
