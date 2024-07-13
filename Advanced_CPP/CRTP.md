# CRTP (Curiously Recurring Template Pattern)

## Introduction
CRTP is a powerful pattern that allows a base class to use the derived class as a template parameter, enabling static polymorphism.

## Code Example
```cpp
#include <iostream>

template<typename Derived>
class Base {
public:
    void interface() {
        static_cast<Derived*>(this)->implementation();
    }
};

class Derived1 : public Base<Derived1> {
public:
    void implementation() {
        std::cout << "Derived1 implementation" << std::endl;
    }
};

class Derived2 : public Base<Derived2> {
public:
    void implementation() {
        std::cout << "Derived2 implementation" << std::endl;
    }
};

int main() {
    Derived1 d1;
    Derived2 d2;

    d1.interface();
    d2.interface();

    return 0;
}
```
