---
title: Using Functions
category: Common Concepts
---

You've already encountered the `main` function, the entrypoint of any Glu program, as well as the `func` keyword which is needed to declare a function.

Procedural languages, like Glu, structure programs as a series of functions that call one another.
This is why functions are essential in Glu programming.

```glu
func main() {
    std::print("Hello World!");
}
```

## Declare a Function

To declare a function, four key components are required:
- The `func` keyword
- The function name
- A list of any necessary parameters
- The function’s return type, if applicable

The body of the function is enclosed in curly brackets `{}`, which indicate to the compiler where the function begins and ends.

In the example below, the function prototype of `my_func` specifies the function name and parameters: `param1` and `param2`, both of which are of type `Int`.

![Representation of the differents key components required by a function declaration in Glu: `func my_func(param1: Int, param2: Int);`](/assets/img/function_explaining.png)

A function can be declared without being immediately implemented.
In such cases, a semicolon `;` is used instead of curly brackets.
Although functions do not always require parameters, the `func` keyword and the function name are always necessary for declaration.

## Function Parameters

A function may not always require parameters, but what exactly is a parameter ?
Parameters are special variables that appear in a function's signature.
When a function requires parameters, they must be provided with concrete values.

In the following version of `my_func`, the parameters `param1` and `param2` are assigned the values `42` and `42`:

```glu
func main() {
    my_func(42, 42); // This will print 84
}

func my_func(param1: Int, param2: Int) {
    std::print(param1 + param2);
}
```

This function will print `84` as `my_func` simply adds `param1` to `param2`.

Since the types of `my_func` parameters are explicitly specified as `Int`, you must provide strictly `Int` typed values.
When declaring a function with parameters, the type of each parameter must be explicitly stated.
Note that the `my_func` function is defined after the `main` function in the source code, but it could have been defined before.
Glu does not enforce the order of function definitions, only that they are defined within an accessible scope.

### Optional parameters

Optional parameters can be specified in Glu by specifying default values to any function parameter:

```glu
func main() {
    my_func(2); // This will print 44
}

func my_func(param1: Int, param2: Int = 42) {
    std::print(param1 + param2);
}
```

With this declaration, you do not need to provide `param2` a value, the default one will be used .
You should keep in mind that the order in which your optional parameters are written have his importance.
If a non-optional parameter is asked with an optional one, the optional parameter must always be declared last.

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


Also, note that the `return` keyword doesn't have to be at the end of the function.

```glu
func main() {
    let x: Int = 42;

    if x == 42 {
        return 0;
    }

    return 1;
}
```

### Conclusion

Understanding the structure and syntax of functions is crucial in Glu programming.
Functions enable modular and reusable code, making programs more efficient and easier to manage.


Functions are defined with four key components: the `func` keyword, the function name, parameters, and the return type, with the body enclosed in curly brackets.
Also, parameters are special variables required by some functions, and default values can make those parameters optional.
Functions can also return values, specified by an arrow `->` and the `return` keyword.


The examples provided illustrate how to declare and define functions, specify parameters, and use return values effectively.
Understanding these elements allows you to write modular, efficient, and reusable code in Glu, ensuring your programs are both robust and well-organized.
