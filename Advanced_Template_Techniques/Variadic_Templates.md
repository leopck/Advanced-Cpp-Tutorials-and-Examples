# Variadic Templates

Variadic templates allow functions and classes to accept an arbitrary number of arguments. They are a powerful feature in C++ for creating flexible and reusable code.

## Example: Variadic Template Functions and Classes

### Variadic Template Function

```cpp
#include <iostream>

template<typename... Args>
void print(Args... args) {
    (std::cout << ... << args) << std::endl;
}

int main() {
    print(1, 2, 3.5, "Hello");
    return 0;
}
```

Variadic Template Class

```cpp
#include <iostream>
#include <tuple>

template<typename... Args>
class MyClass {
public:
    MyClass(Args... args) : data(args...) {}

    void print() {
        printTuple(data);
    }

private:
    std::tuple<Args...> data;

    template<std::size_t... Is>
    void printTupleHelper(const std::tuple<Args...>& t, std::index_sequence<Is...>) {
        ((std::cout << std::get<Is>(t) << " "), ...);
    }

    void printTuple(const std::tuple<Args...>& t) {
        printTupleHelper(t, std::index_sequence_for<Args...>{});
    }
};

int main() {
    MyClass<int, double, std::string> obj(1, 3.14, "Hello");
    obj.print();
    return 0;
}
```
