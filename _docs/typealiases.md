---
title: Using Type Aliases
category: Data Types
---

Type aliases are a way to create a new name for an existing type in Glu. They are useful for making complex types more readable and for providing a descriptive name for a type that is used in multiple places.

## Declaring a Type Alias

In Glu, you can declare a type alias using the `typealias` keyword followed by the alias name and the existing type. Here is an example of a type alias declaration:

```glu
typealias Age = Int;
```

In this example, the type alias `Age` is created for the existing type `Int`. You can now use `Age` as a synonym for `Int` in your code:

```glu
let age: Age = 30;
```

Type aliases can be used for any type, including primitive types, custom types, and generic types. Type aliases can also themselves be generic. Here are some examples of type aliases for custom types and generic types:

```glu
typealias Name = String;
struct Point2D<T> { x: T, y: T }
typealias Point = Point2D<Int>;
typealias Callback<T> = *(T) -> Void;
```

In the first example, the type alias `Name` is created for the existing type `String`. In the second example, the type alias `Point` is created for the generic type `Point2D<Int>`. In the third example, the type alias `Callback` is created for a generic function type.

## Using Type Aliases

Type aliases can be used anywhere you would use the original type. They are particularly useful when working with complex types or when you want to provide a more descriptive name for a type. For example, you can use a type alias to define a custom error type:

```glu
typealias Error = String;
```

Now you can use `Error` instead of `String` when working with error messages in your code:

```glu
func handleError(error: Error) {
    std::print("An error occurred: \(error)");
}
```

Type aliases can also be used to simplify the declaration of generic types. For example, you can define a type alias for a dictionary with a specific key and value type:

```glu
typealias StringMap<T> = Dictionary<String, T>;
```

Now you can use `StringMap<Int>` instead of `Dictionary<String, Int>`, which makes the code more concise and readable.

## Conclusion

Type aliases are a powerful feature of Glu that allow you to create descriptive names for existing types and simplify the declaration of complex types. They can improve the readability and maintainability of your code by providing meaningful names for types that are used throughout your program. Type aliases are particularly useful when working with generic types or when you want to define custom types that are easier to understand and use.

Do note that type aliases are purely a compile-time feature and do not introduce any new types or runtime overhead. They are simply a way to provide more descriptive names for existing types in your code. You can not create overloaded functions or operators based on type aliases. If you need to define new types with different behavior, you should use structs instead.

