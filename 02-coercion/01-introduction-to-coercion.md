# Introduction to coercion

- JS changes a value from one type into another type so that an operation can continue.
- This process is called as coercion.
- Coercion isn't magic, it is defined behaviour.
- Without understanding coercion, many JS results look random.
- For eg:

```js
console.log("5" + 1); // "51"
console.log("5" + 1); // 4
if("hi") // runs as valid code
```

- These are not random behaviours.
- They happen because:
  - the operator or language construct needs a certain kind of value.
  - JS converts the input if required.
  - the conversion rule is defined in the official ecmascript specification.

- So because of coercion, JS first looks at what the operation needs, then converts the value if needed.
- There is no single coercion rule as it is context based.
- Different contexts ask for different conversions:
  - `if(...)` wants boolean-like behaviour.
  - `-` wants numeric behaviour.
  - `+` may want string concatenation or numeric addition.
  - `==` has its own special comparison rules.
- This is why coercion could feel confusing at first as it is not just one topic to learn. It is a family of rules defined in the spec which JS uses internally.

## Questions to ask

- When you see something surprising in JS, ask:
  1. What operation is being performed?
  2. What type of value does the operation need?
  3. Will JS convert something to make that operation possible?
