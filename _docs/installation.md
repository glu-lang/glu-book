---
title: Glu Installation
category: Basics
---

This guide will walk you through the steps to install the Glu programming language on your system. Glu is designed to be easy to install and set up, allowing you to start coding quickly. There are different methods to start using Glu, depending on your preferences.

## Using Docker

The easiest way to get started with Glu is by using Docker. If you don't have Docker installed, you can download it from the [official Docker website](https://www.docker.com/get-started).

Once you have Docker installed, you can run the following command in your terminal to pull the Glu Docker image, start a container, and mount your current directory into the container:

```bash
docker run -it --rm -v "$(pwd)":/workspace -w /workspace glu-lang/glu:latest /bin/bash
```

This will open a bash shell inside the Glu Docker container, with your current directory mounted at `/workspace`. You can now compile and run Glu programs from within the container.

## Compiling Docker Image

If you prefer to build the Docker image yourself, you can clone the Glu repository and build the image using the provided Dockerfile and build script.

```bash
git clone git@github.com:glu-lang/glu.git
cd glu
./docker.sh build
./docker.sh run
```

This will build the Docker image and start a container, similar to the previous method.

## Installing from Source

If you want to install Glu directly on your system without using Docker, you can compile it from source. First, clone the Glu repository:

```bash
git clone git@github.com:glu-lang/glu.git
cd glu
```

Make sure you have the necessary dependencies installed, including a C++ compiler and CMake. You need CMake, LLVM/Clang 18, Flex, and Bison installed on your system. If you need the demangler, you also need to install Rust and Cargo.

### Configuring Build on Ubuntu

To install dependencies on Ubuntu, you can use the following command:

```bash
sudo apt-get install build-essential llvm-20-dev llvm-20-tools libclang-20-dev flex bison cargo
```

To configure the build environment, run:

```bash
cmake -Bbuild -DCMAKE_BUILD_TYPE=Release
```

### Configuring Build on macOS

To install dependencies on macOS, you need Xcode command line tools and Homebrew. You can install the required packages using Homebrew:

```bash
brew install llvm@18 bison rust
```

To configure the build environment, you need to set the paths to Flex and Bison. Those depend on the paths where Xcode is installed and where Homebrew installs Bison. For most systems, you can run:

```bash
cmake -Bbuild -DCMAKE_BUILD_TYPE=Release -DFLEX_EXECUTABLE=/usr/bin/flex -DFLEX_INCLUDE_DIR=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include -DBISON_EXECUTABLE=/opt/homebrew/opt/bison/bin/bison
```

### Building Glu (Release)

Once you have the dependencies, you can compile Glu using CMake and the build options you want. For a release build, run:

```bash
cmake -Bbuild -DCMAKE_BUILD_TYPE=Release -DENABLE_ASAN=OFF
cmake --build build -j$(nproc)
```

This will create the Glu compiler executable as `build/tools/gluc/gluc`. The standard library will be located in `build/tools/lib/glu/`. The demangler executable will be located at `build/tools/glu-demangle/glu-demangle`.

If you only need the executable and standard library, you can run:

```bash
cmake --build build --target gluc stdlib
```

### Building Glu (Debug)

If you want to build it in debug mode, you can simply use the `./coverage.sh` script, which will configure and build Glu in debug mode, with assertions and asan enabled.

```bash
./coverage.sh
```

This will create the Glu compiler executable as `build/tools/gluc/gluc`.
