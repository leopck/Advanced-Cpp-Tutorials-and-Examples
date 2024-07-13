# Implementing a Custom `std::function` Replacement

A custom `std::function` replacement can be optimized for specific use cases, such as avoiding heap allocations for small callable objects.

## Example: Custom `std::function` Replacement

```cpp
#include <iostream>
#include <utility>
#include <cstring>

template <typename Signature>
class SmallFunction;

template <typename Ret, typename... Args>
class SmallFunction<Ret(Args...)> {
    using Storage = std::aligned_storage_t<32>;
    using Invoker = Ret(*)(Storage&, Args&&...);

    template <typename F>
    static Ret invoke(Storage& storage, Args&&... args) {
        return (*reinterpret_cast<F*>(&storage))(std::forward<Args>(args)...);
    }

public:
    template <typename F>
    SmallFunction(F&& f) {
        new(&storage) F(std::forward<F>(f));
        invoker = &invoke<std::decay_t<F>>;
    }

    Ret operator()(Args... args) {
        return invoker(storage, std::forward<Args>(args)...);
    }

private:
    Storage storage;
    Invoker invoker;
};

int main() {
    SmallFunction<void()> f = []() { std::cout << "Hello, SmallFunction!" << std::endl; };
    f();

    return 0;
}
```
