---
title: Variables
category: Common Concepts
---

Here's the revised version of your text:

## The Concept of Variables

One of the most basic programming concepts is variables. Often, a programmer needs to bind a value to a name, but the mutability of this variable is a crucial consideration. Some languages default to making variables either mutable or immutable. Let's explore how and why Glu does not have a default case by distinguishing between these two types of variables.

### The `let` Keyword

First, let's look at how to declare a constant. When a variable is declared using the keyword `let`, its value cannot be changed. To illustrate this, let's write some Glu code:

> This code sample does not compile.
{: .prompt-warning }

```glu
func main() {
    let c: Int = 42;

    c = 21;
}
```

Save and run this Glu program. You should receive a compilation error message indicating a modification of a constant error.

This example highlights how the compiler helps in spotting errors in your programs. While compiler errors might seem frustrating, they indicate that your program isn't yet performing safely as intended; they do not reflect on your programming skills.

It's essential to catch these errors at compile-time when trying to modify a constant value, as this can prevent potential bugs. If one part of the code expects a value to remain constant while another part modifies it, the first part might malfunction. Such bugs can be tricky to diagnose, especially when the value changes intermittently. The Glu compiler ensures that when you declare a value as constant using `let`, it truly remains unchanged, so you don't have to monitor it manually. This guarantees that your code is more reliable and easier to follow.

If you really need to set the `c` variable to `21`, you should use another keyword.

### The `var` Keyword

The second keyword available for creating a non-constant variable is `var`. If you need, for example, a counter, you'll have to use the keyword `var` to declare your variable.

What does the previous example look like using the `var` keyword?

> Greetings, this Glu code will compile!
{: .prompt-tip }

```glu
func main() {
    var c: Int = 42;

    c = 21;
}
```

Now, using the `var` keyword, we're allowed to change the value bound to `c` from `42` to `21`.

Ultimately, deciding whether to use the keyword `let` or `var` is up to you and depends on what you think is clearest in that particular situation.

## Summary

In Glu, there are two keywords for declaring variables:

- `let` for constants
- `var` for non-constants
