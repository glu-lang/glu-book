---
title: Export a Glu Library to C++
author: Emil
categories: [The Glu Basics]
order: 105
---

# Export a Glu Library to C++

We can not only import C++ libraries into Glu but also export Glu libraries to C++. This also allows you to import libraries from another language, and re-export them to a second language. In this example, we will export a Glu library to C++, but the same principle can be applied to other languages.

## Creating a Glu Library

Create a file named `libb.glu` in the project directory.

```bash
touch libb.glu
```

Open the `libb.glu` file in a text editor and add the following code.

```glu
@export func funcB() {
    std::print("Hello from Glu!");
}
```

This file defines a function `funcB` that we will export to C++. The `@export` annotation is used to mark the function as an exported function. Exported functions can be called from other languages, such as C++.

## Creating a C++ File

Create a file named `main.cpp` in the project directory.

```bash
touch main.cpp
```

Open the `main.cpp` file in a text editor and add the following code.

```cpp
#include "libb.hpp"

int main() {
    libb::funcB();
}
```

This file includes the Glu library `libb` and calls the function `funcB` from the library in the `main` function.

## Compiling the Program

We need multiple steps to compile the program:
 - Compile the Glu library to LLVM bitcode.
 - Generate a C++ header file from the Glu library.
 - Compile and link the C++ program.

First, compile the Glu library to LLVM bitcode.

```bash
gluc -emit-llvm libb.glu -o libb.bc
```

This will create an LLVM bitcode file named `libb.bc` from the Glu library `libb.glu`.

Next, generate a C++ header file from the Glu library.

```bash
gluc -emit-cpp-header libb.glu -o libb.hpp
```

This will generate a C++ header file named `libb.hpp` from the Glu library `libb.glu`. The header file contains the C++ declarations for the exported functions in the Glu library.

Finally, compile and link the C++ program.

```bash
clang++ main.cpp libb.bc -o main
```

This will compile the C++ program `main.cpp` and link it with the Glu library `libb.bc` to create an executable file named `main`.

You can then run the executable with `./main` on Linux or macOS.
