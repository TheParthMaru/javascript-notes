# JavaScript Coercion Learning Path

A structured roadmap to learn coercion in JavaScript in a deep, practical, and step-by-step way.

---

## Purpose of this plan

This learning path is designed to help me learn coercion properly without trying to understand everything at once.

The goal is to:

- build the topic layer by layer
- understand the most important coercion rules deeply
- connect coercion to real JavaScript behavior
- use the ECMAScript specification as a reference when needed
- avoid shallow memorization of weird examples

---

## How this topic will be studied

We will not treat coercion as one giant topic.

Instead, we will study it in stages.

For each stage, the teaching flow should be:

1. **Concept**
   - What this part means

2. **Why it matters**
   - Why this rule exists and where it appears in JavaScript

3. **Mental model**
   - A simple and correct way to think about it

4. **Spec link**
   - Direct modern ECMAScript source link

5. **Examples**
   - Small, focused examples first

6. **Common mistakes**
   - What people usually misunderstand

7. **Quick check**
   - Prediction questions to test understanding

8. **Mini summary**
   - Short recap before moving to the next stage

We should only move to the next stage after the current one is clear.

---

## Main study principle

I do **not** need to memorize every JavaScript internal algorithm.

I **do** need to understand:

- the important coercion rules
- the mental models behind them
- the most useful abstract operations from the spec
- how to verify behavior from the ECMAScript docs when needed

---

## Core topics we will focus on

These are the high-value parts of coercion that matter most in real coding and interviews:

- primitive conversion basics
- string coercion
- number coercion
- boolean coercion
- object-to-primitive conversion
- operator-based coercion
- loose equality (`==`)
- why strict equality (`===`) avoids most coercion issues
- real-world coding practices around coercion
- spec navigation

---

## Recommended study order

1. Mental model of coercion
2. Primitive values involved in coercion
3. `ToBoolean`
4. `ToNumber`
5. `ToString`
6. `ToPrimitive`
7. Operator-based coercion
8. Loose equality `==`
9. Real-world best practices
10. Spec navigation drills

This order is chosen because:

- boolean coercion is the easiest starting point
- number and string coercion are very common in real code
- object coercion is harder, so it should come later
- `==` becomes much easier after understanding the earlier rules

---

# Stage 0 — Build the base mental model

## Goal

Understand what coercion actually means before looking at tricky examples.

## What to learn

- what “type coercion” means
- implicit coercion vs explicit conversion
- why JavaScript converts values at all
- the idea that coercion is context-based, not random

## Expected outcome

I should be able to explain:

- why `"5" + 1` behaves differently from `"5" - 1`
- why `if ("hello")` works
- why coercion is not “magic”

## Spec reference

- Type Conversion:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-type-conversion

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 1 — Primitive values and where coercion happens

## Goal

Know what kinds of values JavaScript is converting.

## What to learn

- primitive types involved in coercion:
  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`
  - `bigint`
  - `symbol`
- difference between primitive values and objects
- why many coercion rules first try to get an object into a primitive form

## Expected outcome

I should be able to say:

- this value is already primitive
- this is an object, so JavaScript may first need `ToPrimitive`

## Spec reference

- ECMAScript Data Types and Values:  
  https://tc39.es/ecma262/multipage/ecmascript-data-types-and-values.html

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 2 — `ToBoolean`

## Goal

Learn the simplest coercion rule first.

## Why this stage matters

Boolean coercion is the cleanest entry point into coercion.

## What to learn

- truthy and falsy values
- why `if`, `while`, logical operators, and ternary conditions depend on boolean conversion
- why `"0"` is truthy but `0` is falsy
- why `[]` is truthy

## Expected outcome

I should be able to predict condition checks without guessing.

## Spec reference

- `ToBoolean(argument)`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-toboolean

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 3 — `ToNumber`

## Goal

Understand how values become numbers.

## What to learn

- number conversion from:
  - strings
  - booleans
  - `null`
  - `undefined`
  - arrays
- why `Number("")` becomes `0`
- why `Number("   42   ")` works
- why `Number(undefined)` becomes `NaN`
- why some strings fail and become `NaN`

## Expected outcome

I should be able to explain cases like:

- `+"5"`
- `"5" - 1`
- `Number(false)`
- `Number(null)`

## Spec reference

- `ToNumber(argument)`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-tonumber

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 4 — `ToString`

## Goal

Understand string coercion and why `+` is special.

## What to learn

- string conversion from numbers, booleans, `null`, and `undefined`
- difference between:
  - `String(value)`
  - template literals
  - string concatenation with `+`
- why `1 + "2"` becomes `"12"`

## Expected outcome

I should be able to explain when JavaScript is doing:

- numeric addition
- string concatenation

## Spec reference

- `ToString(argument)`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-tostring

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 5 — `ToPrimitive`

## Goal

Understand the most important bridge rule for objects.

## Why this stage matters

This is where many weird coercion cases begin.

## What to learn

- objects are not directly used in many operations
- JavaScript often first converts an object to a primitive
- the role of:
  - `valueOf()`
  - `toString()`
  - preferred type / hint
- why arrays behave oddly in coercion

## Expected outcome

I should be able to explain:

- `[] + []`
- `[1] + 1`
- why object coercion often ends up as strings

## Spec references

- `ToPrimitive(input [, preferredType])`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-toprimitive
- `OrdinaryToPrimitive(O, hint)`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-ordinarytoprimitive

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 6 — Operator-based coercion

## Goal

See how coercion depends on the operator being used.

## What to learn

- `+` is special because it can mean:
  - numeric addition
  - string concatenation
- `-`, `*`, `/`, `%` expect numeric conversion
- comparison operators may trigger conversion
- logical operators do not always return booleans

## Expected outcome

I should stop treating coercion as one generic thing and start asking:

- which operation is happening?
- what kind of value does that operation need?

## Suggested spec references

- ECMAScript language expressions:  
  https://tc39.es/ecma262/multipage/ecmascript-language-expressions.html
- Addition operator (`+`):  
  https://tc39.es/ecma262/multipage/ecmascript-language-expressions.html#sec-addition-operator-plus
- Unary plus operator:  
  https://tc39.es/ecma262/multipage/ecmascript-language-expressions.html#sec-unary-plus-operator

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 7 — Loose equality `==`

## Goal

Learn the most misunderstood part of coercion.

## What to learn

- `==` does not simply convert both sides to numbers
- different type pairs follow different rules
- very important cases:
  - `5 == "5"`
  - `false == 0`
  - `"" == 0`
  - `null == undefined`
  - `[] == 0`
- why `null == undefined` is special
- why `===` avoids most of this

## Expected outcome

I should be able to explain these cases with confidence instead of memorizing them blindly.

## Spec reference

- `IsLooselyEqual(x, y)`:  
  https://tc39.es/ecma262/multipage/abstract-operations.html#sec-islooselyequal

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 8 — Real-world engineering rules

## Goal

Move from theory to safe coding practice.

## What to learn

- when implicit coercion is acceptable
- when explicit conversion is better
- why most codebases prefer `===`
- how to write readable conditions
- when using `Boolean()`, `Number()`, or `String()` is clearer than relying on implicit coercion

## Expected outcome

I should be able to write code that is:

- readable
- predictable
- interview-safe
- production-safe

## Suggested references

- Revisit:
  - `ToBoolean`
  - `ToNumber`
  - `ToString`
  - `IsLooselyEqual`
- Strict equality operators section:  
  https://tc39.es/ecma262/multipage/ecmascript-language-expressions.html#sec-equality-operators

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Stage 9 — Spec navigation skill

## Goal

Learn how to use the ECMAScript docs as a reference without getting lost.

## What to learn

- how to search the multipage spec
- how to move between:
  - operator behavior
  - abstract operations
  - data types
- how to trace a coercion case step by step

## Expected outcome

When I forget a rule, I should know how to find it instead of guessing.

## Main references

- Multipage ECMAScript spec home:  
  https://tc39.es/ecma262/multipage/
- Abstract operations:  
  https://tc39.es/ecma262/multipage/abstract-operations.html
- ECMAScript language expressions:  
  https://tc39.es/ecma262/multipage/ecmascript-language-expressions.html

## Progress

- [ ] Not started
- [ ] In progress
- [ ] Completed

---

# Study method during the journey

For each stage:

- do not rush
- ask questions whenever something feels unclear
- do a few predictions before seeing the answer
- keep notes of confusing examples
- connect each rule to operator behavior or real code usage
- only move forward when the current stage feels stable

---

# What this learning path is trying to prevent

This plan is specifically designed to avoid these common problems:

- memorizing random weird examples without understanding
- mixing boolean coercion with number coercion
- thinking `==` always converts both sides to numbers
- calling coercion “magic”
- getting lost in the ECMAScript docs too early
- learning advanced object coercion before understanding primitive conversion

---

# Final objective

By the end of this roadmap, I should be able to:

- explain what coercion is in simple and correct terms
- predict common coercion behavior with confidence
- explain `ToBoolean`, `ToNumber`, `ToString`, and `ToPrimitive`
- understand why `==` behaves the way it does
- know when to rely on coercion and when to avoid it
- use the ECMAScript specification as a reference when needed

---

# Official spec references

## Official standard page

- ECMAScript 2025 (ECMA-262):  
  https://ecma-international.org/publications-and-standards/standards/ecma-262/

## Best reading version for learning

- TC39 ECMAScript HTML spec:  
  https://tc39.es/ecma262/

---

# Progress tracker

- [ ] Stage 0 — Mental model of coercion
- [ ] Stage 1 — Primitive values and where coercion happens
- [ ] Stage 2 — `ToBoolean`
- [ ] Stage 3 — `ToNumber`
- [ ] Stage 4 — `ToString`
- [ ] Stage 5 — `ToPrimitive`
- [ ] Stage 6 — Operator-based coercion
- [ ] Stage 7 — Loose equality `==`
- [ ] Stage 8 — Real-world engineering rules
- [ ] Stage 9 — Spec navigation skill
