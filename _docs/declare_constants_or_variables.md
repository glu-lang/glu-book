---
title: Declare constants or variables
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

This example highlights how the compiler helps in spotting errors in your program.


It's essential to catch these errors at compile-time when trying to modify a constant value, as this can prevent potential bugs.
If one part of the code expects a value to remain constant while another part modifies it, the first part might malfunction.
Such bugs can be tricky to diagnose, especially when the value changes intermittently.
The Glu compiler ensures that when you declare a value as constant using `let`, it truly remains unchanged, so you don't have to monitor it manually.

Now, if you need a mutable variable for exemple a counter, you must use another keyword.

## Declare a non-constant variable

In Glu, the keyword for creating a variable is the `var` keyword.
As said before, if you need a counter for instance, you must use the `var` keyword.

Let's look again the previous example but declaring the `x` variable using the `var` keyword:

> Greetings, this Glu code will compile!
{: .prompt-tip }

```glu
func main() {
    var x: Int = 42;

    x = 21;
}
```

Now, using the `var` keyword, we're allowed to change the value of `x` from `42` to `21`.

## Summary

In Glu, there are two keywords for declaring local symbols:

- `let` for constants
- `var` for non-constants
