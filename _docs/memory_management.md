---
title: Managing Memory
category: Common Concepts
---

Memory management is an essential part of programming. It involves allocating and deallocating memory to store data during the execution of a program. In Glu, memory management must be handled explicitly by the programmer, as Glu does not have automatic memory management like garbage collection.

In this chapter, we will explore the basics of memory management in Glu, including how to allocate and deallocate memory, manage memory leaks, and use references and pointers effectively.

## Stack and Heap

In Glu, memory is divided into two main regions: the stack and the heap, just like in many other programming languages.

Stack memory is used for storing local variables and function call information. It is managed automatically by the compiler and is fast but limited in size. Stack memory is allocated when a function is called and deallocated when the function returns.

Heap memory is used for storing dynamically allocated memory, such as objects and arrays. It is managed explicitly by the programmer and is slower but can grow as needed.

## Stack Allocation

Allocating memory on the stack can be done by declaring variables inside a function. For example:

```glu
func allocateInteger() {
    var x: Int;
}
```

In this example, the variable `x` is allocated on the stack when the function `allocateInteger` is called. The memory for `x` is automatically deallocated when the function returns.

## Variable Addresses

In Glu, you can get the memory address of a variable using the `&` operator. Pointers are used to store memory addresses. Pointers can then be used to access or modify the value stored at that memory address, using the pointer dereference operator `.*`.

```glu
func main() {
    var x: Int = 42;
    var address: *Int = &x;
    std::print(address.*); // Output: 42
    address.* = 21;
    std::print(x); // Output: 21
}
```

Note that you can only get the address of a variable allocated on the stack, which is not the case for `let` constants.

## Heap Allocation

Allocating memory on the heap can be done using the `alloc` function. The `alloc` templated function allocates memory for a single value of the specified type on the heap and returns a pointer to that memory.

```glu
func main() {
    var x: *unique Int = alloc<Int>();
    x.* = 42;
    std::print(x.*); // Output: 42
    free(x);
}
```

The `alloc` function returns a unique pointer, which is a linear type: it must be transferred exactly once. This way, the compiler ensures that the memory is only freed once and that there are no memory leaks.

If you need to leak the memory, you can use the `release` function to release the memory from the unique pointer. This can be necessary when you want to transfer ownership of the memory to another part of the program, which uses a different memory management strategy. The `release` function consumes the unique pointer and returns the raw pointer, which can then be used to free the memory manually.

```glu
func main() {
    let x: *unique Int = alloc<Int>();
    x.* = 42;
    std::print(x.*); // Output: 42
    let raw: *Int = release(x);
    // Do something with raw
    // Note: The compiler will not check if raw is freed correctly.
    // Only use this if you know what you are doing.
}
```

Unique pointers are a powerful feature of Glu that allows you to manage memory safely and efficiently. By using unique pointers, you can ensure that memory is deallocated correctly and avoid common memory management issues like double-free errors and memory leaks.

Unique pointers can be transferred between functions using the move semantics, which allows you to pass ownership of the memory from one part of the program to another without copying the memory itself. Unique pointers can also be converted implicitly to raw pointers, which allows you to interact with C libraries or other parts of the program that expect raw pointers. This is necessary when the pointer needs to be borrowed, instead of transferred.

Here is an example of transferring a unique pointer between functions:

```glu
// This returns a unique pointer to an integer, which means that the ownership of the memory is transferred to the caller.
func createCounter() -> *unique Int {
    let counter: *unique Int = alloc<Int>();
    counter.* = 0;
    return counter;
}

// This takes any pointer to an integer and increments the value stored at that memory address.
func incrementCounter(counter: *Int) {
    counter.* += 1;
}

func main() {
    let counter: *unique Int = createCounter();
    incrementCounter(counter);
    std::print(counter.*); // Output: 1
    free(counter);
}
```

In this example, the `createCounter` function allocates memory for an integer on the heap and returns a unique pointer to that memory, which means the caller is responsible for it. The `incrementCounter` function borrows a raw pointer to an integer and increments the value stored at that memory address. The `main` function creates a counter, increments it, and prints the result. As the memory is transferred to the `counter` binding within the `main` function, it is the responsibility of the `main` function to free the memory at the end.

Move semantics means that the binding `counter` in the `main` function takes ownership of the memory allocated in the `createCounter` function. When `counter` is passed to the `incrementCounter` function, it is borrowed as a raw pointer, and the ownership is not transferred. This allows the `incrementCounter` function to modify the value stored at that memory address without taking ownership of the memory. The `free` function finally takes ownership of the memory to deallocate it, ensuring that no pointer to the memory is left after the memory is freed.

Note that move semantics also exist within the same function. For example, you can move a unique pointer to another binding within the same function, which means the first binding is no longer valid:

```glu
func main() {
    let counter: *unique Int = createCounter();
    let moved: *unique Int = counter;
    free(moved);
}
```

In this example, the `counter` binding is moved to the `moved` binding, which means that the `counter` binding is no longer valid. The ownership of the memory is finally transferred to the `free` function. Note that `free(counter);` would not compile in this case, as the ownership of the memory has already been transferred to the `moved` binding.

## Allocating Arrays

You can also allocate arrays on the heap using the `calloc` function. The `calloc` function allocates a contiguous array of memory for the specified number of elements of the specified type, and default-initializes each element. A unique pointer to the array is returned.

```glu
let array: *unique Int = calloc<Int>(10);
std::assert(array[0] == 0);
std::assert(array[9] == 0);
array[0] = 42;
std::print(array[0]); // Output: 42
free(array);
```

In this example, the `calloc` function allocates an array of 10 integers on the heap and returns a unique pointer to that memory. The elements of the array are default-initialized, the default value for integers being 0. The array can be accessed and modified using the subscript operator `[]`. Just like with single values, the memory for the array must be freed using the `free` function for the code to compile.

Arrays can also be resized using the `realloc` function. The `realloc` function takes a pointer to an existing array, the new size of the array, and returns a new pointer to the resized array. Because they all are unique pointers, the compiler can check that the old pointer is not used after the reallocation.

```glu
let array: *unique Int = calloc<Int>(10);
array[8] = 42;
let second: *unique Int = realloc(array, 20);
// array cannot be referenced anymore, as it was moved to second
std::assert(second[8] == 42);
```

## Shared Pointers

In addition to unique pointers, Glu also supports shared pointers, which allow multiple parts of the program to share ownership of the memory. Shared pointers are reference-counted, which means that the memory is deallocated when the last reference to it is dropped.

Shared pointers can be created using the `alloc_shared` function, which allocates memory for a single value of the specified type on the heap and returns a shared pointer to that memory.

```glu
let x: *shared Int = alloc_shared<Int>();
x.* = 42;
std::print(x.*); // Output: 42
```

Unlike unique pointers, shared pointers can be copied and cloned, which allows multiple parts of the program to share ownership of the memory. When a shared pointer is copied, the reference count is incremented, and when a shared pointer is dropped, the reference count is decremented. When the reference count reaches zero, the memory is deallocated.

The reference counting is done automatically when copying and dropping shared pointers, so you don't need to worry about managing the memory manually. This makes shared pointers a convenient and safe way to manage memory in Glu.

## Conclusion

Memory management is a critical aspect of programming, and Glu provides powerful tools to help you manage memory efficiently and safely. By using unique pointers and shared pointers, you can allocate and deallocate memory on the heap, transfer ownership of memory between parts of the program, and avoid common memory management issues like double-free errors and memory leaks. By understanding the basics of memory management in Glu, you can write more robust and reliable programs that make efficient use of memory resources.