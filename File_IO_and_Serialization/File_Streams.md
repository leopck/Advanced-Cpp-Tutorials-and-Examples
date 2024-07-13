# File Streams

File streams provide a way to read from and write to files in C++. The `fstream` library includes classes for file stream operations.

## Example: Reading and Writing Files

### Writing to a File

```cpp
#include <iostream>
#include <fstream>

int main() {
    std::ofstream outfile("example.txt");
    if (!outfile) {
        std::cerr << "Error opening file for writing" << std::endl;
        return 1;
    }
    outfile << "Hello, file!" << std::endl;
    outfile.close();
    return 0;
}
```

### Reading from a File

```cpp
#include <iostream>
#include <fstream>
#include <string>

int main() {
    std::ifstream infile("example.txt");
    if (!infile) {
        std::cerr << "Error opening file for reading" << std::endl;
        return 1;
    }
    std::string line;
    while (std::getline(infile, line)) {
        std::cout << line << std::endl;
    }
    infile.close();
    return 0;
}
```
