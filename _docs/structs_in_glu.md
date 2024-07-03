---
title: Using Structures in Glu
category: Structure related data using Structs
---

Structures are a way to group related data together in Glu. They are similar to classes in other languages but are typically used for simpler data structures. In this section, we will cover how to declare and use structures in Glu.

## Declaring a Structure

A structure in Glu is declared using the `struct` keyword followed by the structure's name and its fields. Each field has a name and a type. Here are some examples of structure declarations:

```glu
struct Person {
    name: String,
    age: Int,
}
```

In this example, the structure `Person` has two fields: name of type `String` and age of type `Int`. You can also provide default values for the fields:

```glu
struct Person {
    name: String = "Unknown",
    age: Int = 0,
}
```

In this example, the fields name and age are initialized to `Unknown` and `0` respectively.

The last comma in the field list is optional, so you can write the following:

```glu
struct Person {
    name: String,
    age: Int
}
```

## Declaring an Instance of a Structure

You can declare a variable that contains a structure and initialize it with or without default values.

```glu
struct Person {
    name: String = "Unknown",
    age: Int = 0
}

func main() {
    let person1: Person = {}; //  initialisation with default values
    let person2: Person = { "Alice", 25 }; // initialisation with custom values
    let person3: Person = { "Bob" }; // initialisation with custom name but with default age
}
```

## Accessing Fields of a Structure

You can access the fields of a structure using the dot operator (`.`). For example:

```glu
struct Person {
    name: String,
    age: Int
}

func main() {
    let person: Person = { "Alice", 25 };
    std::print(person.name); // prints "Alice"
    std::print(person.age); // prints 25
}
```

## Packing and Unpacking Structures

Glu allows you to pack structures for memory efficiency using the `@packed` attribute. This feature exists in other programming languages and can be useful for certain applications where memory usage is critical.

```glu
@packed struct Data {
    a: Int8,
    b: Int64,
}
```

In this example, the structure `Data` is packed, meaning its fields are laid out in memory without padding. Let's compare the memory layout before and after using the `@packed` attribute.

Without `@packed` attribute:

```glu
struct Data {
    a: Int8,
    b: Int64,
}
```

Without the @packed attribute, the compiler may add padding between the fields to align them in memory, improving access speed but using more memory. For example, an `Int8` field followed by an `Int64` field might look like this in memory:

```
| a (1 byte) | padding (7 bytes) | b (8 bytes) |
```

This layout ensures that `b` is aligned to an 8-byte boundary, but it uses a total of 16 bytes (1 + 7 + 8).

With `@packed` attribute:

```glu
@packed struct Data {
    a: Int8,
    b: Int64,
}
```

When the `@packed` attribute is used, no padding is added, and the fields are laid out consecutively in memory:

```
| a (1 byte) | b (8 bytes) |
```

This layout uses only 9 bytes (1 + 8), saving memory. However, accessing `b` may be slower because it is not aligned to an 8-byte boundary.

Using the `@packed` attribute allows you to gain memory efficiency at the cost of execution speed. This trade-off should be considered when deciding whether to pack a structure.

## Overriding Operators for Structures

Glu supports operator overloading, allowing you to define custom behavior for operators when used with your structures. Here is an example of overriding the equality operator `==` for the `Person` structure:

```glu
operator ==(a: Person, b: Person) -> Bool {
    return a.name == b.name && a.age == b.age;
}
```

In this example, the `==` operator is overridden to compare the fields name and age of two `Person` structures.

## Example Program with Structures

Here is a complete example program that demonstrates the use of structures in Glu:

```glu
// Declare a structure with default values
struct Person {
    name: String = "Unknown",
    age: Int = 0
}

// Override the equality operator for the Person structure
operator ==(a: Person, b: Person) -> Bool {
    return a.name == b.name && a.age == b.age;
}

func main() {
    let person1: Person = {}; // initialisation with default values
    let person2: Person = { "Bob", 30 };

    std::print(person1.name); // prints "Unknown"
    std::print(person2.age); // prints 30

    if person1 == person2 {
        std::print("The two persons are the same.");
    } else {
        std::print("The two persons are different.");
    }
}
```

In this example, we declare a structure `Person` with default values, override the `==` operator for the `Person` structure, and use the structure in the main function to create two instances of `Person` and compare them.

By using structures, you can organize your data more effectively and make your Glu programs more readable and maintainable.
