---
title: Templating Functions
category: Templates
---

Template functions (or generic functions) allow you to write functions that can operate on different types of data while maintaining type safety. This makes your code more flexible and reusable. In our language, the syntax for defining a template function is straightforward. Here’s how you can define and use a template function.

## Definition of a Template Function

Let's define a simple template function called templatedPrint that returns its input argument:

```glu
func templatedPrint<T>(a: T) -> T {
    std::print(a);
    return a;
}
```

Here's an explanation of the components:

## Template Declaration:

```glu
func templatedPrint<T>
```

This declares a template function named templatedPrint with a generic type T. The type T can be any type (e.g., Int, String, custom struct, etc.).

Function Parameters and Return Type:

```glu
(a: T) -> T
```

The function takes a single parameter a of type T and returns a value of the same type T.

## Function Implementation:

```glu
return a;
````

The function simply returns the input parameter a.

Using the Template Function

You can use the template function templatedPrint with different types of data. Here are a few examples:

### Using the Function with an Integer:

```glu
let intValue = templatedPrint(42)
print(intValue) // Output: 42
```

### Using the Function with a String:

```glu
let stringValue = templatedPrint("Hello, world!")
print(stringValue) // Output: Hello, world!
```

### Using the Function with a Custom Type:
Let's assume we have a Person structure:

```glu
struct Person {
    name: String
    age: Int
}
```

let person = Person(name: "Alice", age: 30)
let personValue = templatedPrint(person)
print(personValue.name) // Output: Alice
print(personValue.age)  // Output: 30
Example of Using the Template Function in a More Complex Scenario

You can use the templatedPrint function in a more complex scenario to demonstrate its flexibility and reusability:

```glu
// Generic function
func templatedPrint<T>(a: T) -> T {
    return a;
}

// Custom type
struct Person {
    name: String
    age: Int
}

// Instances
let intValue = 42
let stringValue = "Hello, world!"
let personValue = Person(name: "Alice", age: 30)

// Using the templatedPrint function
let newIntValue = templatedPrint(intValue)
let newStringValue = templatedPrint(stringValue)
let newPersonValue = templatedPrint(personValue)

// Printing the results
print(newIntValue)         // Output: 42
print(newStringValue)      // Output: Hello, world!
print(newPersonValue.name) // Output: Alice
print(newPersonValue.age)  // Output: 30
```

In this example:

The templatedPrint function is used with an integer, a string, and a custom type (Person).
The results are printed, demonstrating that the function works correctly with different types of data.

## Conclusion

Template functions enable you to write flexible and reusable code by allowing functions to operate on any type of data while maintaining type safety. By using the syntax described above, you can define and use template functions to handle various types of data, enhancing the flexibility and readability of your code. Template functions are particularly useful when you need to perform the same operation on different types of data in a consistent manner.
