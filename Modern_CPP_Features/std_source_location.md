### `CppTutorials/Modern_CPP_Features/std_source_location.md`

```markdown
# Using `std::source_location` in C++

`std::source_location` is a utility introduced in C++20 that provides information about the source code location. It is particularly useful for logging, debugging, and error reporting. 

## Overview

- **`std::source_location`**: Provides information about the file name, function name, line number, and column number where the `std::source_location::current()` call is made.

## Key Concepts

- **File Name**: The name of the source file.
- **Function Name**: The name of the function.
- **Line Number**: The line number in the source file.
- **Column Number**: The column number in the source file.

## Example: Using `std::source_location` for Logging

### Step 1: Define a Logging Function

Create a logging function that takes a message and a `std::source_location` parameter with a default value.

```cpp
#include <iostream>
#include <string>
#include <source_location>

void log(const std::string& message, 
         const std::source_location& location = std::source_location::current()) {
    std::cout << "File: " << location.file_name()
              << " Line: " << location.line()
              << " Function: " << location.function_name()
              << " Message: " << message << std::endl;
}
```

### Step 2: Use the Logging Function

Use the logging function in different parts of your code to log messages with source location information.

```cpp
int main() {
    log("Starting the program");

    log("Performing an important operation");

    log("Program finished");
    return 0;
}
```

### Full Example Code

Here's the full example code:

```cpp
#include <iostream>
#include <string>
#include <source_location>

void log(const std::string& message, 
         const std::source_location& location = std::source_location::current()) {
    std::cout << "File: " << location.file_name()
              << " Line: " << location.line()
              << " Function: " << location.function_name()
              << " Message: " << message << std::endl;
}

int main() {
    log("Starting the program");

    log("Performing an important operation");

    log("Program finished");
    return 0;
}
```

## Conclusion

`std::source_location` is a powerful utility for adding detailed source code location information to your logs, debug output, and error messages. By using `std::source_location`, you can make it easier to track down issues and understand the flow of your program.

Feel free to integrate `std::source_location` into your own projects to enhance your logging and debugging capabilities.
```

