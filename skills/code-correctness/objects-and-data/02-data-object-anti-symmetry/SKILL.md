---
name: code-correctness-objects-and-data-02-data-object-anti-symmetry
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about objects and data and the lesson "Data / Object Anti-Symmetry." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Objects and Data

## Lesson 02: Data / Object Anti-Symmetry

## Source
- Post folder: `post_2026-04-01_13-15-33`
- Post date: `2026-04-01 13:15:33`
- Theme folder: `objects-and-data`
- Lesson folder: `02-data-object-anti-symmetry`

## Description
Everything doesn't need to be an object. Before you write your next class, ask yourself, are you more likely to add new data types, or new behaviors?

## Hashtags
None

## Transcript
Every class you write is a bet. A bet on what kind of change comes next. Consider 3 shapes: Square, Rectangle, and Circle. They're just data, no behavior at all. All the behavior lives in a separate Geometry class that takes any shape and calculates its area. So what happens when we need a perimeter function? We add it to Geometry, and that's it. The shapes don't change, and nothing that depends on them changes either. But what happens when we need to add new data types? But what happens when we add a new shape? Now every function in Geometry needs a new case. Area, perimeter, all of them. That's the cost of keeping behavior separate from data. So what if we took the opposite approach? Same shapes, but this time each one owns its behavior. There's no Geometry class needed. Now adding a new shape is simple. One new class, nothing else changes. But adding a perimeter function means every Shape class needs a new method. All of them have to change. That's the exact inverse of the problem we had before. Procedural code makes it easy to add functions, while object-oriented code makes it easy to add types. What's easy for one is hard for the other. So everything is not an object. Choose data structures with procedures when you expect to add new operations. Because none of your existing types need to change. Choose objects when you expect to add new types, because none of your existing functions need to change. Know which kind of change is coming, and the design follows. Follow for more clean code principles coming next.
