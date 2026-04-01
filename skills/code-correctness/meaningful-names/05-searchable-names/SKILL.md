---
name: code-correctness-meaningful-names-05-searchable-names
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about meaningful names and the lesson "Searchable Names." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Meaningful Names

## Lesson 05: Searchable Names

## Source
- Post folder: `post_2025-12-17_15-05-11`
- Post date: `2025-12-17 15:05:11`
- Theme folder: `meaningful-names`
- Lesson folder: `05-searchable-names`

## Description
Searchable names make the code navigable 

#programming #cleancode #code

## Hashtags
#programming, #cleancode, #code

## Transcript
You see, single-letter names and numeric constants have a real problem. Basically, they're impossible to locate in a codebase. Think about it. You can search for "max classes per student" and find it instantly. But if you search for the number 7? It appears in filenames, other constants, and everywhere with different meanings. In fact, the letter "e" is even worse. It's the most common letter in English and shows up in every program, making searches totally useless. Fortunately, #cleancode cleancode has a principle for this: use searchable names. The rule is simple. The length of a name should match the size of its scope. So what about single letters? Use them only for local variables in short methods. Everything else needs a searchable name. For example, look at this calculation. Here, the numbers 4 and 5 could mean anything. Now watch the same logic with searchable names. Suddenly, you can find workdays per week in seconds. The code is longer, yes, but it becomes infinitely easier to maintain. Ultimately, searchable names make your code navigable. Follow for more clean code principles.
