# `std::invoke`

## Introduction
`std::invoke` can call any callable object with a set of arguments, providing a uniform interface for invoking functions, function pointers, and member function pointers.

## Code Example
```cpp
#include <iostream>
#include <functional>

struct MyClass {
    void member_function(int x) {
        std::cout << "Member function called with: " << x << std::endl;
    }
};

void free_function(int x) {
    std::cout << "Free function called with: " << x << std::endl;
}

int main() {
    MyClass obj;
    auto lambda = [](int x) { std::cout << "Lambda called with: " << x << std::endl; };

    std::invoke(&MyClass::member_function, obj, 10);
    std::invoke(free_function, 20);
    std::invoke(lambda, 30);

    return 0;
}
```
