# Using `std::filesystem` for File Operations

The `<filesystem>` library provides powerful tools for performing file and directory operations.

## Example: Using `std::filesystem` for File Operations

```cpp
#include <iostream>
#include <filesystem>
#include <fstream>

namespace fs = std::filesystem;

int main() {
    fs::path path = "example.txt";

    // Create a file
    {
        std::ofstream ofs(path);
        ofs << "Hello, filesystem!";
    }

    // Check if the file exists
    if (fs::exists(path)) {
        std::cout << path << " exists." << std::endl;
    }

    // Rename the file
    fs::rename(path, "example_renamed.txt");

    // Remove the file
    fs::remove("example_renamed.txt");

    // Check if the file was removed
    if (!fs::exists("example_renamed.txt")) {
        std::cout << "File successfully removed." << std::endl;
    }

    return 0;
}
```
