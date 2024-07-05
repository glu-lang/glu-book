---
 title: Overloading Operators
 category: Data Types
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

To compare both the names and ages of two instances of Person, we can overload the == operator using the following syntax:

```glu
operator ==(a: Person, b: Person) -> Bool {
    return a.age == b.age && a.name == b.name;
}
```

Here's a step-by-step explanation of this overload:

## 1. Declaration of the Overload Function:

```glu
operator ==(a: Person, b: Person) -> Bool
```
This line declares a new overload function for the == operator. The function takes two arguments, a and b, of type Person and returns a boolean (Bool).

## 2. Implementation of the Function:

```glu
return a.name == b.name && a.age == b.age;
```

This line implements the comparison logic by comparing both the names and ages of the persons. It returns true if both the names and ages of the Person instances are equal, otherwise, it returns false.

## Using the Overload in Code

Once the operator is overloaded, you can compare the names and ages of two instances of Person as follows:

```glu
func main() {
    let emil : Person = {"Emil", 21};
    let jerem : Person = {"Jerem", 20};
    let emilClone : Person = {"Emil", 21};

    if (emil == jerem) {
        std::print("Emil is Jerem??");
    } else {
        std::print("Emil is not Jerem! ");
    }
    if (emil == emilClone) {
        std::print("It's a Clone!!!");
    }
}
```
In this example the following output should be:
```
Emil is not Jerem!
It's a Clone!!!
```

Operator overloading allows you to make your code more natural and easier to read. By using the syntax described above, you can define specific behaviors for common operators (like ==, +, -, etc.) for your custom data types, thus enhancing the flexibility and readability of your code. In this example, we overloaded the == operator to compare both the names and ages of Person instances, making person comparisons more intuitive. A list of all overloadable operators is available at this link: https://glu-lang.org/operators_and_symbols/.
