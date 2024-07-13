# Template Specialization

Template specialization allows you to define specific implementations for particular template arguments. There are two types: full specialization and partial specialization.

## Example: Full and Partial Specialization

### Full Specialization

```cpp
#include <iostream>

template<typename T>
class MyClass {
public:
    void print() {
        std::cout << "General template" << std::endl;
    }
};

// Full specialization for int
template<>
class MyClass<int> {
public:
    void print() {
        std::cout << "Specialized template for int" << std::endl;
    }
};

int main() {
    MyClass<double> obj1;
    MyClass<int> obj2;

    obj1.print(); // Output: General template
    obj2.print(); // Output: Specialized template for int

    return 0;
}
```

Partial Specialization

```cpp
#include <iostream>

template<typename T, typename U>
class MyClass {
public:
    void print() {
        std::cout << "General template" << std::endl;
    }
};

// Partial specialization for pointers
template<typename T>
class MyClass<T, T*> {
public:
    void print() {
        std::cout << "Partial specialization for pointer" << std::endl;
    }
};

int main() {
    MyClass<int, double> obj1;
    MyClass<int, int*> obj2;

    obj1.print(); // Output: General template
    obj2.print(); // Output: Partial specialization for pointer

    return 0;
}

```
