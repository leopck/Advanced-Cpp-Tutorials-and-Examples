# Metaprogramming in C++

Metaprogramming allows you to write code that manipulates other code at compile time. This can lead to more efficient and flexible programs.

## Example: Compile-Time Factorial Calculation

```cpp
#include <iostream>

// Compile-time factorial calculation
template<int N>
struct Factorial {
    static constexpr int value = N * Factorial<N - 1>::value;
};

template<>
struct Factorial<0> {
    static constexpr int value = 1;
};

int main() {
    std::cout << "Factorial of 5: " << Factorial<5>::value << std::endl; // Outputs 120
    return 0;
}
```

Example: Conditional Compilation with `std::enable_if`

```cpp
#include <iostream>
#include <type_traits>

// Conditional function with std::enable_if
template<typename T>
typename std::enable_if<std::is_integral<T>::value, void>::type print(T value) {
    std::cout << "Integral value: " << value << std::endl;
}

template<typename T>
typename std::enable_if<std::is_floating_point<T>::value, void>::type print(T value) {
    std::cout << "Floating-point value: " << value << std::endl;
}

int main() {
    print(42); // Outputs: Integral value: 42
    print(3.14); // Outputs: Floating-point value: 3.14
    return 0;
}
```
