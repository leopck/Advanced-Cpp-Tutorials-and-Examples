# Templates in C++

Templates allow you to write generic and reusable code. They enable functions and classes to operate with any data type without being rewritten for each one.

## Example: Function Templates

```cpp
#include <iostream>

// Function template
template<typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    std::cout << add(3, 4) << std::endl; // Outputs 7
    std::cout << add(3.5, 4.5) << std::endl; // Outputs 8
    return 0;
}
```
Example: Class Templates

```cpp
#include <iostream>

// Class template
template<typename T>
class Box {
public:
    Box(T value) : value(value) {}
    T get_value() const { return value; }
private:
    T value;
};

int main() {
    Box<int> int_box(123);
    Box<std::string> string_box("Hello");

    std::cout << int_box.get_value() << std::endl; // Outputs 123
    std::cout << string_box.get_value() << std::endl; // Outputs Hello

    return 0;
}
```
