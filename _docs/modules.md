---
title: Separating your code into Modules
category: Imports and Modules
---

Modules in Glu allow you to organize and reuse code across different files and projects. Glu provides flexible import mechanisms that allow you to control how external files and their content are imported. This flexibility is essential for managing dependencies, keeping your code clean, and avoiding name conflicts.

In this article, we'll explore different ways to import modules in Glu and discuss why it's important to use them effectively.

## Basic Module Import

In Glu a module is implicitly created for each file. It will include everything contained within the file, and will be named after it.

The simplest way to import a module is using the `import` statement followed by the module's filename.

For example, let's say you have a file called `file.glu`:

```glu
func helloFromFile() {
    std::print("Hello from file!");
}

func byeFromFile() {
    std::print("Bye from file!");
}
```

To use this function in another file, simply import the file and call the function with the module's name as a prefix:

```glu
import file;

func main() {
    file::helloFromFile(); // This will print "Hello from file!"
}
```
Here, `file::helloFromFile()` is used to call `helloFromFile` from the `file` module. The `::` operator is used to access functions, constants, and other definitions from the imported module.

## Renaming Imports

Sometimes, the module name can be long or ambiguous. To make the code more readable, you can rename the imported module using the `as` keyword:

```glu
import file as toto;

func main() {
    toto::helloFromFile();
}
```
In this example, `file` is renamed to `toto`, so you can access the `helloFromFile()` using `toto::helloFromFile()`. This is particularly useful when you have multiple modules with similar names or when you want to shorten the module name for convenience.

## Importing Specific Functions

If you only need a specific function from a module, you can import it directly:
```glu
import file::helloFromFile;

func main() {
    helloFromFile();
}
```
By importing only `helloFromFile()`, you don't need to reference the module name when calling the function. This can make the code cleaner, especially when working with frequently used functions.

## Importing Multiple Items with Renaming

You can also import multiple items from a module and rename them individually. This can be useful when you need to resolve naming conflicts or want to clarify the usage of certain functions:

```glu
import file::{helloFromFile, byeFromFile as bye};

func main() {
    helloFromFile();
    bye(); // This calls byeFromFile, but it's renamed to bye.
}
```
In this case, `helloFromFile()` is imported as is, but `byeFromFile()` is renamed to `bye()`. This allows you to use more descriptive or context-specific names in your code.

## Importing Structs and Enums

In addition to functions, Glu allows you to import structures (struct) and enumerations (enum) from other modules. This is particularly useful when you want to share data types between different parts of your project.

### Importing a Struct
Let's say you have a `Person` struct defined in a file named `types.glu`:

```glu
struct Person {
    name: String,
    age: Int,
}
```
You can import this `Person` struct and use it in another file:

```glu
import types::Person;

func main() {
    let john: john = { "John", 25 }
    std::print(john.name);
}
```

Here, `Person` is imported from the `types` file, and the struct is instantiated and used in the `main` function.

### Importing an Enum

Enums are another important feature that can be imported using the `import` statement. Suppose you have the following enum defined in `status.glu`:

```glu
enum Status {
    SUCCESS,
    FAILURE,
}
```

To use this enum in another file, you can import it like so:

```glu
import status::Status;

func printStatus(value: Status) {
    if value == Status::SUCCESS {
        std::print("Success!");
    } else {
        std::print("Failure!");
    }
}
```

If you only need one element of the Enum you can also import it like so:

```glu
import {status::Status, status::Status::SUCCESS};

func printStatus(value: Status) {
    if value == SUCCESS {
        std::print("Success!");
    } else {
        std::print("Failure!");
    }
}

These functions use the imported `Status` enum and checks whether it holds a SUCCESS or a FAILURE value to print a status message.

## Importing Everything

If you need access to all functions and definitions in a module, you can use the `*` syntax:

```glu
import file::*;

func main() {
    function();
}
```
This imports all the items from the module `file` and allows you to use them without the module prefix. However, be cautious when using this approach, as it can lead to name conflicts if multiple modules define the same function or variable names.

## Why Use Modules?

Modules are a powerful tool that help you:

* Organize Your Code: As your project grows, keeping everything in one file can become unmanageable. By splitting your code into multiple modules, you can separate functionality into logical units. This makes it easier to maintain and extend your codebase.

* Avoid Name Conflicts: When working with large projects or when integrating third-party libraries, name conflicts can occur if two files define functions or variables with the same name. Modules allow you to prevent these conflicts by scoping definitions within the module.

* Reuse Code: By placing common functionality into separate modules, you can reuse code across multiple projects. This reduces duplication and simplifies maintenance. If a bug is found in a module, fixing it in one place will propagate the fix to all places where the module is used.

* Encapsulate Logic: Modules provide a way to encapsulate logic, making it easier to reason about the program. Each module can represent a distinct functionality, which can be tested and understood independently from the rest of the code.

## Best Practices

* Use Specific Imports: Whenever possible, import only the functions or definitions you need. This reduces the likelihood of name conflicts and makes it clear which parts of the module are used.

* Rename Imports: If you're working with multiple modules that contain functions with the same name, use the as keyword to rename the imports and avoid confusion.

* Avoid Wildcard Imports (*): While convenient, wildcard imports can lead to name collisions and make it difficult to track where specific functions or variables are coming from. Use them sparingly, especially in larger projects.

* Modularize Your Code: As your project grows, break it down into smaller, reusable modules. This makes your code easier to navigate and maintain over time.

## Conclusion

Modules in Glu are a versatile tool that help you manage the complexity of larger projects. By using the various import methods provided by Glu, you can organize your code, avoid conflicts, and make your programs more maintainable. Whether you are building small scripts or large applications, making effective use of modules is crucial to writing clean, scalable code.
