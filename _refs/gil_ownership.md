---
title: Ownership in GIL
category: The Architecture
---

In Ownership-SSA GIL, ownership semantics are explicitly represented through specific instructions and conventions. This explicit representation allows the compiler to enforce ownership rules and optimize memory management effectively.

You can view the OSSA GIL representation of a Glu program by compiling it with the `--print-gilgen` flag.

The ownership-specific instructions are removed by later GIL passes, which translate the ownership semantics into standard GIL instructions. This is done to simplify the GIL representation and make it easier to optimize and translate to LLVM IR.

## Overview

Here is an example of a simple Glu function:

```glu
func sum() {
    var x: Int = 10;
    x += 20;
    return x;
}
```

And its raw GIL representation:

```gil
gil @sum : $() -> Int {
entry:
    %0 = alloca $Int
    %1 = integer_literal $Int, 10
    store %1 : $Int, %0 : $*Int
    $2 = mutable_borrow %0 : $*Int
    %3 = load %2 : $*Int
    %4 = integer_literal $Int, 20
    %5 = call @+ : $(Int, Int) -> Int, %3 : $Int, %4 : $Int
    store %5 : $Int, %2 : $*Int
    end_borrow %2 : $*Int
    %6 = immutable_borrow %0 : $*Int
    %7 = load %6 : $*Int
    return %7 : $Int
    end_borrow %6 : $*Int
}
```

And here is the same GIL after ownership instructions have been removed:

```gil
gil @sum : $() -> Int {
entry:
    %0 = alloca $Int
    %1 = integer_literal $Int, 10
    store %1 : $Int, %0 : $*Int
    %3 = load %0 : $*Int
    %4 = integer_literal $Int, 20
    %5 = call @+ : $(Int, Int) -> Int, %3 : $Int, %4 : $Int
    store %5 : $Int, %0 : $*Int
    %6 = load %0 : $*Int
    return %6 : $Int
}
```

## Borrow Instructions

In the OSSA GIL representation, you can see the use of `mutable_borrow` and `immutable_borrow` instructions. These instructions are used to create temporary references to variables, allowing safe access to their values without transferring ownership. The `mutable_borrow` instruction allows for mutable access to a variable, while the `immutable_borrow` instruction allows for read-only access.

The `end_borrow` instruction is used to indicate the end of a borrow, ensuring that the borrowed reference is no longer used after this point. This avoids multiple mutable borrows or mutable and immutable borrows of the same variable at the same time, which would violate ownership rules.

## Move Semantics

In the OSSA GIL representation, move semantics are represented by the use of the `move` instruction. When a variable is moved, its ownership is transferred to another variable or function, and the original variable is no longer valid. This is enforced by the compiler, which ensures that moved variables are not used after the move.

Here is an example of a function that takes ownership of a unique pointer:

```glu
func takeOwnership(ptr: *unique Int) {
    drop(ptr);
}
```

And its OSSA GIL representation:

```gil
gil @takeOwnership : $(*unique Int) -> () {
entry(%0: $*unique Int):
    %1 = move %0 : $*unique Int
    drop %1 : $*unique Int
    return
}
```

In this example, the `move` instruction transfers ownership of the unique pointer `ptr` to the variable `%1`, which is then passed to the `drop` function. After the move, the original variable `%0` is no longer valid and cannot be used.
The `drop` instruction is used to drop the ownership of the value. For unique pointers, this means deallocating the memory. After the `drop` instruction, the variable `%1` is no longer valid either.

## Copy Semantics

In the OSSA GIL representation, copy semantics are represented by the use of the `copy` instruction. When a variable is copied, a new instance of the value is created, and both the original and the copied variables are valid. This is typically used for types that implement copy semantics, such as integers and booleans. Here is an example of a function that copies an integer:
```glu
func copyValue(x: Int) -> Int {
    let y: Int = x;
    return y;
}
```

And its OSSA GIL representation:

```gil
gil @copyValue : $(Int) -> Int {
entry(%0: $Int):
    %1 = copy %0 : $Int
    return %1 : $Int
}
```

In this example, the `copy` instruction creates a new instance of the integer value within `x`, which is then returned. Both the original variable `%0` and the copied variable `%1` are valid and can be used independently.

## Ownership Instruction List

Here is a list of the ownership-specific instructions used in OSSA GIL:

- `move` - transfers ownership of a value to another value, making the original value invalid. this is removed during canonicalization (as the original value and the moved value are the same in GIL).
- `copy` - creates a new instance of a value, allowing both the original and copied values to be valid. this is removed during canonicalization for trivial types. Otherwise, it is translated to a call to the copy function.
- `mutable_borrow` - creates a temporary mutable reference to a value, allowing for safe mutation without transferring ownership. Between the borrow and the corresponding `end_borrow`, the original value cannot be accessed.
- `immutable_borrow` - creates a temporary immutable reference to a value, allowing for safe read-only access without transferring ownership. Between the borrow and the corresponding `end_borrow`, the original value can still be accessed, but not mutated.
- `end_borrow` - indicates the end of a borrow, ensuring that the borrowed reference is no longer used after this point. Borrows are removed during canonicalization.
- `drop` - drops the ownership of a value. After the `drop` instruction, the variable is no longer valid. This is replaced with a call to the `drop` function for the type if it is defined, otherwise, it is removed during canonicalization for trivial types.

## Conclusion

The explicit representation of ownership semantics in OSSA GIL allows the compiler to enforce ownership rules and optimize memory management effectively. By using specific instructions for borrowing, moving, copying, and dropping values, the compiler can ensure that memory is managed safely and efficiently throughout the program's execution.
