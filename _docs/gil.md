---
title: The GIL Intermediate Language
category: Appendix
---

The Glu Intermediate Language (GIL) is a low-level, single-static assignment (SSA) representation of Glu code. It is used as an intermediate representation during the compilation process and is designed to be easy to generate, optimize, and translate to and from LLVM IR.

## Overview

Here is an example of a simple Glu program and its corresponding GIL representation:

```glu
func main() {
    let x: Int = 10;
    let y: Int = 20;
    let z: Int = x + y;
    std::print("The sum of x and y is " + z);
}
```

```gil
gil @main() : $() -> Void {
    %1 = integer_literal $Int, 10
    debug %1 : $Int, let "x"
    %2 = integer_literal $Int, 20
    debug %2 : $Int, let "y"
    %3 = call @+ : $(Int, Int) -> Int, %1 : $Int, %2 : $Int
    debug %3 : $Int, let "z"
    %4 = string_literal $String, "The sum of x and y is "
    %5 = call @+ : $(String, Int) -> String, %4 : $String, %3 : $Int
    call @std::print : $(String) -> Void, %5 : $String
    return
}
```

The GIL representation is a series of instructions that operate on typed values. Each instruction has a unique identifier (`%1`, `%2`, etc.) and a type annotation (`$Int`, `$String`, etc.). The `debug` instruction is used to associate a variable name with a value for the debugger.

Instruction result identifiers start with a `%` character, function identifiers start with a `@` character, and types start with a `$` character.

The top level syntax of GIL is the same as Glu: the same syntax is used for imports and type definitions, although they can have more annotations in GIL. The main difference is in the function definitions, which are written in SSA form.

```glu
// This part is the same in Glu and GIL:
import std;

typealias FD = Int;

struct File {
    fd: FD,
}

// In Glu:
func open_file(name: String) -> File {
    ...
}
// In GIL:
gil @open_file(name: String) : $(String) -> File {
    ...
}
```

## Basic Blocks

GIL functions are divided into basic blocks, which are sequences of instructions that execute in order. Each basic block ends with a terminator instruction that determines the control flow to the next block.

Here is an example of a function with multiple basic blocks:

```gil
gil @select : $(Bool, Int, Int) -> Int {
entry(%0: Bool, %1: Int, %2: Int):
    cond_br %0 : Bool, then, else
then:
    br merge(%1 : Int)
else:
    br merge(%2 : Int)
merge(%3: Int):
    return %3 : $Int
}
```

In this example, the `entry` block is the starting point of the function. It contains a conditional branch (`cond_br`) that jumps to either the `then` or `else` block based on the value of the boolean argument. Both the `then` and `else` blocks end with an unconditional branch (`br`) to the `merge` block, which contains the return instruction. Conditional branches are used to implement control flow constructs like `if` and `while` statements. Both branches cannot be the same. The unconditional branch is used to jump to a specific block, and supplies the arguments for the block.

## Instructions

GIL instructions are divided into several categories based on their purpose:
 - **Terminator Instructions**: These instructions determine the control flow of the program. Examples include `return`, `br`, and `cond_br`. They are always the last instruction in a basic block.
 - **Constant Instructions**: These instructions create constant values. Examples include `integer_literal` and `string_literal`.
 - **Debug Instruction**: This instruction has no effect on the program's execution but is used to associate a variable name with a value when using a debugger, or for decompiling the GIL back to Glu.
 - **Call Instruction**: This instruction calls a function or an operator.
 - **Conversion Instructions**: These instructions convert values between different types. Examples include `cast_int_to_ptr` and `bitcast`.
 - **Memory Instructions**: These instructions allocate, load, or store memory. Examples include `alloca`, `load`, and `store`.
 - **Aggregate Instructions**: These instructions work with aggregate types like structs. Examples include `struct_extract`, `struct_create` and `struct_destructure`.

### Instruction Syntax

The general syntax of a GIL instruction is as follows:

```gil
%result = instruction_name arg0, arg1, arg2, ..., loc "file.glu":line:column
```

Where:
 - `%result` is the SSA identifier for the result of the instruction.
 - `instruction_name` is the name of the instruction.
 - `arg0`, `arg1`, `arg2`, ... are the arguments to the instruction.
 - `loc "file.glu":42:3` is the location in the source code the instruction was generated from (file, line, and column).

Some instruction don't have a result:
```gil
instruction_name arg0, arg1, arg2, ..., loc "file.glu":42:3
```

Some can have multiple results:
```gil
%result1, %result2 = instruction_name arg0, arg1, arg2, ..., loc "file.glu":42:3
```

Arguments can be SSA values (starting with `%`), constant values (integers, floats, strings), global function identifiers (starting with `@`), types (starting with `$`), or basic block identifiers.

### Terminator Instructions

Terminator instructions are used to control the flow of the program. They are always the last instruction in a basic block. They don't produce a value, but they may take arguments.

#### `return`

The `return` instruction is used to return a value from a function. It takes a single argument, which is the value to be returned, of the return type of the function.

```gil
return %3 : $Int
```

#### `br`

The `br` instruction is an unconditional branch that jumps to a specific basic block. It takes the target block and the arguments for the block.

```gil
br merge(%1 : Int)
br loop(%0 : Int, %1 : Int)
br end
```

#### `cond_br`

The `cond_br` instruction is a conditional branch that jumps to one of two basic blocks based on a boolean condition. It takes the condition, the target blocks for the true and false cases.

```gil
cond_br %0 : Bool, then, else
```

#### `unreachable`

The `unreachable` instruction is used to indicate that a particular path of execution should never be reached. It is typically used to signal an error condition that should never occur.

```gil
unreachable
```

### Constant Instructions

Constant instructions create constant values that can be used in the program. They return a value and have no side effects.

#### `integer_literal`

The `integer_literal` instruction creates a constant integer value of a specified integer type.

```gil
%1 = integer_literal $Int, 10
%2 = integer_literal $UInt, 20
%3 = integer_literal $Int8, -7
```

#### `float_literal`

The `float_literal` instruction creates a constant floating-point value of a specified floating-point type.

```gil
%1 = float_literal $Float, 3.14
%2 = float_literal $Double, 2.71828
```

#### `string_literal`

The `string_literal` instruction creates a constant string value.

```gil
%1 = string_literal $String, "Hello, world!"
```

#### `function_ptr`

The `function_ptr` instruction creates a constant function pointer value.

```gil
%1 = function_ptr @main : $() -> Void
return %1 : $*() -> Void
```

#### `enum_variant`

The `enum_variant` instruction creates a constant enum variant value.

```gil
enum Result { SUCCESS, FAILURE }
%1 = enum_variant @Result::SUCCESS
```

The argument is the enum variant locator. The result type is the enum type.

### Debug Instruction

The `debug` instruction is used to associate a variable name with a value for debugging purposes. It has no effect on the program's execution.

The simplest form of the `debug` instruction is:
```gil
debug %1 : $Int, let "x", loc "file.glu":42:3
```

This associates the value `%1` with a constant binding (let) named "x". The `loc` part specifies the location in the source code where the variable was defined (file, line, and column). The `loc` part exists for all instructions, and is also important for stepping through the code in a debugger.

If the variable is mutable, you can use the `var` binding:
```gil
debug %1 : $Int, var "x", loc "file.glu":42:3
```

If the variable is a parameter, you can use the `arg` binding:
```gil
debug %1 : $Int, arg "x", loc "file.glu":42:3
```

If the value referenced by the debug instruction is not used by another non-debug instruction, the value and anything it references must not have side effects. Removing the debug instruction and any dead code it references should not change the behavior of the program.

Example:
```gil
// Unoptimized code:
%1 = integer_literal $Int, 10
debug %1 : $Int, let "x"
%2 = integer_literal $Int, 20
debug %2 : $Int, let "y"
%3 = call @+ : $(Int, Int) -> Int, %1 : $Int, %2 : $Int
debug %3 : $Int, let "z"
return %3 : $Int

// Valid optimized code #1:
%1 = integer_literal $Int, 10
debug %1 : $Int, let "x"
%2 = integer_literal $Int, 20
debug %2 : $Int, let "y"
%3 = integer_literal $Int, 30
debug %3 : $Int, let "z"
return %3 : $Int

// Valid optimized code #2:
%1 = integer_literal $Int, 10
debug %1 : $Int, let "x"
%2 = integer_literal $Int, 20
debug %2 : $Int, let "y"
%3 = call @+ : $(Int, Int) -> Int, %1 : $Int, %2 : $Int
debug %3 : $Int, let "z"
%4 = integer_literal $Int, 30
return %4 : $Int
```

The call to the `+` function is not optimized out in the second example because the value is used in the debug instruction. The `+` function is known to have no side effects, so it can either be folded into a constant 30, or the call can be kept for debugging only. LLVM will fold the call to the `+` function into a constant anyway at a later stage.

### Call Instruction

The `call` instruction is used to call a function or an operator. It takes the function or operator name, and the arguments to the function or operator.

```gil
%3 = call @+ : $(Int, Int) -> Int, %1 : $Int, %2 : $Int
%4 = call @std::print : $(String) -> Void, %0 : $String
```

The first argument can be:
 - A global function name, starting with `@`, and having a function type.
 - An operator name, starting with `@`, and having a function type.
 - A local function pointer, starting with `%`, and having a function pointer type.

The following arguments must match the function type's argument types. The result type of the instruction is the return type of the function.

### Conversion Instructions

Conversion instructions are used to convert values between different types. They have no side effects and return a value.

#### `cast_int_to_ptr`

The `cast_int_to_ptr` instruction converts an integer value to a pointer value. This may break pointer aliasing rules, so it should be used with caution.

```gil
%0 = integer_literal $Int, 0xff73000
%1 = cast_int_to_ptr $*Char, %0 : $Int
```

#### `cast_ptr_to_int`

The `cast_ptr_to_int` instruction converts a pointer value to an integer value of the same bit width.

```gil
%1 = cast_ptr_to_int $Int64, %0 : $*Char
```

#### `bitcast`

The `bitcast` instruction converts a value to a different type without changing the underlying bit pattern. This is useful for converting between different pointer types, between integers and floating-point values, between integers of different signedness, or between incompatible struct types.

```gil
%1 = bitcast $Int32, %0 : $Float
%1 = bitcast $*Int32, %0 : $*Float
%1 = bitcast $UInt, %0 : $Int
```

#### `int_trunc`

The `int_trunc` instruction truncates an integer value to a smaller integer type, discarding the high-order bits.

```gil
%1 = int_trunc $Int8, %0 : $Int
```

#### `int_zext`

The `int_zext` instruction zero-extends an integer value to a larger integer type, filling the high-order bits with zeros. Should only be used for unsigned integers.

```gil
%1 = int_zext $UInt32, %0 : $UInt8
```

#### `int_sext`

The `int_sext` instruction sign-extends an integer value to a larger integer type, filling the high-order bits with the sign bit. Should only be used for signed integers.

```gil
%1 = int_sext $Int32, %0 : $Int8
```

#### `float_trunc`

The `float_trunc` instruction truncates a floating-point value to a smaller floating-point type.

```gil
%1 = float_trunc $Float, %0 : $Double
```

#### `float_ext`

The `float_ext` instruction extends a floating-point value to a larger floating-point type.

```gil
%1 = float_ext $Double, %0 : $Float
```

### Memory Instructions

Memory instructions are used to allocate, load, and store memory. They have side effects.

#### `alloca`

The `alloca` instruction allocates memory on the stack for a value of a specified type. It returns a pointer to the allocated memory.

```gil
%1 = alloca $Int
```

The result type is a pointer to the specified type.

#### `load`

The `load` instruction dereferences a pointer and loads the value from memory. It takes a pointer to the memory location to load from.

```gil
%1 = load %0 : $*Int
```

The argument must be a pointer type, and the result type is the deferenced type.

#### `store`

The `store` instruction stores a value to a memory location. It takes the value to store and a pointer to the memory location.

```gil
store %1 : $Int, %0 : $*Int
```

The first argument is the value to store, and the second argument is a pointer type to the memory location.

### Aggregate Instructions

Aggregate instructions work with aggregate types like structs. They have no side effects and return a value.

#### `struct_extract`

The `struct_extract` instruction extracts a field from a struct value. It takes the struct value and the field.

```gil
struct Point { x: Int, y: Int }
%1 = struct_extract %0 : $Point, @Point::x
```

The first argument is the struct value, and the second argument is the field locator. The result type is the type of the field.

#### `struct_create`

The `struct_create` instruction creates a struct value from its fields. It takes the field values.

```gil
struct Point { x: Int, y: Int }
%1 = struct_create $Point, %0 : $Int, %1 : $Int
```

The first argument is the struct type, and the following arguments are the field values. The following arguments must match the field types in order. The result type is the first argument.

#### `struct_destructure`

The `struct_destructure` instruction destructures a struct value into its fields. It takes the struct value and the field types.

```gil
%1, %2 = struct_destructure %0 : $Point
```

The argument is the struct value. The result types are the field types.

### Pointer Instructions

Pointer instructions are used to manipulate pointers. They have no side effects and return a value.

#### `struct_field_ptr`

The `struct_field_ptr` instruction computes the address of a field within a struct. It takes a pointer to the struct and the field value.

```gil
%1 = struct_field_ptr %0 : $*Point, @Point::x
```

The first argument is a pointer to the struct, and the second argument is the field locator. The result type is a pointer to the field type.

#### `ptr_offset`

The `ptr_offset` instruction computes the address of an element at a specified offset from a pointer. It takes a pointer and an unsigned integer offset or constant.

```gil
%1 = ptr_offset %0 : $*Int, %2 : $UInt
%1 = ptr_offset %0 : $*Int, 4
```

The first argument is a pointer, and the second argument is an integer offset. The result type is the same as the first argument.

## Conclusion

The Glu Intermediate Language (GIL) is a low-level, single-static assignment (SSA) representation of Glu code. It is designed to be easy to generate, optimize, and translate to and from LLVM IR. GIL functions are divided into basic blocks, which are sequences of instructions that execute in order. GIL is in the middle of the compilation process, between the high-level Glu code and the low-level LLVM IR. It is an essential part of the Glu compiler infrastructure and is used to perform optimizations and generate efficient machine code.
