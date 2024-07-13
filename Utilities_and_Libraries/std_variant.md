# Using `std::variant` for Type-Safe Unions

`std::variant` allows you to safely store a value of one of several types.

## Example: Using `std::variant` for Type-Safe Unions

```cpp
#include <iostream>
#include <variant>
#include <string>

using VariantType = std::variant<int, double, std::string>;

void print_variant(const VariantType& v) {
    std::visit([](auto&& arg) { std::cout << arg << std::endl; }, v);
}

int main() {
    VariantType v = 42;
    print_variant(v);

    v = 3.14;
    print_variant(v);

    v = std::string("Hello, variant");
    print_variant(v);

    return 0;
}
```
