---
name: code-correctness-objects-and-data-01-data-abstraction
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about objects and data and the lesson "Data Abstraction." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Objects and Data

## Lesson 01: Data Abstraction

## Source
- Post folder: `post_2026-03-23_15-22-14`
- Post date: `2026-03-23 15:22:14`
- Theme folder: `objects-and-data`
- Lesson folder: `01-data-abstraction`

## Description
Instead of exposing data, expose behavior.

#code #cleancode #programming #developer

## Hashtags
#code, #cleancode, #programming, #developer

## Transcript
Most developers make their variables private, then immediately add getters and setters for every one of them, which effectively makes them public again. The whole reason to keep variables private is the freedom to change how they're stored without breaking dependence. That freedom disappears the moment you expose the shape of the data. Hiding implementation isn't about adding a layer of functions, it's about abstraction. This class stores a point as x and y, rectangular coordinates. This one stores the same point as radius and theta, polar coordinates. Same point on the same plane, two completely different representations. But both classes expose the implementation, not the functionality. Abstract the implementation away, and you can still get x and y, or radius and theta. Set Cartesian or polar. The caller manipulates the data without ever knowing how it's stored. And it does one more thing. The interface can set an access policy. You can read coordinates independently, but you must set them together as an atomic operation. Public fields have no way to enforce that. Abstraction is not just about hiding fields. It's about exposing behavior rather than structure. Take this interface that a vehicle uses. The fields are hidden, but the methods still describe just the fields. On the other hand, this interface only exposes behavior: percent fuel remaining. We have no clue about the form of the data. Is it in gallons? Liters? Is it diesel? Electric? None of that matters. The implementation is genuinely gone. Serious thought needs to go into how you represent an object's data. The worst thing you can do is blindly add getters and setters. Expose what users can do with the data. Not how the data is stored. Follow for more clean code principles coming next.
