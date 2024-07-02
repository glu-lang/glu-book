---
title: Import a C++ Library in Glu
category: Understanding Imports and Namespaces
---

# Import a C++ Library in Glu

The Glu programming language is designed to be used in conjunction with other languages and tools. In this section, we will see how to use Glu in a project that involves multiple languages and tools.

## Creating a Project Directory

First, create a directory for the project.

```bash
mkdir glue-project
cd glue-project
```

## Creating a C++ File

Create a file named `liba.hpp` in the project directory.

```bash
touch liba.hpp
```

Open the `liba.hpp` file in a text editor and add the following code.

```cpp
#include <iostream>

inline void funcA() {
    std::cout << "Hello from C++!" << std::endl;
}
```

Save the file. This file contains a function `funcA` that we will call from Glu. Note that this file has a `.hpp` extension, which is a common convention for C++ header files, used for libraries.

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

This file imports the C++ library `liba` from the file `liba.hpp` and calls the function `funcA` from the library in the `main` function.

The `import` statement is used to import external modules or libraries into a Glu program. In this case, we import the `liba` module, which corresponds to the C++ library defined in the `liba.hpp` file. The `::` operator is used to access functions and variables in modules.

To compile the program, run the following command in the terminal.

```bash
clang++ -g -emit-llvm liba.hpp -o liba.bc
gluc main.glu -I .
```

This will create an LLVM bitcode file named `liba.bc` from the C++ header file `liba.hpp`, using the Clang compiler. The `-g` flag is used to include debugging information in the generated bitcode file, which the Glu compiler will use to import the C++ code correctly.

Then, we compile the Glu program `main.glu` using the `gluc` compiler. The `-I .` flag is used to specify the include path: the current directory. This is necessary to tell the compiler where to find the `liba` module. The `-I` flag can be used for including bitcode files, LLVM IR files, glu files, and directories containing glu files.

The `gluc` compiler will generate an executable file named `main` in the project directory, which contains the Glu program linked with the C++ library.

You can run the executable with `./main` on Linux or macOS, or `.\main.exe` on Windows.

The output should be:
```
Hello from C++!
```

This demonstrates how Glu can be used in conjunction with other languages and tools to create a project that involves multiple components written in different languages. Glu can be used to glue together components written in different languages, providing a seamless integration between them.

## Linking separately

If you want to compile the Glu program and the C++ library separately, you can do so by following these steps.

First, compile the C++ library to LLVM bitcode.

```bash
clang++ -g -emit-llvm liba.hpp -o liba.bc
```

Then, compile the Glu program to LLVM bitcode.

```bash
gluc -I . -emit-llvm main.glu -o main.bc
```

The `main.bc` file will include the LLVM bitcode for both the Glu program and the C++ library, in a single compilation unit. By adding the `-O` flag, you can optimize the LLVM bitcode which will completely inline the C++ code into the Glu code.

If, instead, you want to keep the C++ code separate from the Glu code, you can keep the LLVM bitcode files separate and link them together later:

```bash
# Compile the C++ library to LLVM bitcode
clang++ -g -emit-llvm liba.hpp -o liba.bc
# Compile the Glu program to LLVM bitcode, without including the C++ code
gluc -I . -import-as-declare -emit-llvm main.glu -o main.bc
# Link the LLVM bitcode files
clang++ main.bc liba.bc -o main
```
