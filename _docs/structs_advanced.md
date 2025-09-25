---
title: Overloading Structure Ownership Behaviors
category: Polymorphism
---

Structures in Glu allow you to group related data together, and you can enhance their functionality by overloading some behaviors. This includes overloading destructors, copy operations, etc.

## Structure Ownership Types

In Glu, structures can have different ownership semantics, which dictate how instances of the structure can be used. The three main ownership types are:

 - Linear: Must be used exactly once.
 - Affine: Must be used at most once.
 - Normal: Can be used freely.

Linear and affine structures are move-only types, meaning they cannot be copied, only moved. Normal structures can be copied freely. By default, structures are normal, but you can specify the ownership type using attributes.

## Declaring an Affine Structure

An affine structure is a structure that must be used at most once. This is useful for managing resources that should not be duplicated, such as file handles or network connections, but can be discarded when no longer needed.

```glu
@affine struct FileHandle {
    id: Int32
}
```

In this example, the structure `FileHandle` has a single field: `id` of type `Int32`. The `@affine` attribute indicates that instances of this structure cannot be copied, only moved.

```glu
let handle1: FileHandle = openFile("example.txt");
let handle2: FileHandle = handle1; // Moves handle1 to handle2, handle1 is now invalid
let handle3: FileHandle = handle1; // Error: handle1 has been moved
```

## Declaring a Linear Structure

A linear structure is a structure that must be used exactly once. This is useful for managing resources that require strict ownership semantics. Just like affine structures, linear structures cannot be copied, only moved, but additionally, they must be explicitly consumed or dropped.

```glu
@linear struct UniqueResource {
    ...
}

func doSomething() {
    let resource1: UniqueResource = acquireResource();
    let resource2: UniqueResource = resource1; // Moves resource1 to resource2, resource1 is now invalid
    drop(resource2); // Explicitly drop resource2
}
```

Unlike affine structures, if a linear structure is not moved elsewhere or dropped before it goes out of scope, the compiler will raise an error.

## Overloading the Copy Function

You can overload the copy function for a structure to define custom behavior when an instance of the structure is copied. This is useful for managing resources that require special handling during copying, for implementing copy-on-write semantics, or for implementing custom deep copies.

You cannot overload the copy function for linear or affine structures, as they cannot be copied.

```glu
struct InnerCowString {
    data: *Char,
    refCount: UInt32
}

struct CowString {
    inner: *InnerCowString
}

func copy(original: CowString) -> CowString {
    original.inner.refCount += 1;
    return original;
}
```

In this example, we have two structures: `InnerCowString` which would be stored on the heap, and `CowString` which contains a pointer to `InnerCowString`. `CowString` implements copy-on-write semantics by overloading the `copy` function. When a `CowString` instance is copied, the reference count of the underlying `InnerCowString` is incremented, and the original instance is returned.

## Overloading the Drop Function

You can overload the drop function for a structure to define custom behavior when an instance of the structure is destroyed. This is useful for managing resources that require special handling during destruction, such as closing file handles or freeing memory.

For example, for the `FileHandle` structure defined earlier, you can overload the drop function to close the file when the `FileHandle` instance is destroyed:

```glu
struct FileHandle {
    id: Int32
}

func drop(handle: FileHandle) {
    closeFile(handle.id);
}
```

Or for the `CowString` structure, you can overload the destructor to decrement the reference count and free the memory when the reference count reaches zero:

```glu
func drop(str: CowString) {
    str.inner.refCount -= 1;
    if str.inner.refCount == 0 {
        free(str.inner.data);
        free(str.inner);
    }
}
```

## Overloading the Move Function

You can overload the move function for a structure to define custom behavior when an instance of the structure is moved. This is useful for managing resources that require special handling during moving, such as updating pointers or transferring ownership.

```glu
struct LinkedListNode {
    value: Int,
    prev: *LinkedListNode,
    next: *LinkedListNode
}

func move(original: *LinkedListNode, target: *LinkedListNode) {
    if original.prev != null {
        original.prev.next = target;
    }
    if original.next != null {
        original.next.prev = target;
    }
    target.value = original.value;
    target.prev = original.prev;
    target.next = original.next;
}
```

The `move` function takes two pointers to `LinkedListNode` instances: `original` and `target`. The original is a pointer to the value being moved, and the target is an uninitialized pointer where the value should be moved to. The original value is left in an unspecified state after the move, the `drop` function will not be called on it.

## Calling Ownership Functions Manually

You can call the ownership functions manually, as well as let the compiler do it for you. The compiler automatically calls these functions when appropriate, such as when a structure is copied, moved, or destroyed.

```glu
func useFileHandles() {
    let handle1: FileHandle = openFile("example.txt");
    let handle2: FileHandle = move(handle1); // Explicit move
    let handle3: FileHandle = handle2; // Implicit move
    drop(handle3); // Explicit drop (this line can be omitted, as drop will be called automatically when handle3 goes out of scope)
}

func useCowStrings() {
    let str1: CowString = createCowString("Hello, world!");
    let str2: CowString = copy(str1); // Explicit copy
    let str3: CowString = str2; // Implicit copy
    drop(str2); // Explicit drop
    // drop will be called automatically for str3 and str1 here (in reverse order of creation)
}
```

Note that for move-only structures, the `copy` function cannot be called. When a value is used, the value will be copied if it is copyable, or moved if it is move-only.

## Conclusion

Structures in Glu provide powerful capabilities for managing data and resources. By overloading ownership functions, you can customize how structures behave during copying, moving, and destruction, allowing for efficient and safe resource management in your programs.
