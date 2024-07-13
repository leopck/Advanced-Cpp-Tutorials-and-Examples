# Tag Dispatch

## Introduction
Tag dispatch is a technique used to select different function overloads based on compile-time information.

## Code Example
```cpp
#include <iostream>
#include <type_traits>

struct IntegralTag {};
struct FloatingPointTag {};

template<typename T>
void print(T value, IntegralTag) {
    std::cout << "Integral: " << value << std::endl;
}

template<typename T>
void print(T value, FloatingPointTag) {
    std::cout << "Floating-point: " << value << std::endl;
}

template<typename T>
void print(T value) {
    if constexpr (std::is_integral_v<T>) {
        print(value, IntegralTag{});
    } else if constexpr (std::is_floating_point_v<T>) {
        print(value, FloatingPointTag{});
    }
}

int main() {
    print(42);
    print(3.14);
    return 0;
}
```
