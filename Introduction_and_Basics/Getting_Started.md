# Getting Started with C++

Welcome to the world of C++ programming! This guide will help you get started with the basics of C++ and set up your development environment.

## Installing a Compiler

Before you can start writing C++ programs, you need to have a C++ compiler installed on your system. Some popular options are:

- GCC (GNU Compiler Collection) for Linux and Windows
- Clang for macOS and Linux
- MSVC (Microsoft Visual C++) for Windows

## Writing Your First Program

Let's write a simple C++ program that prints "Hello, World!" to the console.

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}

Compiling and Running the Program

To compile and run the program:

    Save the code in a file named hello.cpp.
    Open a terminal or command prompt.
    Navigate to the directory where hello.cpp is saved.
    Run the following command to compile the program:

bash

g++ -o hello hello.cpp

    Run the compiled program:

bash

./hello

You should see the output: Hello, World!
