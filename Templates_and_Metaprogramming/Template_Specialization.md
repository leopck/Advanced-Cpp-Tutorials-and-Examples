# Template Specialization

Template specialization allows you to define different implementations of a template for specific types.

## Example: Function Template Specialization

```cpp
#include <iostream>

// Generic template
template<typename T>
void print(T value) {
    std::cout << "Generic: " << value << std::endl;
}

// Specialization for int
template<>
void print(int value) {
    std::cout << "Integer: " << value << std::endl;
}

int main() {
    print(5); // Outputs: Integer: 5
    print(3.14); // Outputs: Generic: 3.14
    return 0;
}
```

Example: Class Template Specialization

```cpp
#include <iostream>

// Generic template
template<typename T>
class Box {
public:
    Box(T value) : value(value) {}
    void print() const { std::cout << "Generic: " << value << std::endl; }
private:
    T value;
};

// Specialization for int
template<>
class Box<int> {
public:
    Box(int value) : value(value) {}
    void print() const { std::cout << "Integer: " << value << std::endl; }
private:
    int value;
};

int main() {
    Box<int> int_box(123);
    Box<double> double_box(3.14);

    int_box.print(); // Outputs: Integer: 123
    double_box.print(); // Outputs: Generic: 3.14

    return 0;
}
```
