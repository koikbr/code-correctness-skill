---
name: code-correctness-functions-03-level-of-abstraction
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about functions and the lesson "Level of Abstraction." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Functions

## Lesson 03: Level of Abstraction

## Source
- Post folder: `post_2026-01-07_13-34-00`
- Post date: `2026-01-07 13:34:00`
- Theme folder: `functions`
- Lesson folder: `03-level-of-abstraction`

## Description
Mixing high level and low level is always confusing and obscures the code's intent.

## Hashtags
None

## Transcript
Functions are the first line of organization in any program. And just like any organization, they fail when roles get blurred. A CEO's function isn't packing boxes or choosing email fonts. They operate at the highest level. The manager works one step below, breaking big goals into smaller ones, closer to the details, but still abstracting away the specifics. And at the ground level, the specialist handles the real execution, each task focused and distinct. Your code needs that same discipline. It should read like a top-down narrative, descending one layer of abstraction at a time. Take this function that processes an order. It validates stock, calculates the bill, handles payment, and notifies the warehouse. But this function is a micromanager. Should an order processor really know the raw syntax of Stripe's API? Let's delegate. What remains is pure intent. The details are still there, just not here. Each one handled at its appropriate depth. And when you need the details, you dive deeper. Think of every function as a to paragraph. To process an order, we validate stock, calculate the bill, charge the customer, and notify the warehouse. To calculate the bill, we sum the items, apply discounts, and add tax. Each function describes its scope and points to the next. Keep every function at its level, and the whole system stays clear. Follow for more clean code principles coming next.
