# Using `std::any` for Type Erasure

`std::any` allows you to store values of any type and retrieve them later, providing a type-safe alternative to `void*`.

## Example: Type Erasure with `std::any`

```cpp
#include <iostream>
#include <any>
#include <string>

void print_any(const std::any& a) {
    if (a.type() == typeid(int)) {
        std::cout << "Integer: " << std::any_cast<int>(a) << std::endl;
    } else if (a.type() == typeid(double)) {
        std::cout << "Double: " << std::any_cast<double>(a) << std::endl;
    } else if (a.type() == typeid(std::string)) {
        std::cout << "String: " << std::any_cast<std::string>(a) << std::endl;
    } else {
        std::cout << "Unknown type\n";
    }
}

int main() {
    std::any a = 42;
    print_any(a);

    a = 3.14;
    print_any(a);

    a = std::string("Hello, std::any!");
    print_any(a);

    try {
        std::cout << "Incorrect cast: " << std::any_cast<int>(a) << std::endl;
    } catch (const std::bad_any_cast& e) {
        std::cout << "Caught bad_any_cast: " << e.what() << std::endl;
    }

    return 0;
}
```
