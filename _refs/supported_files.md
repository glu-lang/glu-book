---
title: Supported File Formats
category: Appendix
---

The Glu programming language supports several file formats as inputs and as importable libraries. Below is a list of the supported file formats. For importable formats, you can use the `import` statement in Glu to include them in your Glu programs, which will automatically search for the appropriate file based on the name, and with the supported file extensions, in the order listed below. For command line inputs, you can provide files of these formats as input to the Glu compiler, for the given functionalities.

| File Format | Usage | Importable? | Command Line Input? |
|-------------|-------|-------------|---------------------|
| Glu source code (`.glu`) | Full Support: Glu modules | Yes | Yes (All) |
| Clang Header Files (`.h`, `.hpp`, etc.) | Clang Import: C/C++ header files | Planned | Planned |
| LLVM Bitcode (`.bc`) | IR Import: LLVM IR binary format with Debug Info, generated from a supported compiler (LLVM 20) | Yes | Yes (Interface Only) |
| LLVM IR (`.ll`) | IR Import: LLVM IR textual format with Debug Info, generated from a supported compiler (LLVM 19-20) | Yes | Yes (Interface Only) |
| Clang Source Code (`.c`, `.cpp`, etc.) | Compilation + IR Import: C/C++ source files | Planned | Planned |
| Rust Source Code (`.rs`) | Compilation + IR Import: Rust source files | Planned | Planned |

## Supported Languages for IR Import

Below are the supported programming languages for importing LLVM Bitcode (`.bc`) and LLVM IR (`.ll`) files into Glu, along with the required compiler versions. Other languages may be supported, but these are the ones that have been tested and verified to work with the Glu compiler.

### C and C++ via Clang

**Supported Versions (BC):** Clang 20  
**Supported Versions (LL):** Clang 19-20 or Apple Clang 17+

For importing C and C++ source files (not header files), you need to compile them to LLVM bitcode (`.bc`) or LLVM IR (`.ll`) using Clang with debug information enabled. The following commands can be used:

```bash
# C to BC (Clang 20)
clang -g -c -emit-llvm source.c -o source.bc
# C to LL (Clang 19-20 or Apple Clang 17+)
clang -g -S -emit-llvm source.c -o source.ll
# C++ to BC (Clang 20)
clang++ -g -c -emit-llvm source.cpp -o source.bc
# C++ to LL (Clang 19-20 or Apple Clang 17+)
clang++ -g -S -emit-llvm source.cpp -o source.ll
```

### D via LDC

**Supported Versions (BC):** LDC 1.41  
**Supported Versions (LL):** LDC 1.40-1.41+

For importing D source files, you need to compile them to LLVM bitcode (`.bc`) or LLVM IR (`.ll`) using LDC with debug information enabled. The following commands can be used:

```bash
# D to BC (LDC 1.41)
ldc2 -c --output-bc -g source.d -of=source.bc
# D to LL (LDC 1.40-1.41+)
ldc2 -c --output-ll -g source.d -of=source.ll
```

You might want to link using `ldc2` to link with the D standard library. This can be done by compiling all of your code to object files and then linking them together with ldc2:

```bash
# Compile D code to LLVM IR
ldc2 -c --output-ll -g source.d -of=source.ll
# Compile Glu code
gluc -c main.glu -o main.o
# Compile D code to object file
ldc2 -c --output-obj -g source.d -of=source.o
# Link everything together
ldc2 main.o source.o -of=output_executable
```

### Odin
  
**Supported Versions (LL):** Odin 2025-11

For importing Odin source files, you need to compile them to LLVM IR (`.ll`) using the Odin compiler with debug information enabled. The following command can be used:

```bash
# Odin to LL (Odin 2025-11)
odin build source.odin -file -debug -build-mode:llvm-ir
```

When using the Odin standard library, it needs to be linked in manually, as follows:

```bash
# Compile Glu code
gluc -c main.glu -o main.o
# Remove Odin entry point if your Glu code has its own main function
rm source-runtime-entry*.ll
# Link Glu with Odin Standard Library
clang main.o source-*.ll -o output_executable
```

### Rust via rustc

**Supported Versions (BC):** Rust 1.87-1.90  
**Supported Versions (LL):** Rust 1.82-1.90

For importing Rust source files, you need to compile them to LLVM bitcode (`.bc`) or LLVM IR (`.ll`) using `rustc` with debug information enabled. The following commands can be used:

```bash
# Rust to BC (Rust 1.87-1.90)
rustc --crate-type=lib -g --emit=llvm-bc source.rs -o source.bc
# Rust to LL (Rust 1.82-1.90)
rustc --crate-type=lib -g --emit=llvm-ir source.rs -o source.ll
```


### Swift via swiftc

**Supported Versions (LL):** Swift 6.2

For importing Swift source files, you need to compile them to LLVM IR (`.ll`) using the Swift compiler with debug information enabled. The following command can be used:

```bash
# Swift to LL (Swift 6.2) (remove parse-as-library if your swift file is the main file)
swiftc -parse-as-library -emit-ir -g -gdwarf-types -module-name source source.swift -o source.ll`
```

To link with the Swift standard library, the command used depends on the platform and your Swift installation. For macOS, the Swift standard library is automatically linked by Apple Clang:

```bash
gluc main.glu -o output_executable --linker /usr/bin/clang
```

For Linux, you need to specify the name and path of the Swift standard library manually. To find the path of the Swift standard library, you can use:

```bash
swiftc -parse-as-library -module-name source source.swift -use-ld=/bin/echo
```

This will give you all the linker flags needed to link with the Swift standard library. You can then use those flags with `ld` to link your Glu code with the Swift standard library. An example can be found [here](https://github.com/glu-lang/glu/blob/main/test/functional/IRDec/Swift/import-swift.glu).

### Zig

**Supported Versions (BC):** Zig 0.15  
**Supported Versions (LL):** Zig 0.14-0.15

For importing Zig source files, you need to compile them to LLVM bitcode (`.bc`) or LLVM IR (`.ll`) using `zig` with debug information enabled. The following commands can be used:

```bash
# Zig to BC (Zig 0.15)
zig build-obj -fllvm -fno-strip %s -femit-llvm-bc=%t.bc
# Zig to LL (Zig 0.14-0.15)
zig build-obj -fllvm -fno-strip %s -femit-llvm-ir=%t.ll
```
