# Template Metaprogramming

Template metaprogramming is a technique used to perform computations at compile-time using templates. This can optimize runtime performance by shifting computations to the compiler.

## Example: Compile-Time Computation

### Compile-Time Factorial Calculation

```cpp
#include <iostream>
#include <type_traits>

template<int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value;
};

template<>
struct Factorial<0> {
    static const int value = 1;
};

int main() {
    std::cout << "Factorial of 5: " << Factorial<5>::value << std::endl; // Output: 120
    return 0;
}
```

Compile-Time Fibonacci Calculation

```cpp
#include <iostream>

template<int N>
struct Fibonacci {
    static const int value = Fibonacci<N - 1>::value + Fibonacci<N - 2>::value;
};

template<>
struct Fibonacci<0> {
    static const int value = 0;
};

template<>
struct Fibonacci<1> {
    static const int value = 1;
};

int main() {
    std::cout << "Fibonacci of 10: " << Fibonacci<10>::value << std::endl; // Output: 55
    return 0;
}

```
