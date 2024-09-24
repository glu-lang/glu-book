---
title: Understanding Linear Types
category: Common Concepts
---

Linear types are a type system that restricts the use of resources to prevent resource leaks and ensure safe memory management. In Glu, linear types are used to manage resources like file handles, network sockets, and memory.

## What are Linear Types?

Linear types are a type system that enforces the "linear use" of resources. In a linear type system, each resource can only be used once. This means that once a resource is consumed, it cannot be reused or duplicated. Linear types prevent resource leaks by ensuring that resources are properly managed and released.

In Glu, linear types are used to manage resources that require explicit cleanup, such as file handles, network sockets, and memory. By using linear types, you can ensure that resources are properly released and prevent memory leaks and other resource-related issues.

## Using Linear Types

In Glu, linear types are declared using the `@linear` attribute. Here is an example of a linear type declaration:

```glu
@linear LinearIntPtr = *Int;
```

In this example, the linear type `LinearIntPtr` is created for the pointer type `*Int`. The `@linear` attribute indicates that `LinearIntPtr` is a linear type, which means that it can only be used once.

You can create linear types for any type in Glu, including primitive types, custom types, and generic types. Linear types are particularly useful for managing resources that require explicit cleanup, such as memory allocations and file handles.

```glu
func createPointer(value: Int) -> LinearIntPtr {
    return &value;
}

func usePointer(ptr: LinearIntPtr) {
    let value = *ptr;
    std::print("Value: \(value)");
}

func main() {
    let ptr = createPointer(42);
    usePointer(ptr);
    // If you try to use ptr again here, you will get a compile-time error
}
```

In this example, the `createPointer` function creates a linear pointer to an integer value, and the `usePointer` function consumes the pointer exactly once. If you try to use the pointer again after calling `usePointer`, you will get a compile-time error because linear types enforce the "linear use" of resources.

## Conclusion

Linear types are a powerful feature of Glu that help prevent resource leaks and ensure safe memory management. By enforcing the "linear use" of resources, linear types help you manage resources like file handles, network sockets, and memory more effectively. When working with linear types, it is important to consume resources exactly once to prevent resource leaks and ensure proper cleanup. Linear types are a valuable tool for managing resources in Glu and improving the reliability and safety of your code.
