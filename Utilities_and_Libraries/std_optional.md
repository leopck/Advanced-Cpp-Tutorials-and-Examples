# Using `std::optional` for Safe Pointer-Like Behavior

`std::optional` can be used to represent an object that may or may not be present, providing a safer alternative to raw pointers.

## Example: Safe Pointer-Like Behavior with `std::optional`

```cpp
#include <iostream>
#include <optional>

struct MyStruct {
    int value;
};

std::optional<MyStruct> create_struct(bool should_create) {
    if (should_create) {
        return MyStruct{42};
    }
    return std::nullopt;
}

int main() {
    auto opt_struct = create_struct(true);
    if (opt_struct) {
        std::cout << "Struct created with value: " << opt_struct->value << std::endl;
    } else {
        std::cout << "Struct not created" << std::endl;
    }

    opt_struct = create_struct(false);
    if (opt_struct) {
        std::cout << "Struct created with value: " << opt_struct->value << std::endl;
    } else {
        std::cout << "Struct not created" << std::endl;
    }

    return 0;
}
```
