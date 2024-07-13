# Three-Way Comparison (Spaceship Operator)

C++20 introduces the three-way comparison operator `<=>`, which simplifies the implementation of comparisons and ordering.

## Example: Implementing and Using `<=>` Operator

```cpp
#include <iostream>
#include <compare>

class Version {
public:
    Version(int major, int minor, int patch)
        : major_(major), minor_(minor), patch_(patch) {}

    auto operator<=>(const Version&) const = default;

    void print() const {
        std::cout << major_ << '.' << minor_ << '.' << patch_ << std::endl;
    }

private:
    int major_;
    int minor_;
    int patch_;
};

int main() {
    Version v1(1, 0, 0);
    Version v2(1, 0, 1);

    if (v1 < v2) {
        std::cout << "v1 is less than v2" << std::endl;
    } else if (v1 > v2) {
        std::cout << "v1 is greater than v2" << std::endl;
    } else {
        std::cout << "v1 is equal to v2" << std::endl;
    }

    return 0;
}
```
