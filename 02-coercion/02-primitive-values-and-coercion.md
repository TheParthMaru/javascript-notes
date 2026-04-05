# Primitive & Non-Primitive Values

- For coercion, the most important split is:
  - primitive values
  - objects
- As per the current ECMAScript spec, below are the primitve values:
  - `number`
  - `bigint`
  - `string`
  - `boolean`
  - `undefined`
  - `null`
  - `symbol`

- A primitive is a value that is not an object.

- An object is different from a primitive.
- Objects are things like:
  - plain objects `{}`
  - arrays `[]`
  - functions
  - dates
  - wrapper objects like `new String("abc")`

- When JavaScript needs a value for an operation:
  - if the value is already a primitive, coercion may be more direct.
  - if the value is an object, JavaScript often has to first turn it into a primitive

## Exception of an object

- `null` value is a primitive value but the below example says otherwise:

```js
console.log(typeof null); // object
```

- This is a weird exception that exists in JS for a very long time.
