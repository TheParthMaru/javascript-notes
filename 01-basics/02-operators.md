# Operators in JavaScript

## What is an Operator?

- An operator is a symbol or keyword that tells JavaScript to perform an operation on one or more values.
- These values are called operands.
- For example,

```js
let sum = 10 + 5;
```

- Here:
  - `+` is the operator.
  - `10` and `5` are operands.
  - the operation is addition.

## Why Operators Matter

- Operators are used everywhere in JavaScript:
  - doing calculations
  - comparing values
  - assigning values
  - checking logical conditions
  - working with strings
  - handling defaults
  - accessing object properties safely
  - writing cleaner and shorter code

## Categories of Operators

JavaScript operators can be broadly grouped into:

1. Arithmetic Operators
2. Assignment Operators
3. Comparison Operators
4. Equality Operators
5. Logical Operators
6. Unary Operators
7. Increment and Decrement Operators
8. String Operators
9. Conditional (Ternary) Operator
10. Type Operators
11. Bitwise Operators
12. Nullish Coalescing Operator
13. Optional Chaining
14. Comma Operator
15. Special/Other Useful Operators

## Arithmetic Operators

Arithmetic operators are used for mathematical calculations.

### 1. Addition (`+`)

Adds two values.

```js
let a = 10;
let b = 5;
console.log(a + b); // 15
```

**Note:**

In JavaScript, `+` is also used for string concatenation.

```js
console.log("Hello " + "World"); // Hello World
console.log("Age: " + 25); // Age: 25
```

### 2. Subtraction (`-`)

Subtracts the right operand from the left operand.

```js
console.log(10 - 3); // 7
```

### 3. Multiplication (`*`)

Multiplies two values.

```js
console.log(4 * 3); // 12
```

### 4. Division (`/`)

Divides the left operand by the right operand.

```js
console.log(10 / 2); // 5
console.log(5 / 2); // 2.5
```

### 5. Remainder (`%`)

Returns the remainder after division.

```js
console.log(10 % 3); // 1
console.log(8 % 2); // 0 -> even
console.log(7 % 2); // 1 -> odd
```

### 6. Exponentiation (`**`)

Raises the left operand to the power of the right operand.

```js
console.log(2 ** 3); // 8
console.log(5 ** 2); // 25
```

## Assignment Operators

Assignment operators assign values to variables.

### 1. Simple Assignment (`=`)

```js
let x = 10;
```

### 2. Add and Assign (`+=`)

```js
let x = 10;
x += 5;
console.log(x); // 15
```

Equivalent to:

```js
x = x + 5;
```

### 3. Subtract and Assign (`-=`)

```js
let x = 10;
x -= 3;
console.log(x); // 7
```

### 4. Multiply and Assign (`*=`)

```js
let x = 4;
x *= 2;
console.log(x); // 8
```

### 5. Divide and Assign (`/=`)

```js
let x = 20;
x /= 4;
console.log(x); // 5
```

### 6. Remainder and Assign (`%=`)

```js
let x = 10;
x %= 3;
console.log(x); // 1
```

### 7. Exponentiation and Assign (`**=`)

```js
let x = 2;
x **= 3;
console.log(x); // 8
```

### 8. Logical Assignment Operators

#### AND assignment (`&&=`)

Assigns only if left side is truthy.

```js
let name = "Parth";
name &&= "Updated";
console.log(name); // Updated

let value = "";
value &&= "Hello";
console.log(value); // ""
```

#### OR assignment (`||=`)

Assigns only if left side is falsy.

```js
let username = "";
username ||= "Guest";
console.log(username); // Guest
```

#### Nullish assignment (`??=`)

Assigns only if left side is `null` or `undefined`.

```js
let age;
age ??= 18;
console.log(age); // 18

let score = 0;
score ??= 100;
console.log(score); // 0
```

## Comparison Operators

Comparison operators compare two values and return a boolean.

### 1. Greater Than (`>`)

```js
console.log(10 > 5); // true
```

### 2. Less Than (`<`)

```js
console.log(3 < 2); // false
```

### 3. Greater Than or Equal To (`>=`)

```js
console.log(10 >= 10); // true
```

### 4. Less Than or Equal To (`<=`)

```js
console.log(7 <= 8); // true
```

## Equality Operators

This is one of the most important areas in JavaScript.

There are two major types of equality checks:

1. Loose equality
2. Strict equality

### 1. Loose Equality (`==`)

Checks whether two values are equal after type coercion. Internally, JS will convert the types before comparing.

```js
console.log(5 == "5"); // true, because JS internally converts "5" to 5.
console.log(false == 0); // true, because JS internally converts false to 0.
console.log("" == 0); // true, because JS internally converts "" to 0.
console.log(null == undefined); // true, because in loose equality, JS treats null and undefined as equal
```

### 2. Loose Inequality (`!=`)

```js
console.log(5 != "6"); // true
console.log(5 != "5"); // false
```

### 3. Strict Equality (`===`)

Checks equality without type coercion.

```js
console.log(5 === "5"); // false
console.log(5 === 5); // true
```

### 4. Strict Inequality (`!==`)

```js
console.log(5 !== "5"); // true
console.log(5 !== 5); // false
```

### Best practice

Prefer `===` and `!==` in modern JavaScript.

## Difference Between `==` and `===`

### `==`

- compares after type conversion
- can produce unexpected results

### `===`

- no type conversion
- safer and more predictable

```js
console.log(0 == false); // true
console.log(0 === false); // false
```

## Logical Operators

Logical operators are used to combine or invert conditions.

### Truthy and Falsy Values

Logical operators often depend on whether a value is truthy or falsy.

#### Falsy values in JavaScript

- `false`
- `0`
- `-0`
- `0n`
- `""`
- `null`
- `undefined`
- `NaN`

Everything else is generally truthy.

```js
console.log(Boolean("Hello")); // true
console.log(Boolean([])); // true
console.log(Boolean({})); // true
console.log(Boolean("0")); // true
```

### 1. Logical AND (`&&`)

Returns true only if both conditions are true.

```js
let age = 20;
console.log(age > 18 && age < 30); // true
```

In JavaScript, `&&` returns:

- the first falsy value, or
- the last value if all are truthy

```js
console.log("Hello" && "World"); // World
console.log(0 && "World"); // 0
```

### 2. Logical OR (`||`)

Returns true if at least one condition is true.

```js
let isAdmin = false;
let isEditor = true;
console.log(isAdmin || isEditor); // true
```

In JavaScript, `||` returns:

- the first truthy value, or
- the last value if none are truthy

```js
console.log("" || "Default"); // Default
console.log("Hello" || "World"); // Hello
```

### 3. Logical NOT (`!`)

Flips true to false and false to true.

```js
console.log(!true); // false
console.log(!false); // true
```

### 4. Double NOT (`!!`)

Used to convert a value into a boolean.

```js
console.log(!!"hello"); // true
console.log(!!0); // false
```

## Unary Operators

Unary operators work on a single operand.

### Unary Plus (`+`)

Tries to convert a value to a number.

```js
console.log(+"5"); // 5
console.log(+true); // 1
console.log(+"abc"); // NaN
```

### Unary Minus (`-`)

Converts to number and negates it.

```js
console.log(-"5"); // -5
console.log(-true); // -1
```

### `typeof`

Returns the type of a value.

```js
console.log(typeof 42); // "number"
console.log(typeof "hello"); // "string"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof {}); // "object"
console.log(typeof null); // "object"
```

### `delete`

Deletes a property from an object.

```js
const user = { name: "Parth", age: 25 };
delete user.age;
console.log(user); // { name: "Parth" }
```

### `void`

Evaluates an expression and returns `undefined`.

```js
console.log(void 0); // undefined
```

## Increment and Decrement Operators

### Increment (`++`)

```js
let x = 5;
x++;
console.log(x); // 6
```

### Decrement (`--`)

```js
let x = 5;
x--;
console.log(x); // 4
```

### Prefix vs Postfix

#### Prefix increment

```js
let x = 5;
let y = ++x;
console.log(x); // 6
console.log(y); // 6
```

#### Postfix increment

```js
let a = 5;
let b = a++;
console.log(a); // 6
console.log(b); // 5
```

## String Operator

### Concatenation with `+`

```js
let firstName = "Parth";
let lastName = "Maru";
console.log(firstName + " " + lastName); // Parth Maru
console.log("Age: " + 25); // Age: 25
```

### Modern style

Template literals are usually cleaner:

```js
let age = 25;
console.log(`Age: ${age}`);
```

## Conditional (Ternary) Operator

The ternary operator is a shorthand for `if...else`.

### Syntax

```js
condition ? valueIfTrue : valueIfFalse;
```

### Example

```js
let age = 20;
let result = age >= 18 ? "Adult" : "Minor";
console.log(result); // Adult
```

Use ternary for short and simple decisions.

## Type Operators

### `typeof`

Already covered above.

### `instanceof`

Checks whether an object is an instance of a constructor.

```js
let arr = [1, 2, 3];
console.log(arr instanceof Array); // true
console.log(arr instanceof Object); // true
```

```js
function Person(name) {
	this.name = name;
}

let p1 = new Person("Parth");
console.log(p1 instanceof Person); // true
```

### `in`

Checks whether a property exists in an object.

```js
const user = { name: "Parth", age: 25 };
console.log("name" in user); // true
console.log("email" in user); // false
```

## Bitwise Operators

Bitwise operators work on the binary representation of numbers, that is, on individual bits. The operations happens bit by bit.

Bitwise operators in JavaScript work on 32-bit signed integers. That means JavaScript temporarily converts the number into a 32-bit binary integer before performing the operation.

So even though JavaScript normally uses the `number` type for all numeric values, bitwise operations treat numbers as 32-bit integers internally.

Main bitwise operators:

- `&` AND
- `|` OR
- `^` XOR
- `~` NOT
- `<<` left shift
- `>>` right shift
- `>>>` unsigned right shift

### Bitwise AND (`&`)

The bitwise AND operator returns `1` only if both bits are `1`.
Otherwise, it returns `0`.

#### Truth Table

```
0 & 0 = 0
0 & 1 = 0
1 & 0 = 0
1 & 1 = 1
```

```js
console.log(5 & 3); // 1

/*
Convert to binary
5 = 0101
3 = 0011

Now compare bit by bit
  0101
& 0011
------
  0001 = 1
------
*/
```

Bitwise AND is often used to:

- check whether a particular bit is set
- masking
- low-level flags

### Bitwise OR (`|`)

The bitwise OR operator returns `1` if at least one of the bits is `1`.

#### Truth Table

```
0 | 0 = 0
0 | 1 = 1
1 | 0 = 1
1 | 1 = 1
```

```js
console.log(5 | 3);

/*
Convert to binary
5 = 0101
3 = 0011

Compare
  0101
| 0011
------
  0111 = 7
------
*/
```

Bitwise OR is used for:

- setting bits
- combining flags

### Bitwise XOR (`^`)

- XOR stands for exclusive OR.
- It returns `1` when the two bits are different.
- It returns `0` when the two bits are the same.

#### Truth Table

```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

```js
console.log(5 ^ 3); // 6

/*
Convert to binary
5 = 0101
3 = 0011

Compare
  0101
^ 0011
------
  0110 = 6
------
*/
```

XOR is often used in:

- toggling bits
- finding unique values in certain problems
- swap tricks in low-level programming

### Bitwise NOT (`~`)

- Bitwise NOT flips every bit:
  - 0 becomes 1
  - 1 becomes 0

```js
console.log(~5); // -6

/*
Convert to binary 32 bit
5 = 00000000 00000000 00000000 00000101

Applying NOT
~5 = 11111111 11111111 11111111 11111010
*/
```

- This happens because JS uses 32-bit signed integers, and negative numbers are stored using two’s complement representation.

#### Generic formula

```md
~n === -(n + 1)
```

### Left Shift (`<<`)

- The left shift operator shifts all bits to the left by a given number of positions.
- Each left shift generally multiplies the number by 2.

```js
console.log(5 << 1); // 10

/*
Convert to binary
5 = 0101

Shift by 1
1010 = 10
*/

console.log(5 << 2); // 20

/*
Convert to binary
5 = 0101

Shift by 2
0101 << 2 = 10100 = 20
*/
```

- Left shift is used when working with:
  - powers of 2
  - bit masks
  - binary flags

#### Pattern

```
n << 1 ≈ n * 2
n << 2 ≈ n * 4
n << 3 ≈ n * 8
```

### Right Shift (`>>`)

- The right shift operator shifts bits to the right.
- For positive numbers, each right shift generally divides the number by 2 and discards the remainder.

```js
console.log(8 >> 1); // 4

/*
Convert to binary
8 = 1000

Shift by 1
0100 = 4
*/

console.log(8 >> 2); // 2

/*
Convert to binary
8 = 1000

Shift by 2
1000 >> 2 = 0010 = 2
*/
```

#### Pattern

```
n >> 1 ≈ floor(n / 2)
n >> 2 ≈ floor(n / 4)
```

- `>>` preserves the sign bit, so negative numbers remain negative.
- That is why it is called sign-propagating right shift.

### Zero-fill Right Shift (`>>>`)

- This operator also shifts bits to the right, but it fills the left side with 0s.
- Unlike `>>`, it does not preserve the sign in the same way.

```js
console.log(8 >>> 1); // 4
```

- For positive numbers, >> and >>> often give the same result.
- But for negative numbers, they behave differently.

**Difference between `>>` and `>>>` :**

- `>>`
  - keeps the sign
  - used for signed integers

- `>>>`
  - fills leftmost bits with 0
  - treats the number more like an unsigned value

```js
console.log(-8 >> 1); // -4
console.log(-8 >>> 1); // large positive number
```

- You do not need to master this deeply right now.
- Just know:

- `>>`: keeps sign
- `>>>`: zero-fills from the left

## Nullish Coalescing Operator (`??`)

Returns the right side only if the left side is:

- `null`
- `undefined`

```js
let username = null;
console.log(username ?? "Guest"); // Guest

let count = 0;
console.log(count ?? 10); // 0
```

### Why not always use `||`?

Because `||` treats all falsy values as absent.

```js
console.log(0 || 100); // 100
console.log(0 ?? 100); // 0
```

Use `??` when `0`, `false`, or `""` are valid and should be preserved.

## Optional Chaining (`?.`)

Allows safe access to nested properties without throwing an error if something is nullish.

```js
const user = {
	profile: {
		name: "Parth",
	},
};

console.log(user.profile?.name); // Parth
console.log(user.address?.city); // undefined
```

Optional chaining also works with methods:

```js
const user = {
	greet() {
		return "Hello";
	},
};

console.log(user.greet?.()); // Hello
```

## Spread and Rest (`...`)

### Spread

Expands elements.

```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];
console.log(arr2); // [1, 2, 3, 4]
```

```js
const user = { name: "Parth" };
const updatedUser = { ...user, age: 25 };
console.log(updatedUser); // { name: "Parth", age: 25 }
```

### Rest

Collects remaining elements.

```js
function sum(...numbers) {
	console.log(numbers);
}
sum(1, 2, 3); // [1, 2, 3]
```

## Comma Operator (`,`)

Evaluates multiple expressions and returns the value of the last one.

```js
let result = (1 + 2, 3 + 4);
console.log(result); // 7
```

Rarely used in everyday code.

## Operator Precedence

When multiple operators appear in one expression, JavaScript decides which to evaluate first using precedence.

```js
console.log(2 + 3 * 4); // 14
console.log((2 + 3) * 4); // 20
```

### Common precedence idea

1. parentheses `()`
2. unary operators
3. exponentiation `**`
4. multiplication/division/remainder
5. addition/subtraction
6. comparison
7. equality
8. logical AND
9. logical OR
10. nullish coalescing
11. ternary
12. assignment

Use parentheses whenever clarity is needed.

## Short-Circuiting

Logical operators stop evaluating as soon as the result is known.

### AND short-circuit

```js
console.log(false && someFunction());
```

### OR short-circuit

```js
console.log(true || someFunction());
```

Practical use:

```js
let username = input || "Guest";
```

But remember:

- `||` uses falsy logic
- `??` uses nullish logic

## Best Practices for Using Operators

- Prefer strict equality: `===` and `!==`
- Use `??` for defaults when falsy values are valid
- Use parentheses for readability
- Avoid tricky coercion-based code
- Use ternary only for simple decisions

Example:

```js
let quantity = 0;
let finalQuantity = quantity ?? 1;
```

```js
let total = price * quantity + tax;
```

## Real-World Examples

### Login fallback

```js
let username = enteredName || "Guest";
```

### Safer fallback with nullish

```js
let score = 0;
let displayScore = score ?? "Not available";
console.log(displayScore); // 0
```

### Age eligibility

```js
let age = 20;
let canVote = age >= 18;
console.log(canVote); // true
```

### Form validation

```js
let email = "test@example.com";
let password = "12345678";

if (email !== "" && password.length >= 8) {
	console.log("Valid form");
}
```

### Safe nested access

```js
const user = {
	address: {
		city: "Leicester",
	},
};

console.log(user.address?.city); // Leicester
console.log(user.profile?.bio); // undefined
```

## Most Important Operators to Master First

For strong JavaScript fundamentals, master these first:

1. `=`
2. `+`, `-`, `*`, `/`, `%`
3. `>`, `<`, `>=`, `<=`
4. `===`, `!==`
5. `&&`, `||`, `!`
6. `?:`
7. `??`
8. `?.`

These are the ones you will use constantly.
