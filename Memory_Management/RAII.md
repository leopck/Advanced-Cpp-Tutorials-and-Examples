# RAII (Resource Acquisition Is Initialization)

## Introduction
RAII is a programming idiom used to manage resource allocation and deallocation. Resources are acquired in a constructor and released in a destructor.

## Code Example
```cpp
#include <iostream>
#include <stdexcept>

class Resource {
public:
    Resource() {
        data = new int[100];
        std::cout << "Resource acquired\n";
    }

    ~Resource() {
        delete[] data;
        std::cout << "Resource released\n";
    }

private:
    int* data;
};

void useResource() {
    Resource res;
    // Perform operations with res
    if (true) { // Simulate an error
        throw std::runtime_error("An error occurred");
    }
}

int main() {
    try {
        useResource();
    } catch (const std::exception& e) {
        std::cout << e.what() << std::endl;
    }

    return 0;
}
```
