---
title: Declaring Constants and Variables
category: Common Concepts
---

One of the most basic programming concepts is the notion of variable. There is at least one variable in every program and they have many use cases.
But depending on the context, it is important to keep control on the mutability of a value.

## Declare a constant in Glu

If you need to bind a name to a value, you should declare it as a constant.
In Glu, you can declare a constant value using the keyword `let`. To illustrate this, let's write some Glu code:


```glu
func main() {
    let x: Int = 42;
}
```

In this code, the declared value `x` is a constant.


Note that you can't change the value of `x` otherwise the compiler will throw an error.

> Be careful, this code sample does not compile.
{: .prompt-warning }

```glu
func main() {
    let c: Int = 42;

    c = 21;
}
```

If you save and run this Glu program, you should receive a compilation error message indicating an attempt to modify a constant value.

## Declare a variable

In Glu, the keyword for creating a variable is the `var` keyword.
As a `let` constant cannot be modified, if you need a mutable variable, you must use the `var` keyword.

Let's look again the previous example but declaring the `x` variable using the `var` keyword:


```glu
func main() {
    var x: Int = 42;

    x = 21;
}
```

Now, using the `var` keyword, we're allowed to change the value of `x` from `42` to `21`.

## Declaring global constants and variables

You can also declare global constants and variables outside of any function.

```glu
let globalConst: Int = 100;

var globalVar: Int = 200;
```

These global constants and variables can be accessed from any function within the same module, or from other modules if they are marked as `public`.

The initialization of global constants and variables is done when the variable is first accessed:

```glu
let globalConst: Int = initGlobalConst();

func initGlobalConst() -> Int {
    std::print("Initializing global constant");
    return 100;
}

func main() {
    std::print(globalConst); // This will print "Initializing global constant" followed by "100"
    std::print(globalConst); // This will only print "100" as the constant is already initialized
}
```

This behavior ensures that the initialization code is only executed once, the first time the global constant or variable is accessed. It can be useful for lazy initialization of resources that are expensive to create.

For `let` constants, this means that the initialization code will not be executed until the constant is actually used, and after that, it cannot change again. For `var` variables, the initialization code will be executed the first time the variable is accessed, but the variable can be modified later. If the first use of the variable is a write, the initialization code will still be executed, but the initial value will be discarded.

If this behavior is not desired, you can use the `@eager` attribute to force the initialization of the global constant or variable at program startup:

```glu
@eager let eagerGlobalConst: Int = initEagerGlobalConst();

func initEagerGlobalConst() -> Int {
    std::print("Initializing eager global constant");
    return 200;
}

// The line "Initializing eager global constant" will be printed before main is called
func main() {
    std::print(eagerGlobalConst); // This will only print "200" as the constant is already initialized
}
```

Note that if the value is never accessed, the initialization code will still be executed at program startup.

Finally, you can use the `@constexpr` attribute to declare a global constant that is guaranteed to be a compile-time constant. This means that the value of the constant must be known at compile time and cannot be the result of a function call or any other runtime computation.

```glu
@constexpr let compileTimeConst: Int = 42;
```

By default, all `let` constants may be optimized as compile-time constants if the compiler can determine their value at compile time. However, using the `@constexpr` attribute explicitly indicates that the constant is intended to be a compile-time constant. If the value cannot be determined at compile time, the compiler will throw an error.

Both `@eager` and `@constexpr` attributes can be used on global `let` constants and `var` variables. They cannot be combined, as they have opposite semantics.

The `@constexpr` attribute can also be used on local variables and constants within a function, but the `@eager` attribute can only be used on global variables and constants.

## Summary

In summary, use the `let` keyword to declare constants and the `var` keyword to declare mutable variables. You can also declare global constants and variables outside of any function, and use attributes like `@eager` and `@constexpr` to control their initialization behavior.
