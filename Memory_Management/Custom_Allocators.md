# Custom Allocators

Custom allocators can optimize memory allocation for specific use cases, improving performance and memory usage.

## Example: Implementing and Using Custom Allocators

```cpp
#include <iostream>
#include <memory>

template<typename T>
class CustomAllocator {
public:
    using value_type = T;

    CustomAllocator() = default;

    template<typename U>
    CustomAllocator(const CustomAllocator<U>&) {}

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
bool operator==(const CustomAllocator<T>&, const CustomAllocator<U>&) { return true; }

template<typename T, typename U>
bool operator!=(const CustomAllocator<T>&, const CustomAllocator<U>&) { return false; }

int main() {
    std::vector<int, CustomAllocator<int>> vec = {1, 2, 3, 4, 5};
    vec.push_back(6);

    for (const auto& v : vec) {
        std::cout << v << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
