
# Type Traits

Type traits are a C++ feature that allows querying and modifying type information at compile-time, enabling more flexible and type-safe code.

## Example: Using Type Traits for Compile-Time Type Information

```cpp
#include <iostream>
#include <type_traits>

template<typename T>
void print_type_info() {
    if (std::is_integral<T>::value) {
        std::cout << "Type is integral" << std::endl;
    } else if (std::is_floating_point<T>::value) {
        std::cout << "Type is floating-point" << std::endl;
    } else {
        std::cout << "Type is unknown" << std::endl;
    }
}

int main() {
    print_type_info<int>();
    print_type_info<double>();
    print_type_info<std::string>();

    return 0;
}
```
