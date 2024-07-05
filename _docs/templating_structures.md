---
 title: Templating Structures
 category: Templates
---

Templates allow you to define generic structures that can handle different data types without rewriting the structure for each type. This makes your code more reusable and flexible. In our language, the syntax for defining a template structure is straightforward. Here’s how to use template structures.

## Defining a Template Structure

Let's define a template structure Container that can hold any type of data:

```glu
struct Container<T> {
    content: T
}
```

Here's an explanation of the components:

### Template Declaration:

```glu
struct Container<T>
```

This declares a template structure named Container with a generic type T. The type T can be any type (Int, String, another struct, etc.).

### Structure Definition:

```glu
{
    content: T
}
```

This defines the structure with a single field content of type T.

# Using the Template Structure

You can use the template structure to create instances that hold different types of data. Here are a few examples:

### Creating an Instance with an Integer:

```glu
let intContainer : Container<Int> = { 42 };
std::print(intContainer.content); // Output: 42
```

### Creating an Instance with a String:

```glu
let stringContainer : Container<String> = { "Hello, world!" };
std::print(stringContainer.content); // Output: Hello, world!
```

### Creating an Instance with a Custom Type:

Let's assume we have a Person structure:

```glu
struct Person {
    name: String
    age: Int
}
```

We can create a template structure to hold a Person instance:

```glu
let person : Person = { "Alice", 30 };
let personContainer : Container<Person> = { person };
std::print(personContainer.content.name); // Output: Alice
std::print(personContainer.content.age);  // Output: 30
```

## Conclusion:

Template structures enable you to write flexible and reusable code by allowing structures to hold data of any type. By using the syntax described above, you can define and use template structures to handle various types of data, enhancing the flexibility and readability of your code. Template structures are particularly useful when you need to work with different data types in a consistent manner.
