Create a coroutine that generates prime numbers lazily.

```cpp
#include <iostream>
#include <coroutine>

struct Generator {
    struct promise_type;
    using handle_type = std::coroutine_handle<promise_type>;

    struct promise_type {
        int current_value;

        Generator get_return_object() { return Generator{handle_type::from_promise(*this)}; }
        std::suspend_always initial_suspend() { return {}; }
        std::suspend_always final_suspend() noexcept { return {}; }
        std::suspend_always yield_value(int value) {
            current_value = value;
            return {};
        }
        void return_void() {}
        void unhandled_exception() { std::terminate(); }
    };

    handle_type coro;

    Generator(handle_type h) : coro(h) {}
    ~Generator() {
        if (coro) coro.destroy();
    }

    bool move_next() {
        coro.resume();
        return !coro.done();
    }

    int current_value() { return coro.promise().current_value; }
};

bool is_prime(int n) {
    if (n <= 1) return false;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) return false;
    }
    return true;
}

Generator prime_numbers() {
    int num = 2;
    while (true) {
        if (is_prime(num)) {
            co_yield num;
        }
        ++num;
    }
}

int main() {
    auto gen = prime_numbers();

    for (int i = 0; i < 10; ++i) {
        gen.move_next();
        std::cout << gen.current_value() << " ";
    }
    std::cout << std::endl;

    return 0;
}

```
