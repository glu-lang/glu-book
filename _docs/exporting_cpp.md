---
title: Export a Glu Library to C++ (Coming Soon)
category: Imports and Modules
---

We can not only import C++ libraries into Glu but also implement C++ functions in Glu. This also allows you to import libraries from another language, and re-export them to a second language. In this example, we will export a Glu library to C++, but the same principle can be applied to other languages.

## Creating a C++ File

Create a file named `main.cpp` in the project directory.

```bash
touch main.cpp
```

Open the `main.cpp` file in a text editor and add the following code.

```cpp
[[gnu::weak]] void funcB();

int main() {
    funcB();
}
```

This file declares a function `funcB` that we then call from the `main` function. The `[[gnu::weak]]` attribute tells the C++ compiler that the function is weakly linked, meaning that it can be overridden by a function with the same name in another translation unit. This is necessary because the function will be overridden by our Glu implementation. The function is empty instead of a prototype because otherwise, the C++ compiler would not emit debug information for it, which the Glu compiler needs to generate the necessary glue code.

## Implementing an external function from Glu

Create a file named `libb.glu` in the project directory.

```bash
touch libb.glu
```

Open the `libb.glu` file in a text editor and add the following code.

```glu
// This file implements function funcB from main.cpp
@implement import main::funcB;

func funcB() {
    std::print("Hello from Glu!");
}
```

This file defines a function `funcB` that we will export to C++. The `@implement` annotation on the import statement tells the Glu compiler to match the functions in the current Glu library with the imported declaration.

## Compiling the Program

First, compile the C++ program to LLVM bitcode, which the Glu compiler can understand.

```bash
clang++ -g -c -emit-llvm main.cpp -o main.bc
```

This will create an LLVM bitcode file named `main.bc` from the C++ source file `main.cpp`. The `-g` flag is used to include debugging information in the generated bitcode file, which the Glu compiler will use to import the C++ code correctly.

Then, compile the Glu library to LLVM bitcode.

```bash
gluc -emit-llvm-bc libb.glu -o libb.bc
```

Then, compile and link the C++ program.

```bash
clang++ main.cpp libb.bc -o main
```

This will compile the C++ program `main.cpp` and link it with the Glu library `libb.bc` to create an executable file named `main`. The `libb.bc` file contains the correct implementation of the `funcB` function in Glu, which will override the weakly defined function in the C++ program.

You can then run the executable with `./main` on Linux or macOS, or `.\main.exe` on Windows.
