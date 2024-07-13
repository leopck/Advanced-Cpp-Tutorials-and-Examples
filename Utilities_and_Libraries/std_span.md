# Using `std::span` for Lightweight Range Access

`std::span` provides a lightweight, non-owning view of a contiguous sequence of elements.

## Example: Using `std::span` to View Array Elements

```cpp
#include <iostream>
#include <span>

void print_span(std::span<int> s) {
    for (int x : s) {
        std::cout << x << " ";
    }
    std::cout << std::endl;
}

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    print_span(arr); // Use entire array

    print_span({arr + 1, 3}); // Use part of the array

    return 0;
}
