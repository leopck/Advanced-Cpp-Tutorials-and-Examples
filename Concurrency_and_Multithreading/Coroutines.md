# Coroutines

Coroutines are a general control structure whereby flow control is cooperatively passed between two different routines without returning.

## Example: Implementing and Using Coroutines

```cpp
#include <iostream>
#include <coroutine>

struct ReturnObject {
    struct promise_type;
    using handle_type = std::coroutine_handle<promise_type>;

    struct promise_type {
        ReturnObject get_return_object() { return {handle_type::from_promise(*this)}; }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_always final_suspend() noexcept { return {}; }
        void unhandled_exception() { std::terminate(); }
        void return_void() {}
    };

    handle_type coro;

    ReturnObject(handle_type h) : coro(h) {}
    ~ReturnObject() {
        if (coro) coro.destroy();
    }

    void resume() {
        if (!coro.done()) coro.resume();
    }
};

ReturnObject coroutine() {
    std::cout << "Step 1" << std::endl;
    co_await std::suspend_always{};
    std::cout << "Step 2" << std::endl;
}

int main() {
    auto handle = coroutine();
    std::cout << "Before resuming coroutine" << std::endl;
    handle.resume();
    std::cout << "After resuming coroutine" << std::endl;
    handle.resume();
    std::cout << "Coroutine finished" << std::endl;

    return 0;
}
```
