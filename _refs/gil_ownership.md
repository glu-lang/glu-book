---
title: Ownership in GIL
category: The Architecture
---

In Ownership-SSA GIL, ownership semantics are explicitly represented through specific instructions and conventions. This explicit representation allows the compiler to enforce ownership rules and optimize memory management effectively.

You can view the OSSA GIL representation of a Glu program by compiling it with the `--print-ossa-gil` flag to see after ossa passes are run, or by using the `--print-gilgen` flag to see before any passes are run.

The ownership-specific instructions are removed by a later GIL pass (canonicalization), which translate the ownership semantics into standard GIL instructions. This is done to simplify the GIL representation and make it easier to optimize and translate to LLVM IR. You can see the GIL after all passes, including canonicalization, by using the `--print-gil` flag.

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
    store [init] %1 : $Int, %0 : $*Int
    %3 = load [copy] %0 : $*Int
    %4 = integer_literal $Int, 20
    %5 = call @+ : $(Int, Int) -> Int, %3 : $Int, %4 : $Int
    store [set] %5 : $Int, %0 : $*Int
    %7 = load [take] %0 : $*Int
    return %7 : $Int
}
```

## Borrow Instructions

In the OSSA GIL representation, you can see the use of `mutable_borrow` and `immutable_borrow` instructions. These instructions are used to create temporary references to variables, allowing safe access to their values without transferring ownership. The `mutable_borrow` instruction allows for mutable access to a variable, while the `immutable_borrow` instruction allows for read-only access.

The `end_borrow` instruction is used to indicate the end of a borrow, ensuring that the borrowed reference is no longer used after this point. This avoids multiple mutable borrows or mutable and immutable borrows of the same variable at the same time, which would violate ownership rules.

## Move Semantics

In the OSSA GIL representation, values are moved rather than copied. When a variable is moved, its ownership is transferred to another binding, and the original binding is no longer valid. This is enforced by the compiler, which ensures that moved variables are not used after the move.

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
    drop %0 : $*unique Int
    return %<empty>
}
```

In this example, the unique pointer `ptr` is owned by the function `takeOwnership`, which then passes it to the `drop` instruction. After the move, the original binding `%0` is no longer valid and cannot be used.
The `drop` instruction is used to drop the ownership of the value. For unique pointers, this means deallocating the memory. After the `drop` instruction, the variable `ptr` is no longer valid either.

## Copy Semantics

In the OSSA GIL representation, copy semantics are represented by the use of the `copy` instruction. When a variable is copied, a new instance of the value is created, and both the original and the copied variables are valid. This is typically used for types that implement copy semantics, which is the default. Here is an example of a function that copies an integer:
```glu
func copyValue(x: Int) -> Int {
    let y: Int = x;
    return y;
}
```

And a possible OSSA GIL representation:

```gil
gil @copyValue : $(Int) -> Int {
entry(%0: $Int):
    %1 = copy %0 : $Int
    return %1 : $Int
}
```

In this example, the `copy` instruction creates a new instance of the integer value within `x`, which is then returned. Both the original variable `%0` and the copied variable `%1` are valid and can be used independently. Note that `copy` instructions aren't actually generated for trivial types like `Int` and `Bool`, as they are not needed. The example above is for illustrative purposes.

## Loads and Stores

In OSSA GIL, the `load` and `store` instructions are used to access and modify the values of variables allocated on the stack. The `load` instruction can have different ownership qualifiers, such as `take` or `copy`, which indicate how the value is being accessed. The `store` instruction can also have different qualifiers, such as `init` or `set`, which indicate how the value is being modified.

By default, a slot allocated on the stack by an `alloca` instruction is uninitialized.

The load instruction can only be called on an initialized stack slot. It can have the following qualifiers:
- `take` - indicates that the value is being moved from the stack, transferring ownership to the new binding. After a `take` load, the original binding is left uninitialized and cannot be used.
- `copy` - indicates that the value is being copied from the stack, creating a new instance of the value. Both the original and copied values are valid.

The store instruction can have the following qualifiers:
- `init` - indicates that the value is being initialized in the stack slot. This can only be used on an uninitialized stack slot.
- `set` - indicates that the value is being updated in the stack slot. This can only be used on an initialized stack slot. The previous value is dropped (calling the `drop` function if it is defined for the type) before storing the new value.

At the end of a function, all stack slots must be uninitialized, ensuring that all values have been properly moved out of stack slots and dropped if necessary. The end of a function is where the function returns, through a `return` instruction. There can be multiple return instructions in a function, and all of them must ensure that all stack slots are uninitialized.

## Ownership Instruction List

Here is a list of the ownership-specific instructions used in OSSA GIL:

- `copy` - creates a new instance of a value, allowing both the original and copied values to be valid. this is removed during canonicalization for trivial types. Otherwise, it is translated to a call to the copy function.
- `mutable_borrow` - creates a temporary mutable reference to a value, allowing for safe mutation without transferring ownership. Between the borrow and the corresponding `end_borrow`, the original value cannot be accessed.
- `immutable_borrow` - creates a temporary immutable reference to a value, allowing for safe read-only access without transferring ownership. Between the borrow and the corresponding `end_borrow`, the original value can still be accessed, but not mutated.
- `end_borrow` - indicates the end of a borrow, ensuring that the borrowed reference is no longer used after this point. Borrows are removed during canonicalization.
- `drop` - drops the ownership of a value. After the `drop` instruction, the variable is no longer valid. This is replaced with a call to the `drop` function for the type if it is defined, otherwise, it is removed during canonicalization for trivial types.

## Conclusion

The explicit representation of ownership semantics in OSSA GIL allows the compiler to enforce ownership rules and optimize memory management effectively. By using specific instructions for borrowing, taking, copying, and dropping values, the compiler can ensure that memory is managed safely and efficiently throughout the program's execution.
