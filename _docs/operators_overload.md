---
 title: Glu language Keywords
 category: Appendix
---

Operator overloading allows you to redefine the behavior of operators for specific data types. This makes your code more intuitive and readable. In our language, the syntax for overloading an operator is straightforward.

# Example of Overloading the == Operator
Here’s an example of overloading the equality operator (==) for a structure.

Let's assume we have a Person structure defined as follows:
```glu
struct Person {
    name: String
    age: Int
}
```

To compare two instances of Person, we can overload the == operator using the following syntax:
```glu
operator ==(a: Person, b: Person) -> Bool {
    return a.age == b.age && a.name == b.name;
}
```
