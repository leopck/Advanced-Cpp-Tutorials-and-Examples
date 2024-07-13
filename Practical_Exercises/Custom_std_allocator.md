# Practical Exercises

## Exercise 1: Implement a Custom `std::allocator` for Performance Optimization

Create a custom allocator that logs memory allocations and deallocations for performance optimization.

```cpp
#include <iostream>
#include <memory>
#include <vector>

template<typename T>
class LoggingAllocator {
public:
    using value_type = T;

    LoggingAllocator() = default;

    template<typename U>
    LoggingAllocator(const LoggingAllocator<U>&) {}

    T* allocate(std::size_t n) {
        std::cout << "Allocating " << n * sizeof(T) << " bytes\n";
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }

    void deallocate(T* p, std::size_t n) {
        std::cout << "Deallocating " << n * sizeof(T) << " bytes\n";
        ::operator delete(p);
    }
};

template<typename T, typename U>
bool operator==(const LoggingAllocator<T>&, const LoggingAllocator<U>&) { return true; }

template<typename T, typename U>
bool operator!=(const LoggingAllocator<T>&, const LoggingAllocator<U>&) { return false; }

int main() {
    std::vector<int, LoggingAllocator<int>> vec = {1, 2, 3, 4, 5};
    vec.push_back(6);

    for (const auto& v : vec) {
        std::cout << v << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
