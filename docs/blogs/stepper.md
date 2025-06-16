# Reimplementing stepper
Have you ever been in a situation where you have to teach students how to learn programming? One of the key aspects that make programming fun is to visualize what’s going on in a piece of program. A stepper solves this problem.

The stepper is a tool that helps students visualize step-by-step execution of programs. The stepper has been used in CS1101S, one of the introductory programming courses, and it’s been proven to be effective to students. The stepper uses the philosophy of keep reducing and substituting until you cannot do anything about the program. Here are some examples.

- **Simple Expression** `3 + (4 * 7) => 31` <br>
    <small>`3 + (4 * 7) => 3 + 28 => 31`</small>
- **Constant declaration** `const x = 3; x + 1; => 5` <br>
    <small>`const x = 3; x + 1;`  <br>
            `4 + 1;` <br>
            `5; (Terminate)` </small>

For more details, you can refer the implementation details [here](https://github.com/source-academy/js-slang/blob/master/src/tracer/README.md).

However, things start to get a little bit tricky when we substitute function with recursion. The previous version eagerly substitutes functions with recursion which is not great and it’s very tricky to resolve in the previous version. Moreover, after substituting function declaration, the definition of the function is lost. For instance, consider the implementation of factorial.

```JS
const fact = n => n === 1 ? 1 : n * fact(n - 1);
```
What would happen if we call factorial of 5?

``` JS
(n => n === 1 ? 1 : n * fact(n - 1))(5)
```

You can clear see that even though function fact has already been substituted, there should be no fact left in the program, since we cannot recall the definition of fact later on after substitution.

One technique that we use is utilizing $\mu$ term to remember function definition after substitution.

(To be continued)