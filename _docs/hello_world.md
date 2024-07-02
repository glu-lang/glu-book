---
title: Writing a Hello World in Glu
category: The Glu Basics
---

# Hello World in Glu

The traditional hello world program is the first program that we will write in Glu. This program will print the string "Hello, World!" to the console.

## Creating a Project Directory

First, create a directory for the project.
    
```bash
mkdir hello-world
cd hello-world
```

## Creating a Source File

Create a file named `main.glu` in the project directory.

```bash
touch main.glu
```

## Writing the Hello World Program

Open the `main.glu` file in a text editor.
To make a hello world program in Glu, this is the code.

```glu
func main() {
    std::print("Hello, World!");
}
```

Save the file.

## Compiling the Program

To compile the program, run the following command in the terminal.

```bash
gluc main.glu
```

This will create an executable file named `main` in the project directory.

You can run the executable by running the following command.

```bash
# Linux or macOS
./main
# Windows
.\main.exe
```

## Anatomy of the Program

The program consists of a single function `main`. Any executable needs a function `main` to run.

```glu
func main() {

}
```

The main function is a special function that is called when the program is run. Here, we declare a function named `main` that takes no arguments and returns nothing. If the function had any arguments, they would be listed inside the parentheses. If the function returned a value, the return type would be specified after the parentheses, after an arrow `->`.

The body of the function is enclosed in curly braces `{}`. All function definitions in Glu have a body that contains the statements that are executed when the function is called.

In this case, the body contains a single statement that calls the `std::print` function to print the string "Hello, World!" to the console.

```glu
std::print("Hello, World!");
```

The `std::print` function is a standard library function that prints the given string to the console. The function is named `print` and is part of the `std` module, which is the standard library module that contains various utility functions. The `std` module is imported by default in all Glu programs, so we can use its functions without explicitly importing it. The `::` operator is used to access functions and variables in modules. In this case, we use it to access the `print` function in the `std` module.

The `std::print` function takes a single argument, which is the string to be printed. A new line character is automatically added to the end of the string when it is printed.

The string is enclosed in double quotes `""` to indicate that it is a string literal. String literals are sequences of characters enclosed in double quotes that represent text. They are used to represent text.

