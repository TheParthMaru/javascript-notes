# JavaScript Notes — `IsLooselyEqual(x, y)` and `IsStrictlyEqual(x, y)`

These notes explain the ECMAScript abstract operations behind:

- `==` → loose equality
- `===` → strict equality

The goal is to understand the rules from the spec in a simple and structured way.

# 1. Why this topic matters

Loose equality is one of the most misunderstood parts of JavaScript.

Many people memorize examples like:

```js
5 == "5"
false == 0
"" == 0
null == undefined
[] == 0
```

But the better way is to understand the actual spec rule behind them.

The key point is:

> `==` does **not** mean “convert both sides to numbers”.

That is not the real general rule.

Instead, `==` follows a **case-by-case algorithm**.

# 2. The two equality operations

## Loose equality

```js
x == y;
```

This uses:

- `IsLooselyEqual(x, y)`

## Strict equality

```js
x === y;
```

This uses:

- `IsStrictlyEqual(x, y)`

So `==` and `===` are not just “same operator with extra strictness”.  
They are backed by different abstract operations.

# 3. `IsStrictlyEqual(x, y)`

Let’s start with the easier one.

## Algorithm

`IsStrictlyEqual(x, y)` does:

1. If `SameType(x, y)` is false, return `false`.
2. If `x` is a Number, return `Number::equal(x, y)`.
3. Otherwise return `SameValueNonNumber(x, y)`.

## Simple meaning

Strict equality works like this:

1. If the types are different → `false`
2. If both are numbers → compare using number equality rules
3. Otherwise compare according to same-type value equality rules

## Important mental model

For `===`:

> No cross-type coercion happens.

That is the biggest rule.

### Examples

```js
5 === "5"; // false
false === 0; // false
null === undefined; // false
```

- Because the types differ.

## Important notes for numbers

The spec note says `IsStrictlyEqual` differs from `SameValue` in its treatment of:

- signed zeroes
- `NaN`

That means:

```js
NaN === NaN; // false
0 === -0; // true
```

These are important number-specific details.

---

# 4. `IsLooselyEqual(x, y)`

Now the main topic.

## What it is

`IsLooselyEqual(x, y)` is the abstract operation that provides the semantics for:

```js
x == y;
```

This operation is more complex because it may:

- compare directly
- convert strings to numbers
- convert booleans to numbers
- convert objects to primitives
- compare `Number` and `BigInt`
- handle the special `null` / `undefined` rule

---

# 5. Main mental model for `==`

Use this:

> `==` is a case-based comparison algorithm.

Do **not** use this as your mental model:

> `==` converts both sides to numbers

That idea is incomplete and often wrong.

The real process is:

1. check if the types are already the same
2. if not, follow a specific rule for that type pair
3. repeat recursively if needed

---

# 6. Step-by-step explanation of `IsLooselyEqual`

## Rule 1 — Same type → use strict equality

If `x` and `y` already have the same type:

- return `IsStrictlyEqual(x, y)`

### Meaning

If the two values are already of the same type, loose equality does **not** invent extra conversion rules.

It just falls back to strict equality logic.

### Examples

```js
5 == 5; // true
"hi" == "hi"; // true
true == true; // true
```

And:

```js
NaN == NaN; // false
```

because same type leads to strict number equality rules.

---

## Rule 2 and 3 — `null` and `undefined` are loosely equal to each other

If:

- `x` is `null` and `y` is `undefined` → `true`
- `x` is `undefined` and `y` is `null` → `true`

### This is one of the most important special rules

```js
null == undefined; // true
```

But:

```js
null === undefined; // false
```

because strict equality checks type first.

### Important

This does **not** mean:

- `null == 0`
- `undefined == 0`

Those are different cases.

---

## Rule 4 — Normative optional browser-specific case

This is the `[[IsHTMLDDA]]` case.

This is a special browser-related historical behavior and is not important for normal learning of JavaScript coercion.

For now, you can safely treat this as:

> special legacy web behavior, not part of normal everyday equality reasoning.

---

## Rule 5 — Number compared with String

If:

- `x` is a Number
- `y` is a String

then:

- convert `y` using `ToNumber`
- compare again

### Example

```js
5 == "5";
```

Flow:

1. Number vs String
2. convert `"5"` using `ToNumber` → `5`
3. compare again: `5 == 5`
4. same type now → strict equality
5. result → `true`

---

## Rule 6 — String compared with Number

If:

- `x` is a String
- `y` is a Number

then:

- convert `x` using `ToNumber`
- compare again

### Example

```js
"5" == 5;
```

Flow:

1. String vs Number
2. convert `"5"` → `5`
3. compare again: `5 == 5`
4. result → `true`

---

## Rule 7 — BigInt compared with String

If:

- `x` is a BigInt
- `y` is a String

then:

1. try `StringToBigInt(y)`
2. if it fails, return `false`
3. otherwise compare again with the resulting BigInt

### Example

```js
10n == "10"; // true
```

Because `"10"` can become BigInt `10n`.

### Example

```js
10n == "10.5"; // false
```

Because `"10.5"` is not a valid BigInt string.

---

## Rule 8 — String compared with BigInt

This just reverses the previous case.

### Example

```js
"10" == 10n; // true
```

---

## Rule 9 — Boolean on the left

If `x` is a Boolean:

- convert it using `ToNumber`
- compare again

### Example

```js
false == 0;
```

Flow:

1. Boolean on left
2. `ToNumber(false)` → `0`
3. compare again: `0 == 0`
4. result → `true`

### Example

```js
true == 1; // true
```

---

## Rule 10 — Boolean on the right

If `y` is a Boolean:

- convert it using `ToNumber`
- compare again

### Example

```js
0 == false;
```

Flow:

1. Boolean on right
2. `ToNumber(false)` → `0`
3. compare again: `0 == 0`
4. result → `true`

---

## Rule 11 — Primitive-like value compared with Object

If `x` is one of:

- String
- Number
- BigInt
- Symbol

and `y` is an Object

then:

- convert `y` using `ToPrimitive`
- compare again

### Example

```js
"1" == [1];
```

Flow:

1. left is String
2. right is Object
3. convert `[1]` with `ToPrimitive`
4. array becomes some primitive
5. compare again

This is where object equality weirdness begins.

---

## Rule 12 — Object compared with primitive-like value

If `x` is an Object and `y` is one of:

- String
- Number
- BigInt
- Symbol

then:

- convert `x` using `ToPrimitive`
- compare again

### Example

```js
[1] == 1;
```

Flow:

1. left is Object
2. right is Number
3. convert `[1]` using `ToPrimitive`
4. compare again with resulting primitive

---

## Rule 13 — BigInt and Number comparison

If one side is BigInt and the other is Number:

1. if either is not finite → `false`
2. if their real mathematical values are equal → `true`
3. otherwise → `false`

### Examples

```js
1n == 1; // true
2n == 2; // true
1n == 1.5; // false
```

### Important

This is comparison only.

Do not confuse it with numeric operations like:

```js
1n + 1; // TypeError
```

That is operator logic, not loose equality logic.

---

## Rule 14 — Otherwise return false

If none of the special cases matched, the result is:

```js
false;
```

---

# 7. The recursive nature of `==`

One of the most important things to notice is that many rules say:

- convert one side
- then call `IsLooselyEqual(...)` again

This means loose equality often works in **layers**.

### Example

```js
false == "0";
```

Flow:

1. left is Boolean
2. convert `false` → `0`
3. now compare `0 == "0"`
4. Number vs String
5. convert `"0"` → `0`
6. compare `0 == 0`
7. same type → strict equality
8. result → `true`

This is why `==` can feel tricky.

---

# 8. Important classic examples

## Example 1

```js
5 == "5"; // true
```

Reason:

- Number vs String
- convert string to number
- compare `5 == 5`

---

## Example 2

```js
false == 0; // true
```

Reason:

- Boolean becomes `0`
- compare `0 == 0`

---

## Example 3

```js
"" == 0; // true
```

Reason:

1. String vs Number
2. convert `""` using `ToNumber`
3. `ToNumber("")` → `0`
4. compare `0 == 0`

---

## Example 4

```js
null == undefined; // true
```

Reason:

- direct special-case rule

---

## Example 5

```js
null == 0; // false
```

Reason:

- there is no rule saying `null` should become `0` in loose equality
- special `null` rule only pairs it with `undefined`

---

## Example 6

```js
undefined == 0; // false
```

Reason:

- no matching conversion rule to make this true

---

## Example 7

```js
[] == 0; // true
```

Step-by-step high-level flow:

1. left is Object
2. right is Number
3. convert `[]` using `ToPrimitive`
4. array becomes a primitive
5. then String vs Number comparison may happen
6. empty string can become `0`
7. result ends up `true`

This is why object cases feel weird.

---

## Example 8

```js
[1] == 1; // true
```

Reason:

- object becomes primitive
- resulting primitive can become `"1"`
- then String vs Number rule converts `"1"` to `1`
- compare `1 == 1`

---

## Example 9

```js
[] == ""; // true
```

Reason:

- object becomes primitive
- both sides can end up as same string value

---

## Example 10

```js
NaN == NaN; // false
```

Reason:

- same type → strict equality
- strict numeric equality says `NaN` is not equal to `NaN`

---

# 9. Very important contrast: `==` vs `===`

## `===`

- same types only
- no cross-type coercion
- much easier to reason about

## `==`

- special-case algorithm
- may convert values
- may recurse
- object-to-primitive conversion may happen
- more surprising

---

# 10. When `==` is sometimes acceptable

Even though most codebases prefer `===`, one common and intentional use is:

```js
value == null;
```

Why?

Because it checks both:

- `null`
- `undefined`

due to the special loose equality rule.

So:

```js
value == null;
```

can be a short way of saying:

```js
value === null || value === undefined;
```

But outside of such cases, `===` is usually clearer and safer.

---

# 11. Common mistakes

## Mistake 1

Thinking `==` always converts both sides to numbers.

Wrong.  
It uses type-pair-specific rules.

---

## Mistake 2

Thinking `null == 0` should be true because `Number(null)` is `0`.

Wrong.  
Loose equality is not simply `ToNumber` on both sides.

---

## Mistake 3

Thinking object comparisons are direct.

Wrong.  
Objects may first go through `ToPrimitive`.

---

## Mistake 4

Confusing `==` behavior with boolean coercion.

These are different:

```js
if ("") { ... }   // boolean coercion
"" == 0           // loose equality
```

---

## Mistake 5

Assuming `===` means “better version of `==`”.

Not exactly.

They are different algorithms with different behavior.

---

# 12. Best practices

## Prefer `===` by default

This avoids most surprising coercion behavior.

Good:

```js
userRole === "admin";
count === 0;
flag === true;
```

## Use `== null` only when you intentionally want both `null` and `undefined`

Example:

```js
if (value == null) {
	// handles null and undefined
}
```

## Be careful with objects in `==`

Expressions like:

```js
[] == 0
[1] == 1
[] == ""
```

are valid but not readable.

Avoid writing code that depends on such behavior.

---

# 13. Final summary table

| Expression           | Result  | Main reason                            |
| -------------------- | ------- | -------------------------------------- |
| `5 == "5"`           | `true`  | String → Number                        |
| `false == 0`         | `true`  | Boolean → Number                       |
| `"" == 0`            | `true`  | String → Number                        |
| `null == undefined`  | `true`  | special-case rule                      |
| `null == 0`          | `false` | no such loose-equality conversion rule |
| `undefined == 0`     | `false` | no matching rule                       |
| `1n == 1`            | `true`  | BigInt/Number mathematical equality    |
| `NaN == NaN`         | `false` | same type → strict numeric equality    |
| `5 === "5"`          | `false` | type mismatch                          |
| `null === undefined` | `false` | type mismatch                          |

---

# 14. Mini summary

- `===` first checks type and does not perform cross-type coercion
- `==` uses a case-based algorithm
- `null` and `undefined` are loosely equal to each other
- booleans become numbers in loose equality
- strings may become numbers
- objects may become primitives
- `==` is recursive in many cases
- `===` should be your default choice in real-world code

---

# 15. Suggested practice questions

Try explaining these step by step from the algorithm:

```js
5 == "5"
false == "0"
"" == 0
null == undefined
null == 0
[] == 0
[1] == 1
1n == 1
NaN == NaN
5 === "5"
```
