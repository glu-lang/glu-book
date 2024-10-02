---
title: Understanding Linear Types
category: Data Types
---

Linear types are a type system that restricts the use of resources to prevent resource leaks and ensure safe memory management. In Glu, linear types are used to manage resources like file handles, network sockets, and memory.

## What are Linear Types?

Linear types are a system that enforces strict control over resource usage. In a linear type system, each resource must be used **exactly once**—no more, no less. Once a resource is consumed, it cannot be reused, copied, or discarded. This ensures proper management and automatic cleanup of resources, preventing issues like memory leaks.

##### Difference with Affine and Relevant Types

- Affine types are similar to linear types but less strict: they allow each resource to be used at most once. If a resource isn’t used, that’s acceptable, but it can never be duplicated.

- Relevant types, on the other hand, allow resources to be used any number of times, but they can’t be discarded. This ensures that a resource is always used in the program but doesn’t limit how often.

In Glu, linear types are used to manage resources that require explicit cleanup, such as file handles, network sockets, and memory. By using linear types, you can ensure that resources are properly released and prevent memory leaks and other resource-related issues.

## Using Linear Types

In Glu, linear types are declared using the `@linear` attribute. This attribute is only available for struct types and indicates that the struct is a linear type. Here is an example of a linear type declaration:

```glu
@linear struct LinearIntPtr {
    value: *Int,
}
```

In this example, the `LinearIntPtr` struct is declared as a linear type using the `@linear` attribute. The struct contains a single field `value` of type `*Int`, which is a pointer to an integer value.

When working with linear types, it is important to consume resources exactly once to prevent resource leaks. This means that you must consume a linear resource exactly **once and only once**. If you try to use a linear resource more than once, you will get a compile-time error.

```glu
func createPointer(value: Int) -> LinearIntPtr {
    let result: LinearIntPtr = { value: &value };
    return result;
}

func usePointer(ptr: LinearIntPtr) {
    ... // Do something with the struct
}

func main() {
    let ptr: LinearIntPtr = createPointer(42);
    usePointer(ptr);
    // If you try to use ptr again here, you will get a compile-time error
}
```

In this example, the `createPointer` function creates a value of type `LinearIntPtr` and returns it. The `usePointer` function takes a `LinearIntPtr` as an argument and consumes it (as it is not taking a reference to it). Once it’s consumed by the `usePointer` function, it cannot be reused or duplicated.

But, if you don’t use the value of type `LinearIntPtr`, you will get a compile-time error. This is because linear values must be consumed exactly once to prevent resource leaks and ensure proper cleanup. If you don't want to consume the value, you can use the `std::discard` function to leak it.

```glu
func main() {
    let ptr: LinearIntPtr = createPointer(42);
    // If you not use the linear element, you will get a compile-time error

    // Or, consume it like this
    std::discard(ptr);
}
```

## Conclusion

Linear types are a powerful feature of Glu that allow you to manage resources safely and prevent resource leaks. By enforcing strict control over resource usage, linear types ensure that resources are properly consumed and automatically cleaned up. This helps to prevent memory leaks and other resource-related issues, making your programs more robust and reliable. Linear types are particularly useful when working with resources that require explicit cleanup, such as file handles, network sockets, and memory. By using linear types, you can ensure that your programs are more secure and maintainable.
