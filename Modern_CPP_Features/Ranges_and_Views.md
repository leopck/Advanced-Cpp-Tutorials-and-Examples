# Ranges and Views

C++20 introduces the `<ranges>` library, which provides powerful tools for working with ranges, including transformations, filtering, and projections.

## Example: Using `std::ranges` for Range-Based Algorithms

```cpp
#include <iostream>
#include <vector>
#include <ranges>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5, 6};

    auto even = [](int n) { return n % 2 == 0; };
    auto square = [](int n) { return n * n; };

    for (int n : vec | std::ranges::views::filter(even) | std::ranges::views::transform(square)) {
        std::cout << n << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
