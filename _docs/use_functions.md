---
title: Functions
category: Common Concepts
---

You've already encountered the `main` function, the entrypoint of any Glu program, as well as the `func` keyword which is needed to declare a function.

Procedural languages, like Glu, structure programs as a series of functions (also known as procedures) that call one another.
This is why functions are indispensable in Glu programming.

```glu
func main() {
    std::print("Hello World!");
}
```

## Declare a Function

To declare a function, three key components are required:
- The `func` keyword
- The function name
- A list of any necessary parameters

The body of the function is enclosed in curly brackets `{}`, which indicate to the compiler where the function begins and ends.

In the example below, the function prototype of `my_func` specifies the function name and _parameters_: `param1` and `param2`, both of which are of type `Int`.

![Glu Function Declaration Explanation](/assets/img/function_explaining.png)

A function can be declared without being immediately implemented.
In such cases, a semicolon `;` is used instead of curly brackets.
Although functions do not always require _parameters_, the `func` keyword and the function name are always necessary for declaration.

## Function _Parameters_

A function may not always require _parameters_, but what exactly is a _parameter_ ?
Parameters are special variables that appear in a function's signature.
When a function requires _parameters_, they must be provided with concrete values.

In the following version of `my_func`, the _parameters_ `param1` and `param2` are assigned the values `42` and `42`:

```glu
func main() {
    my_func(42, 42);
}

func my_func(param1: Int, param2: Int) {
    std::print(param1 + param2); // This will print 84
}
```

This function will print `84` as `my_func` simply adds `param1` to `param2`.


Since the types of `my_func` _parameters_ are explicitly specified as `Int`, you must provide strictly `Int` typed values.
When declaring a function with _parameters_, the type of each _parameter_ must be explicitly stated.
Note that the `my_func` function is defined after the `main` function in the source code, but it could have been defined before.
Glu does not enforce the order of function definitions, only that they are defined within an accessible scope.

## Functions with Return Values

Functions can return values to the code that calls them.
When declaring a function that returns a value, you must specify the type of the return value after an arrow `->`.
If a function specifies a return value, the `return` keyword must be used to return the desired value.

```glu
func add(a: Int, b: Int) -> Int {
    return a + b;
}

func main() {
    let result = add(3, 5);

    std::print(result); // This will print 8
}
```

In this example, the `add` function takes two parameters, `a` and `b`, both of type `Int`, and returns an `Int`.
The `-> Int` syntax indicates that the function returns an integer. Within the function body, the `return` keyword is used to return the sum of `a` and `b`.
