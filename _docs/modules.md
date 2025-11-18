---
title: Separating your code with Modules and Namespaces
category: Imports and Modules
---

Code in Glu can be separated into different files called modules. Modules in Glu allow you to organize and reuse code across different files and projects. Glu provides flexible import mechanisms that allow you to control how external files and their content are imported. This flexibility is essential for managing dependencies, keeping your code clean, and avoiding name conflicts.

Namespaces are another way to organize code, allowing you to group related functions, types, and constants together. This helps prevent naming collisions and makes it easier to understand the structure of your code.

Modules are a kind of namespace, but local namespaces can also be created within a single file using the `namespace` keyword. This allows you to further organize your code without needing to create separate files.

In this article, we'll explore different ways to import modules in Glu.

## Module Definitions

In Glu, each file represents a module. The module will include everything contained within the file, and will be named after it.

Modules can be imported into other files to access their public functions, types, and constants.

### Importing a Module

The simplest way to import a module is using the `import` statement followed by the module's filename, without the `.glu` extension.

For example, let's say you have a file called `file.glu`:

```glu
public func helloFromFile() {
    std::print("Hello from file!");
}

public func byeFromFile() {
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
Here, `file::helloFromFile()` is used to call `helloFromFile` from the `file` module. The `::` operator is used to access functions, constants, and other definitions from a namespace, which in this case is the module `file`.

Note that only the functions marked as `public` in the imported module can be accessed from outside the module. Functions without the `public` keyword, or that have an explicit `private` keyword, are private to the module and cannot be accessed from other modules.

### Renaming Imports

Sometimes, the module name can be long or ambiguous. To make the code more readable, you can rename the import using the `as` keyword:

```glu
import file as foo;

func main() {
    foo::helloFromFile();
}
```
In this example, `file` is renamed to `foo`, so you can access the `helloFromFile()` using `foo::helloFromFile()`. This is particularly useful when you have multiple modules with similar names or when you want to shorten the module name for convenience.

In this case, note that the module is still called `file`, but the namespace that is defined within the importing file and that allows access to the module's contents, is named `foo`.

### Importing Specific Functions

If you only need a specific function from a module, you can import it directly:
```glu
import file::helloFromFile;

func main() {
    helloFromFile();
}
```
By importing only `helloFromFile()`, no namespace is created, so you can call the function directly without the namespace operator `::`. This can be useful if you only need a few functions from a module and want to avoid cluttering your code with unnecessary prefixes.

### Importing Multiple Items with Renaming

You can also import multiple items from a module and rename them individually. This can be useful when you need to resolve naming conflicts or want to clarify the usage of certain functions:

```glu
import file::{helloFromFile, byeFromFile as bye};

func main() {
    helloFromFile();
    bye(); // This calls byeFromFile, but it's renamed to bye.
}
```
In this case, `helloFromFile()` is imported as is, but `byeFromFile()` is renamed to `bye()`. This allows you to use more descriptive or context-specific names in your code.

### Importing Structs and Enums

In addition to functions, Glu allows you to import structures (struct) and enumerations (enum) from other modules. This is particularly useful when you want to share data types between different parts of your project.

Let's say you have a `Person` struct and a `Status` enum defined in a file named `types.glu`:

```glu
public struct Person {
    name: String,
    age: Int,
}

public enum Status {
    SUCCESS,
    FAILURE,
}
```
You can import this `Person` struct and use it in another file:

```glu
import types::Person;

func main() {
    let john: Person = { "John", 25 }
    std::print(john.name);
}
```

Or you can import the `Status` enum:

```glu
import types::Status;

func printStatus(value: Status) {
    if value == Status::SUCCESS {
        std::print("Success!");
    } else {
        std::print("Failure!");
    }
}
```

You may also import the module as a namespace, in which case you would access the struct and enum using the namespace operator:

```glu
import types;

func main() {
    let jane: types::Person = { "Jane", 30 }
    std::print(jane.name);

    printStatus(types::Status::SUCCESS);
}
```

### Importing Everything

You can also import all public items from a module using the wildcard `*`:

```glu
import file::*;

func main() {
    function();
}
```

This imports all the items from the module `file` and allows you to use them without the namespace operator. However, be cautious when using this approach, as it can lead to name conflicts if multiple modules define the same function or variable names, of can cause issues if the module is updated with new public items.

## Local Namespaces

In addition to modules, Glu allows you to create local namespaces within a single file using the `namespace` keyword. This is useful for organizing related functions, types, and constants without needing to create separate files.

Here's an example of how to create and use a local namespace:

```glu
public namespace math {
    public func add(a: Int, b: Int) -> Int {
        return a + b;
    }
}
```

To use the `add` function from the `math` namespace within the same file, you would do the following:

```glu
func main() {
    let result = math::add(5, 3);
    std::print(result); // Output: 8
}
```

In this example, the `math` namespace contains a public function `add`. You can access this function using the namespace operator `::`, just like with imported modules.

Note that, again, only the items marked as `public` within a namespace can be accessed from outside the module. Private items can be accessed within the same file, but not when imported into other files.

If the above `math` namespace were defined in a separate file, you would need to import that file:

```glu
import math_module; // Assuming the namespace is defined in math_module.glu

func main() {
    let result = math_module::math::add(5, 3);
    std::print(result); // Output: 8
}
```

Here, `math_module` is the module namespace, and `math` is the local namespace defined within that module.

### Nested Namespaces

You can also nest namespaces within other namespaces to create a hierarchical structure:

```glu
namespace geometry {
    namespace circle {
        func area(radius: Float) -> Float {
            return 3.14 * radius * radius;
        }
    }
}
// or
namespace geometry::circle {
    func area(radius: Float) -> Float {
        return 3.14 * radius * radius;
    }
}
``` 

To use the `area` function from the nested `geometry::circle` namespace, you would do the following:

```glu
func main() {
    let area = geometry::circle::area(5.0);
    std::print(area); // Output: 78.5
}
```

### Extending Namespaces

You can always add more functions or types to an existing namespace by declaring the namespace again within the same file:

```glu
namespace math { /* ... previous definitions ... */ }
namespace math {
    public func multiply(a: Int, b: Int) -> Int {
        return a * b;
    }
}
```
Now, both `add` and `subtract` functions are part of the `math` namespace and can be accessed using the namespace operator. Note that within the namespace, you can access other functions and types defined in the same namespace without needing to prefix them with the namespace name:

```glu
namespace math {
    public func square(a: Int) -> Int {
        return multiply(a, a); // Accessing multiply without prefix
    }
}
```

In this example, the `square` function calls the `multiply` function directly, as they are both part of the same `math` namespace.

### Enum Namespaces

Enums in Glu automatically create a namespace for their variants. This means that each variant of the enum can be accessed using the enum's name as a prefix. More details about enums can be found in the [Enums](enums) section.

Note that as enums create their own namespace, you can extend an enum namespace, just like with regular namespaces:

```glu
enum Color {
    red,
    green,
    blue,
}

namespace Color {
    let defaultColor = Color::green;
}
```

Now, `defaultColor` is part of the `Color` enum's namespace and can be accessed using `Color::defaultColor`, similarly to how you would access the enum variants with `Color::red`, `Color::green`, and `Color::blue`.

## Summary

Modules and namespaces in Glu provide powerful ways to organize and manage your code. By using modules, you can separate your code into different files, making it easier to maintain and reuse. Namespaces allow you to group related functions, types, and constants together, preventing naming conflicts and improving code readability.

Modules don't have to be written in Glu, as you will discover in the next chapter.
