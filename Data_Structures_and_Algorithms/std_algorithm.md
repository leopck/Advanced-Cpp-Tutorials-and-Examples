# `std::algorithm`

## Introduction
The C++ Standard Library provides a set of algorithms that operate on containers. These algorithms are defined in the `<algorithm>` header.

## Code Example
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // std::for_each
    std::for_each(vec.begin(), vec.end(), [](int& n) { n *= 2; });

    // std::sort
    std::sort(vec.begin(), vec.end(), std::greater<int>());

    // std::find
    auto it = std::find(vec.begin(), vec.end(), 4);

    if (it != vec.end()) {
        std::cout << "Found: " << *it << std::endl;
    } else {
        std::cout << "Not found" << std::endl;
    }

    for (int n : vec) {
        std::cout << n << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
