---
title: Functions
category: Common Concepts
---

One of the most basic programming concepts is variables. Most of the time, the programmer will need to bind a value to a name, but the mutability of this variable is indeed a subject of concern. Regarding this subject, some languages have chosen to make their variables mutable or immutable by default. Let's explore how and why Glu did not make any default case by separating these two types of variables.

Let's dive into the first way of declaring a constant. When a variable is declared using the keyword `let`, you can't change that value. To illustrate this, let's write some glu.

```
func main() {
	let c: Int = 42;

	c = 21;
}
```

Save and run this Glu program. You should receive a compilation error message concerning a modification of constant error.

This example highlights how the compiler aids in spotting errors in your programs. While compiler errors might seem frustrating, they merely indicate that your program isn't yet performing safely as intended; they do not reflect on your programming skills.

It's essential to catch these errors at compile-time when trying to modify an constant value, as this can prevent potential bugs. If one part of the code expects a value to remain constant while another part modifies it, the first part might malfunction. Such bugs can be tricky to diagnose, especially when the value changes intermittently. The Glu compiler ensures that when you declare a value as constant using `let`, it truly remains unchanged, so you don't have to monitor it manually. This guarantees that your code is more reliable and easier to follow.

