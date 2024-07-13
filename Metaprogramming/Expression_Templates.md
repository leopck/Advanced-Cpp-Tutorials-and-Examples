# Expression Templates

Expression templates are a C++ technique used to optimize mathematical expressions by delaying their evaluation until the entire expression is known, reducing the number of temporary objects.

## Example: Optimizing Mathematical Expressions

```cpp
#include <iostream>

template<typename T>
struct Expression {
    T value;
    Expression(T val) : value(val) {}
};

template<typename T>
Expression<T> operator+(const Expression<T>& lhs, const Expression<T>& rhs) {
    return Expression<T>(lhs.value + rhs.value);
}

template<typename T>
Expression<T> operator*(const Expression<T>& lhs, const Expression<T>& rhs) {
    return Expression<T>(lhs.value * rhs.value);
}

template<typename T>
std::ostream& operator<<(std::ostream& os, const Expression<T>& expr) {
    os << expr.value;
    return os;
}

int main() {
    Expression<int> a(5);
    Expression<int> b(10);
    Expression<int> c(15);

    auto result = (a + b) * c;
    std::cout << "Result: " << result << std::endl;

    return 0;
}
```
