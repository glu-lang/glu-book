---
title: Templating Functions
category: Polymorphism
---

Templated functions (or generic functions) allow you to write functions that can operate on different types of data while maintaining type safety. This makes your code more flexible and reusable. In Glu, the syntax for defining a template function is straightforward. Here’s how you can define and use a template function.

## Definition of a Template Function

Let's define a simple templated function named `id` that returns its input argument:

```glu
func id<T>(a: T) -> T {
    return a;
}
```

Here's an explanation of the components:


```glu
func id<T>
```

This declares a template function named id with a generic type T. The type T can be any type (e.g., Int, String, custom struct, etc.).

```glu
(a: T) -> T
```

The function takes a single parameter a of type T and returns a value of the same type T.

## Function Implementation:

```glu
return a;
````

The function simply returns the input parameter a.

## Using the Template Function

You can use the template function id with different types of data. Here are a few examples:

Using the Function with an Integer:

```glu
let intValue : Int = id<Int>(42)
std::print(intValue) // Output: 42
```

Using the Function with a String:

```glu
let stringValue : String = id<String>("Hello, world!")
std::print(stringValue) // Output: Hello, world!
```

Using the Function with a Custom Type:
Let's assume we have a Person structure:

```glu
struct Person {
    name: String
    age: Int
}


let person : Person = { "Alice", 30 }
let personValue = id<Person>(person)
std::print(personValue.name) // Output: Alice
std::print(personValue.age)  // Output: 30
```

## Example of Using the Template Function in a More Complex Scenario

You can use the id function in a more complex scenario to demonstrate its flexibility and reusability:

```glu
// Generic function
func id<T>(a: T) -> T {
    return a;
}

// Custom type
struct Person {
    name: String
    age: Int
}

// Instances
let intValue : Int = 42
let stringValue : String = "Hello, world!"
let personValue : Person = { "Alice", 30 }

// Using the id function
let newIntValue  id<Int>(intValue)
let newStringValue = id<String>(stringValue)
let newPersonValue = id<Person>(personValue)

// Printing the results
std::print(newIntValue)         // Output: 42
std::print(newStringValue)      // Output: Hello, world!
std::print(newPersonValue.name) // Output: Alice
std::print(newPersonValue.age)  // Output: 30
```

In this example:

The id function is used with an integer, a string, and a custom type (Person).
The results are printed, demonstrating that the function works correctly with different types of data.

## Conclusion

Template functions enable you to write flexible and reusable code by allowing functions to operate on any type of data while maintaining type safety. By using the syntax described above, you can define and use template functions to handle various types of data, enhancing the flexibility and readability of your code. Template functions are particularly useful when you need to perform the same operation on different types of data in a consistent manner.
