---
title: Using Comments in Glu
category: The Glu Basics
---

# Using Comments in Glu

Comments are an essential part of any programming language. They help make code more readable and maintainable by providing explanations and notes for future reference. In Glu, you can use single-line comments, multi-line comments, and nested comments.

## Single-Line Comments

Single-line comments start with `//` and extend to the end of the line. They are used for short explanations or notes.

```glu
// This is a single-line comment in Glu
func main() {
    std::print("Hello, World!"); // This prints Hello, World! to the console
}
```

In this example, the comment `// This is a single-line comment in Glu` is ignored by the compiler, and the code on the next line is executed normally. Similarly, the comment `// This prints Hello, World! to the console` explains what the following line of code does.

## Multi-Line Comments

Multi-line comments start with `/*` and end with `*/`. They can span multiple lines and are useful for longer explanations or temporarily disabling blocks of code.

```glu
/*
This is a multi-line comment in Glu.
It can span multiple lines.
*/
func main() {
    std::print("Hello, World!"); /* This is a multi-line comment at the end of a line */
}
```

In this example, the text between `/*` and `*/` is ignored by the compiler, allowing you to write comments that span multiple lines.

## Nested Comments

Glu also supports nested comments, which means you can have comments within comments. This can be useful for debugging or when you want to comment out a section of code that already contains comments.

```glu
/*
This is a multi-line comment.
    /* This is a nested comment inside a multi-line comment. */
This is still part of the outer comment.
*/
func main() {
    std::print("Hello, World!");
}
```

In this example, the nested comment `/* This is a nested comment inside a multi-line comment. */` is within an outer multi-line comment. Both the inner and outer comments are ignored by the compiler.

## Example Program with Comments

Here is an example of a Glu program with various types of comments:

```glu
// This is a single-line comment

/*
This is a multi-line comment.
It provides a more detailed explanation
about the code below.
*/

func main() {
    // Print a greeting to the console
    std::print("Hello, World!");

    /*
    The following code is commented out for debugging purposes.
    std::print("This line is temporarily disabled.");
    */

    /*
    This is a multi-line comment with nested comments.
    /*
    Nested comments are useful for temporarily disabling code blocks
    that already contain comments.
    */
    std::print("This line will still execute.");
    */
}
```

This example demonstrates the use of single-line, multi-line, and nested comments within a Glu program, showcasing how comments can be used effectively to document and debug your code.
