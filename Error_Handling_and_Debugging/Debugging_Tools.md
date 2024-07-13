# Debugging Tools

Debugging tools and techniques help developers identify and fix issues in their code more efficiently. `std::source_location` is a useful tool for obtaining source code information.

## Example: Using std::source_location for Debugging

```cpp
#include <iostream>
#include <source_location>

void log(const std::string& message, const std::source_location& location = std::source_location::current()) {
    std::cout << "File: " << location.file_name() << " Line: " << location.line()
              << " Function: " << location.function_name() << " Message: " << message << std::endl;
}

int main() {
    log("Hello, source_location!");

    return 0;
}
```
