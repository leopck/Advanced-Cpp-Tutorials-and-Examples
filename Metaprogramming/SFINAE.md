# SFINAE (Substitution Failure Is Not An Error)

SFINAE is a C++ feature that allows invalid substitutions in templates to be silently ignored during overload resolution, enabling more flexible and safer template programming.

## Example: Using std::enable_if for Function Overloading

```cpp
#include <iostream>
#include <type_traits>

template <typename T>
typename std::enable_if<std::is_integral<T>::value, void>::type print(T value) {
    std::cout << "Integral value: " << value << std::endl;
}

template <typename T>
typename std::enable_if<std::is_floating_point<T>::value, void>::type print(T value) {
    std::cout << "Floating-point value: " << value << std::endl;
}

int main() {
    print(42);       // Integral
    print(3.14);     // Floating-point

    return 0;
}
```
