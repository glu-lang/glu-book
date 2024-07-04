---
title: Control Flow
category: Common Concepts
---

Control flow is an important concept in programming. It allows you to control the flow of your program by making decisions and repeating actions. Without control flow, your program would execute in a linear fashion, which would greatly limit its capabilities.

In this chapter, we will explore the different control flow structures available in Glu, such as conditional statements and loops. Understanding these structures will help you write more complex and powerful programs.

## Conditions

Conditional statements allow you to execute different blocks of code based on certain conditions. The most common conditional statement is the `if` statement.

### The `if`, `else`, and `else if` Statements

The `if` statement allows you to execute a block of code if a condition is true. Here is an example:

```glu
let x: Int = 10;

if x > 5 {
    std::print("x is greater than 5");
}
```

In this example, the code inside the `if` block will only be executed if the condition `x > 5` is true, which is the case here.

You can also add an `else` block to execute code when the condition is false:

```glu
let x: Int = 3;

if x > 5 {
    std::print("x is greater than 5");
} else {
    std::print("x is less than or equal to 5");
}
```

In this case, the code inside the `else` block will be executed because the condition `x > 5` is false.

You can also chain multiple conditions using `else if`:

```glu
let age: Int = 18;

if age < 18 {
    std::print("You are a minor");
} else if age < 21 {
    std::print("You are a young adult");
} else {
    std::print("You are an adult");
}
```

### The Ternary Operator

The ternary operator is a shorthand way of writing an `if` statement. It has the following syntax:

```glu
let result: Int = condition ? value_if_true : value_if_false;
```

Here is an example:

```glu
let x: Int = 10;
std::print(x > 5 ? "x is greater than 5" : "x is less than or equal to 5");
```

This code is equivalent to the following `if` statement:

```glu
let x: Int = 10;
var message: String;
if x > 5 {
    message = "x is greater than 5";
} else {
    message = "x is less than or equal to 5";
}
std::print(message);
```

## Loops

Loops allow you to repeat a block of code multiple times. There are two main types of loops in Glu: `for` loops and `while` loops.

### The `while` Loop

The `while` loop repeats a block of code as long as a condition is true. Here is an example:

```glu
let count: Int = 0;

while count <= 5 {
    std::print(count);
    count = count + 1;
}
```

In this example, the loop will print the numbers from 0 to 5, inclusive.

The `while` loop is useful when you don't know in advance how many times you need to repeat a block of code. In this case, a `for` loop might be more appropriate.

### The `for` Loop

The `for` loop repeats a block of code a fixed number of times. Here is an example:

```glu
for i in 0...5 {
    std::print(i);
}
```

This code is equivalent to the `while` loop example above. The `for` loop is more concise and easier to read when you know in advance how many times you need to repeat a block of code.

Here, `i` is a variable that takes on the values from 0 to 5, inclusive. The `...` operator is used to create a range of values. The `for` loop will iterate over each value in the range and execute the block of code.

You can also use the `..<` operator to create a range that excludes the upper bound:

```glu
for i in 0..<5 {
    std::print(i);
}
```

In this case, the loop will print the numbers from 0 to 4.

There are other types of ranges, such as the infinite range `0...`, which starts at 0 and goes to infinity:

```glu
for i in 0... {
    std::print(i);
}
```

This infinite loop will print the numbers from 0 to the maximum value of an `Int`.

For loops can also iterate over arrays and other collections, not just ranges:

```glu
let numbers: [Int] = { 1, 2, 3, 4, 5 };

for n in numbers {
    std::print(n);
}
```

In this example, the loop will print the numbers from the `numbers` array: 1, 2, 3, 4, and 5.

### The `break` and `continue` Statements

Within a loop, you can use the `break` statement to exit the loop early:

```glu
for i in 0... {
    if i > 5 {
        break;
    }
    std::print(i);
}
```

In this example, the loop will print the numbers from 0 to 5 and then break out of the loop.

You can also use the `continue` statement to skip the rest of the current iteration and continue with the next one:

```glu
for i in 0...5 {
    if i % 2 == 0 {
        continue;
    }
    std::print(i);
}
```

In this example, even numbers will be skipped, and only odd numbers between 0 and 5 inclusive will be printed: 1, 3, and 5.

### Nested Loops

You can nest loops inside each other to create more complex patterns. Here is an example of a nested loop:

```glu
for i in 0...2 {
    for j in 0...2 {
        std::printout("*");
    }
    std::print("");
}
```

This code will print the following pattern:

```
***
***
***
```

In this example, the outer loop iterates over the rows, and the inner loop iterates over the columns. The `std::printout` function is used to print characters without a newline at the end, and `std::print` is used to print a newline at the end of each row.

## Recursion

Recursion is an alternative to loops, where code can be repeated by calling a function from within itself. Here is an example of a recursive function that prints a number by printing each digit separately:

```glu
func printNumber(n: Int) {
    if n < 10 {
        std::printout(n);
    } else {
        printNumber(n / 10);
        std::printout(n % 10);
    }
}

func main() {
    printNumber(123);
}
```

In this example, the `printNumber` function prints the number `123` by recursively calling itself with the number divided by 10 until the number is less than 10. Here is what happens at each step:

- `main` calls `printNumber(123)`.
  - `printNumber(123)` calls `printNumber(123 / 10)` which is `printNumber(12)`.
    - `printNumber(12)` calls `printNumber(12 / 10)` which is `printNumber(1)`.
      - `printNumber(1)` prints `1` as it is less than 10.
    - `printNumber(12)` prints `12 % 10` which is `2`.
  - `printNumber(123)` prints `123 % 10` which is `3`.

This very simple function demonstrates the power of recursion in solving seemingly complex problems. Recursion is a powerful tool that can be difficult to understand at first but can lead to elegant and concise solutions to many problems. However, it can also be less efficient than using loops, so it should be used judiciously.

When using recursion, always make sure that the base case (the condition that stops the recursion) is always reached, to avoid infinite recursion, which can lead to a stack overflow and crash your program. In the example above, the base case is when `n < 10`, in which case the function does not call itself recursively.

## Conclusion

Control flow structures are essential for writing complex programs. By using conditional statements, loops, and recursion, you can create programs that make decisions, repeat actions, and solve problems in an elegant and efficient way. Understanding these structures will help you become a better programmer and write more powerful and flexible code.
