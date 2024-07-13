# Concepts

Concepts allow you to specify requirements for template parameters, improving code readability and error messages.

## Example: Defining and Using Concepts

```cpp
#include <iostream>
#include <concepts>

template <typename T>
concept Incrementable = requires(T x) {
    ++x;
    x++;
};

template <Incrementable T>
void increment(T& x) {
    ++x;
}

int main() {
    int a = 5;
    increment(a);
    std::cout << "Incremented value: " << a << std::endl;

    // Uncommenting the following lines would result in a compile-time error
    // std::string s = "hello";
    // increment(s);

    return 0;
}
```
