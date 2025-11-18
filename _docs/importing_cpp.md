---
title: Import a C++ Library in Glu
category: Imports and Modules
---

The Glu programming language is designed to be used in conjunction with other languages and tools. In this tutorial, we will see how to use Glu in a project that involves multiple languages and tools.

## Creating a Project Directory

First, create a directory for the project.

```bash
mkdir glue-project
cd glue-project
```

## Creating a C++ File

Create a file named `liba.cpp` in the project directory.

```bash
touch liba.cpp
```

Open the `liba.cpp` file in a text editor and add the following code.

```cpp
#include <iostream>

void funcA() {
    std::cout << "Hello from C++!" << std::endl;
}
```

Save the file. This file contains a function `funcA` that we will call from Glu. Note that this file has a `.cpp` extension, which is used for C++ source files.

## Creating a Glu File

Create a file named `main.glu` in the project directory.

```bash
touch main.glu
```

Open the `main.glu` file in a text editor and add the following code.

```glu
import liba;

func main() {
    liba::funcA();
}
```

This file imports the C++ library `liba` from the file `liba.cpp` and calls the function `funcA` from the library in the `main` function.

The `import` statement is used to import external modules or libraries into a Glu program. In this case, we import the `liba` module, which corresponds to the C++ library defined in the `liba.cpp` file. The namespace operator `::` is then used to access functions and variables in the imported module.

To compile the program, run the following command in the terminal.

```bash
clang++ -g -c -emit-llvm liba.cpp -o liba.bc
GLU_LINKER='clang++' gluc main.glu -o main
```

This will create an LLVM bitcode file named `liba.bc` from the C++ file `liba.cpp`, using the Clang compiler. The `-g` flag is used to include debugging information in the generated bitcode file, which the Glu compiler will use to import the C++ code correctly.

Then, we compile the Glu program `main.glu` using the `gluc` compiler. You could add an `-I .` flag to specify the include path, but imports are always looked up in the current directory first. Imports can be used for including bitcode files, LLVM IR files, and glu files.

The `GLU_LINKER` environment variable is set to `clang++`, which tells the Glu compiler to use the Clang C++ compiler as the linker for linking the Glu program with the C++ standard library, which is necessary when C++ libraries such as iostream are used.

The `gluc` compiler will generate an executable file named `main` in the project directory, which contains the Glu program linked with the C++ library.

You can run the executable with `./main`; the output should be:
```
Hello from C++!
```

This demonstrates how Glu can be used in conjunction with other languages and tools to create a project that involves multiple components written in different languages. Glu can be used to glue together components written in different languages, providing a seamless integration between them.

## Linking separately

If you want to compile the Glu program and the C++ library separately, you can do so by following these steps.

First, compile the C++ library to LLVM bitcode.

```bash
clang++ -g -c -emit-llvm liba.cpp -o liba.bc
```

Then, compile the Glu program to LLVM bitcode.

```bash
gluc -emit-llvm-bc main.glu -o main.bc
```

The `main.bc` file will include the LLVM bitcode for the Glu program only. The C++ library will remain in `liba.bc`.

Finally, link the LLVM bitcode files together using the Clang C++ compiler.

```bash
# Link the LLVM bitcode files
clang++ main.bc liba.bc -o main
```

## Importing Namespaces and Structs

C++ is a powerful language that supports namespaces and structs. Glu can import C++ namespaces and structs, allowing you to use them in your Glu programs.

Just like you can import Glu namespaces and structs, the `import` statement can be used to import ones defined in C++:

```cpp
namespace math {
    struct Point {
        int x;
        int y;

        static Point getDefault();
    };

    int add(int a, int b);
}

int math::add(int a, int b) {
    return a + b;
}

math::Point math::Point::getDefault() {
    return {0, 0};
}
```

Note that the code needs to use separate declarations and definitions, as within C++ source files, the compiler would omit unused inline definitions.

You can then import the `math` namespace and use the `Point` struct and `add` function in Glu:

```glu
import cpp_file::math;

func main() {
    let p: math::Point = math::Point::getDefault();
    let sum = math::add(5, 10);
    std::print("Point: (" + std::intToString(p.x) + ", " + std::intToString(p.y) + ")");
    std::print("Sum: " + std::intToString(sum));
}
```

Note that when importing C++ namespaces, functions within structs will be considered namespaces. Non-static functions within structs will have the member variable `this` added as the first parameter, as Glu does not have member functions.

## Viewing Imported Interfaces

Before using an imported C++ library, it can be useful to view the interface that Glu has generated for the library. This can be done using the `-print-interface` flag in the Glu compiler.

```bash
clang++ -g -c -emit-llvm cpp_file.cpp -o cpp_file.bc
gluc -print-interface cpp_file.bc
```

This will print the Glu interface of the imported C++ library to the terminal, allowing you to see the available functions, structs, and namespaces that can be used in your Glu program. Note that the interface may not always perfectly match the original C++ code, and might not compile in Glu as-is, but it provides a useful overview of the imported library's structure. 
