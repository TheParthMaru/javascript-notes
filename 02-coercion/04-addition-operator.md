# The Addition Operator (`+`)

## What makes `+` special?

Unlike operators like:

- `-`
- `*`
- `/`

the `+` operator has **two possible meanings**:

1. **string concatenation**
2. **numeric addition**

That is why `+` is one of the most confusing operators in JavaScript.

## Core mental model

When JavaScript sees:

```js
a + b;
```

it does **not** immediately assume:

- numeric addition

Instead, it must first decide:

- should this be **string concatenation**?
- or should this be **numeric addition**?

That is the most important thing to remember.

## Spec note

The addition operator either performs:

- string concatenation
- or numeric addition

And the evaluation of:

```js
left + right;
```

is delegated to:

```text
EvaluateStringOrNumericBinaryExpression(left, +, right)
```

So the short `+` section in the spec is only an entry point.  
The real logic is in the delegated operations.

## Why `+` is different from `-`

### `-`

```js
"5" - 1; // 4
```

`-` is numeric only, so JavaScript goes directly toward numeric conversion.

### `+`

```js
"5" + 1; // "51"
```

`+` can mean two things, so JavaScript must first decide which path applies.

## `EvaluateStringOrNumericBinaryExpression(leftOperand, opText, rightOperand)`

This abstract operation prepares the operands before the real operator logic runs.

It performs these steps:

1. Evaluate the left operand
2. Get the actual left value
3. Evaluate the right operand
4. Get the actual right value
5. Pass both values and the operator text to:
   - `ApplyStringOrNumericBinaryOperator(lVal, opText, rVal)`

This is the **setup layer**.

It does **not** contain the detailed coercion rules for `+`.

Instead, it:

- evaluates both expressions
- gets their values
- delegates to the deeper operator rule

If you want to understand the real behavior of `+`, this operation is **not enough by itself**.

The actual coercion logic lives in `ApplyStringOrNumericBinaryOperator`

## `ApplyStringOrNumericBinaryOperator(lVal, opText, rVal)`

This is the most important operation for understanding `+`.

It handles binary operators like:

- `+`
- `-`
- `*`
- `/`
- `%`
- `**`
- shifts
- bitwise operators

But for our topic, the key focus is the behavior of:

- `+`

### The real rule for `+`

For:

```js
a + b;
```

JavaScript does this:

1. Convert both sides to primitives using `ToPrimitive`
2. If either primitive is a string:
   - convert both to strings
   - concatenate them
3. Otherwise:
   - treat it as a numeric operation
   - convert both sides using `ToNumeric`
   - if the numeric types differ (`Number` vs `BigInt`), throw `TypeError`
   - otherwise perform numeric addition

That is the real behavior of `+`.

## Step-by-step breakdown of the `+` path

### Step 1 — Convert left side to primitive

```text
lPrim = ToPrimitive(lVal)
```

### Step 2 — Convert right side to primitive

```text
rPrim = ToPrimitive(rVal)
```

Important:

- `ToPrimitive` here is called **without an explicit hint**
- that usually behaves like number preference
- but special objects can override this

### Step 3 — Check for string path

If:

- `lPrim` is a string
- or `rPrim` is a string

then JavaScript goes down the **string concatenation path**

### Step 4 — String concatenation path

If string path is chosen:

1. `ToString(lPrim)`
2. `ToString(rPrim)`
3. return concatenation of both strings

### Step 5 — Otherwise it must be numeric

If neither primitive is a string, the spec says it is now a **numeric operation**.

### Step 6 — Numeric conversion path

JavaScript does:

```text
lNum = ToNumeric(lVal)
rNum = ToNumeric(rVal)
```

### Step 7 — Numeric types must match

If one becomes:

- `Number`

and the other becomes:

- `BigInt`

then JavaScript throws `TypeError`.

So:

```js
1 + 2n; // TypeError
```

### Step 8 — Perform the actual numeric addition

- if both are `BigInt`, use `BigInt::add`
- otherwise use `Number::add`

## Clean mental model for `+`

When JavaScript sees:

```js
a + b;
```

think like this:

1. convert both sides to primitives
2. if either primitive is a string:
   - convert both to strings
   - concatenate
3. otherwise:
   - convert both to numeric values
   - numeric types must match
   - perform numeric addition

## Examples

### Example 1

```js
"5" + 1; // "51"
```

Why?

1. both values are already primitive
2. one primitive is a string
3. string path
4. `"5"` + `"1"` → `"51"`

### Example 2

```js
5 + "1"; // "51"
```

Why?

- one primitive is a string
- string path applies

### Example 3

```js
5 + 1; // 6
```

Why?

- neither primitive is a string
- numeric path
- both become `Number`
- numeric addition happens

### Example 4

```js
true + 1; // 2
```

Why?

- neither primitive is a string
- numeric path
- `true` becomes numeric `1`
- `1 + 1 = 2`

### Example 5

```js
null + 1; // 1
```

Why?

- neither primitive is a string
- numeric path
- `null` goes through numeric conversion and becomes `0`
- result is `1`

### Example 6

```js
1n + 2n; // 3n
```

Why?

- neither primitive is a string
- numeric path
- both stay BigInt
- BigInt addition happens

### Example 7

```js
1 + 2n; // TypeError
```

Why?

- numeric path
- one side becomes `Number`
- the other becomes `BigInt`
- types differ
- error is thrown

### Example 8

```js
[1] + 2;
```

Why this gets tricky:

- `[1]` is an object
- objects first go through `ToPrimitive`
- then JavaScript decides string path or numeric path

So object coercion is often the reason `+` feels strange.

## Why object cases feel weird

Because the `+` operator does **not** decide directly from the original object.

It first does:

```text
Object
  ↓
ToPrimitive(...)
  ↓
Primitive
  ↓
string path OR numeric path
```

That is why earlier topics like `ToPrimitive`, `ToString`, and `ToNumber` were necessary first.

# `ToNumeric(value)`

## What is `ToNumeric`?

`ToNumeric` is an abstract operation that converts a value into either:

- a **Number**
- or a **BigInt**

So the simple meaning is:

> If JavaScript needs a numeric value, it may use `ToNumeric`.

This is different from `ToNumber`.

## Why `ToNumeric` is important

`ToNumeric` is used in the numeric path of `+` and other numeric operators.

This is important because JavaScript numeric operations may support:

- Number
- BigInt

But the operator logic still requires both numeric types to match.

## The algorithm

`ToNumeric(value)` does:

1. `primValue = ToPrimitive(value, NUMBER)`
2. If `primValue` is a BigInt, return it
3. Otherwise return `ToNumber(primValue)`

## Main mental model

Use this:

> `ToNumeric` first gets a primitive with number preference.  
> If that primitive is a BigInt, keep it as BigInt.  
> Otherwise, use `ToNumber`.

That is the full idea.

## Step-by-step understanding

### Step 1 — `ToPrimitive(value, NUMBER)`

Before numeric conversion happens, JavaScript first gets a primitive with **number preference**.

This matters especially for objects.

### Step 2 — If the primitive is BigInt, return it

This is the key difference from `ToNumber`.

If the primitive value is BigInt:

- return it directly

### Step 3 — Otherwise, use `ToNumber`

If the primitive is not BigInt, then JavaScript uses the rules of `ToNumber`.

That means all `ToNumber` behavior still matters here for:

- strings
- booleans
- `null`
- `undefined`
- numbers

## `ToNumeric` vs `ToNumber`

### `ToNumber`

- returns only a `Number`
- throws for `BigInt`
- throws for `Symbol`

### `ToNumeric`

- first does `ToPrimitive(..., NUMBER)`
- if the primitive is a `BigInt`, return it
- otherwise call `ToNumber`

So:

> `ToNumeric` allows BigInt  
> `ToNumber` does not

That is the cleanest comparison.

## Examples

### Example 1

```js
5 + 2;
```

Numeric path:

- `ToNumeric(5)` → `5`
- `ToNumeric(2)` → `2`

Both are `Number`, so normal numeric addition happens.

### Example 2

```js
1n + 2n;
```

Numeric path:

- `ToNumeric(1n)` → `1n`
- `ToNumeric(2n)` → `2n`

Both are `BigInt`, so BigInt addition happens.

### Example 3

```js
1 + 2n;
```

Numeric path:

- left becomes `Number`
- right becomes `BigInt`

Types differ, so `TypeError` is thrown.

### Example 4

```js
true + 1; // 2
```

Numeric path:

- `ToPrimitive(true, NUMBER)` → `true`
- not BigInt
- `ToNumber(true)` → `1`

So result is `2`.

### Example 5

```js
"5" - 1; // 4
```

For subtraction, JavaScript uses numeric logic.

- `ToPrimitive("5", NUMBER)` → `"5"`
- not BigInt
- `ToNumber("5")` → `5`

So result becomes `4`.

## Clean object flow in `ToNumeric`

If the value is an object, the flow becomes:

```text
Object
  ↓
ToPrimitive(..., NUMBER)
  ↓
If BigInt → return BigInt
Else → ToNumber(...)
```

This is why object coercion can become complex in numeric operations.

## Final combined picture

## For `a + b`

The full big-picture flow is:

```text
a + b
  ↓
EvaluateStringOrNumericBinaryExpression(...)
  ↓
get actual left and right values
  ↓
ApplyStringOrNumericBinaryOperator(lVal, "+", rVal)
  ↓
ToPrimitive(left)
ToPrimitive(right)
  ↓
If either primitive is a string:
    ToString(left)
    ToString(right)
    concatenate
Else:
    ToNumeric(left)
    ToNumeric(right)
    ensure same numeric type
    add numerically
```

This is the most complete and useful mental model for the addition operator.
