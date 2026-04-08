---
name: code-correctness-objects-and-data-04-hybrids-dtos-and-active-records
description: Lesson module from the public s4.codes code-correctness series. Use this when the task is specifically about objects and data and the lesson "Hybrids, DTOs and Active Records." This module preserves the original reel description and transcription so the guidance stays grounded in the source material.
---

# Objects and Data

## Lesson 04: Hybrids, DTOs and Active Records

## Source
- Post folder: `post_2026-04-08_13-30-48`
- Post date: `2026-04-08 13:30:48`
- Theme folder: `objects-and-data`
- Lesson folder: `04-hybrids-dtos-and-active-records`

## Description
Pick a side, your class can't be both.

## Hashtags
#cleancode, #code, #programming, #developer

## Transcript
Some classes refuse to pick a side. They expose their data through getters and setters, but they also have methods that do real work. This class does both. It lets anyone read and write the price, but it also calculates discounts. So when a subscription comes along with different pricing, the discount logic is trapped inside this class. And when you need new behavior, the data has already leaked. External code uses the getters everywhere. You've made it hard to add functions and it's hard to add types. That's a hybrid. The worst of both worlds. But a pure data structure doesn't have this problem. A DTO has public fields and no behavior. It carries data between your database and your code, and that's all it needs to do. ActiveRecord looks similar. They're data structures with navigational methods like save and delete that map directly to database rows, and that's their job. But the problem starts when developers add business logic. Now the class calculates discounts, and you've got a hybrid again. Half data structure, half object, committed to neither. The fix isn't removing save and delete. It's treating the ActiveRecord as what it is and putting the business rules somewhere else. The pricing logic moves into its own object. The ActiveRecord stays clean, and each class has one clear identity. Know what you're building and commit to it. Objects hide data and expose behavior. Data structures expose data and skip the behavior. And the moment you try to do both, you lose the advantages of each. Follow for more clean code principles coming next.
