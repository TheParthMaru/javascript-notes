# What do you mean by scope?

- Scope means the visibility of variables and functions in a JS code.

## Compiled vs interpreted language

- A compiled language is typically translated into machine code before the program runs.
- An interpreted language is typically read and executed during the runtime, instruction by instruction.
- But in the modern programming languages, this distinction is not perfectly black and white.
- Modern language like JavaScript has an engine like v8 which does the following:
  - parse the code.
  - create internal representations.
  - interpret some code.
  - JIT-compile frequently used parts for performance.
- So JS is commonly called as an interpreted language, but modern engines are more advanced than a basic interpreter.

## Real-world difference

### Compiled languages often:

- catch many errors before running
- produce standalone binaries
- can be very fast
- are closer to the machine

### Interpreted languages often:

- are easier to run quickly during development
- rely on a runtime/interpreter/VM
- can feel more flexible
- may trade some raw performance for developer speed

- But again, these are trends, not strict rules.

## JS not being a interpreted language

```js
console.log("Hello");

function foo() {
    console....log("world");
}

console.log("hello world");
```

- If JS was an interpreted language, then the above could would have executed without any errors.
- But instead, the whole script fails.
- This happens because the JS engine does not execute code in a purely line-by-line naive way.
- Before execution, the engine first does an initial processing phase over the script.
- That phase includes things like:
  - parsing the code
  - checking whether the syntax is valid
  - building an internal representation of the code
- The above code contains a syntax error which means that the code is invalid and the engine cannot even finish preparing the program for execution.
- A better mental model can be as follows:

```
Source code
   ↓
Parse / syntax check
   ↓
Internal representation / compilation steps
   ↓
Execution
```

- If parsing fails, execution never starts.
- Also, this does not mean that JS is fully compiled like C.
- It means JavaScript engines usually have multiple phases, such as:
  - parsing
  - early error checking
  - compilation to bytecode/internal instructions
  - execution
  - possible JIT optimization later
