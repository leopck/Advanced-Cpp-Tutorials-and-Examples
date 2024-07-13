# SFINAE (Substitution Failure Is Not An Error)

## Introduction
SFINAE is a feature of C++ templates that allows a template specialization to fail without causing a compilation error. It enables the creation of more specialized and flexible template code.

## Code Example
```cpp
#include <iostream>
#include <type_traits>

template<typename T>
std::enable_if_t<std::is_integral_v<T>, void> print(T value) {
    std::cout << "Integral value: " << value << std::endl;
}

template<typename T>
std::enable_if_t<std::is_floating_point_v<T>, void> print(T value) {
    std::cout << "Floating-point value: " << value << std::endl;
}

int main() {
    print(42);       // Integral
    print(3.14);     // Floating-point

    return 0;
}
```
