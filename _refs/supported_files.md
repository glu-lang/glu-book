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
