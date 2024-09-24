---
title: Understanding Linear Types
category: Common Concepts
---

Linear types are a type system that restricts the use of resources to prevent resource leaks and ensure safe memory management. In Glu, linear types are used to manage resources like file handles, network sockets, and memory.

## What are Linear Types?

Linear types are a type system that enforces the "linear use" of resources. In a linear type system, each resource can only be used once and only once. This means that once a resource is consumed, it cannot be reused or duplicated. Linear types prevent resource leaks by ensuring that resources are properly managed and released.

In Glu, linear types are used to manage resources that require explicit cleanup, such as file handles, network sockets, and memory. By using linear types, you can ensure that resources are properly released and prevent memory leaks and other resource-related issues.

## Using Linear Types

In Glu, linear types are declared using the `@linear` attribute. This attribute is only available for struct types and indicates that the struct is a linear type. Here is an example of a linear type declaration:

```glu
@linear struct LinearIntPtr {
    value: *Int;
}
```

In this example, the `LinearIntPtr` struct is declared as a linear type using the `@linear` attribute. The struct contains a single field `value` of type `*Int`, which is a pointer to an integer value.

When working with linear types, it is important to consume resources exactly once to prevent resource leaks. This means that you must consume a linear resource exactly once and only once. If you try to use a linear resource more than once, you will get a compile-time error.

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

In this example, the `createPointer` function creates a `LinearIntPtr` struct and returns it. The `usePointer` function takes a `LinearIntPtr` struct as an argument and consumes it. Once the `LinearIntPtr` struct is consumed by the `usePointer` function, it cannot be reused or duplicated.

## Conclusion

Linear types are a powerful feature of Glu that help prevent resource leaks and ensure safe memory management. By enforcing the "linear use" of resources, linear types help you manage resources like file handles, network sockets, and memory more effectively. When working with linear types, it is important to consume resources exactly once to prevent resource leaks and ensure proper cleanup. Linear types are a valuable tool for managing resources in Glu and improving the reliability and safety of your code.
