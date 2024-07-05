---
title: Future Directions for Glu
layout: post
---

The Glu Programming Language is a work in progress. This document outlines potential future directions for the language.

## Initializer and Function Argument Labels

Glu currently does not support argument labels for initializers and functions. This feature could be added to improve the readability of code.

This would be an optional feature, as it would not be required to provide argument labels when calling functions or initializers.

It would look like this:

```glu
struct Person {
    name: String,
    age: Int
}

let alice: Person = { name: "Alice", age: 25 };

func greet(person: Person) {
    std::print("Hello, " + person.name + "!");
}

greet(person: alice);
```

This would be useful when taking booleans as arguments, as it is not always clear what the boolean represents. It would be entirely optional, but when used, it must match the name of the parameter in the function definition.

## String Interpolation

String interpolation is a feature that allows you to embed variables and expressions within a string literal. This feature could be added to Glu to make it easier to construct strings.

It would use a syntax similar to Swift:

```glu
let name: String = "Alice";
let greeting: String = "Hello, \(name)!";
std::assert(greeting == "Hello, Alice!");
```

This feature would make it easier to construct strings with dynamic content and would improve the readability of code.

## Concepts

Concepts are a feature in C++ that allow you to specify requirements on template arguments. This feature could be added to Glu to improve the clarity of template functions.

Concepts would allow you to specify constraints on template arguments, making it easier to understand the requirements of a function.

It could look like this:

```glu
concept Iterable<T> {
    func begin(self: Self) -> Iterator<T>;
    func end(self: Self) -> Iterator<T>;
}
concept Iterator<T> {
    operator .*(self: Self) -> T;
    operator ++(self: Self);
}
concept Addable {
    operator +(self: Self, other: Self) -> Self;
}

func sum<T: Iterable<Int>, N: Addable>(iterable: T) -> N {
    var total: N = {}; // Default initialization
    for element in iterable {
        total = total + element;
    }
    return total;
}

let exampleA: Int   = sum({1, 2, 3} as Int[3]); // 6
let exampleB: Float = sum({0.5, 0.25} as Float[2]); // 0.75
```

Concepts define the functions and operators that must be implemented by a type to satisfy the requirements of the concept. This makes it easier to understand the constraints on template arguments and improves the readability of code.

## Conclusion

These are just a few potential future directions for the Glu Programming Language. The language is still in development, and new features and improvements are being considered. If you have any ideas or suggestions for the language, please feel free to share them in the comments section below. Your feedback is valuable and will help shape the future of Glu. Thank you for your support! 