---
title: Understanding Primitives
category: Data Types
---

Primitive types refer to the simplest and most basic data types in programming that are built into a language and are not 
composed of other types. They represent the most fundamental forms of data in the language.

## Primitive Types

### Int (Signed Integers)
Represent whole numbers (positive, negative, or zero).

The size and range vary depending on the exact type:

| Type  | Bit Size | Range |
|-------|----------|-------|
| Int   | Depends  |       |
| Int8  | 8 bits   | -128 to 127 |
| Int16 | 16 bits  | -32,768 to 32,767 |
| Int32 | 32 bits  | -2,147,483,648 to 2,147,483,647 |
| Int64 | 64 bits  | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |

### UInt (Unsigned Integers)
Represent whole numbers but only non-negative values (no negative numbers).
The size and range also depend on the number of bits used.

| Type   | Bit Size | Range                           |
|--------|----------|---------------------------------|
| UInt   | Depends  |                                 |
| UInt8  | 8 bits   | 0 to 255                        |
| UInt16 | 16 bits  | 0 to 65,535                     |
| UInt32 | 32 bits  | 0 to 4,294,967,295              |
| UInt64 | 64 bits  | 0 to 18,446,744,073,709,551,615 |

### Char
Represents a single character, such as a letter, digit, or symbol. It represents one byte of unicode.

Example:
```glu
let character: Char = 'a';
```

### String
Represents a sequence of characters or text. It's used to store and manipulate text data.

Example:
```glu
let myText: String = "Hello Glu!";
```

### Float / Double
Represent real numbers (numbers with decimal points), following the IEEE 754 standard for floating-point arithmetic, which defines how numbers are stored and calculated.


| Type   | Bit Size | Precision                           |
|--------|----------|---------------------------------|
|Float|32 bits	|~7 decimal places	|
|Double|64 bits	|~15-16 decimal places		|


### Bool
A Boolean type that represents logical values. It can only be either true or false.

Example:
```glu
let hasBrain: Bool = false;
```