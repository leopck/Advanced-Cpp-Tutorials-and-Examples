# Advanced Template Techniques

Advanced template techniques allow for more sophisticated and powerful C++ code.

## Example: Variadic Templates

```cpp
#include <iostream>

// Variadic template function
template<typename T>
void print(T value) {
    std::cout << value << std::endl;
}

template<typename T, typename... Args>
void print(T first, Args... args) {
    std::cout << first << ", ";
    print(args...);
}

int main() {
    print(1, 2.5, "Hello", 'A'); // Outputs: 1, 2.5, Hello, A
    return 0;
}
```

Example: Fold Expressions

```cpp
#include <iostream>

// Fold expression
template<typename... Args>
auto sum(Args... args) {
    return (args + ...);
}

int main() {
    std::cout << sum(1, 2, 3, 4, 5) << std::endl; // Outputs: 15
    return 0;
}
```

