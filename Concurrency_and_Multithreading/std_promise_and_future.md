# `std::promise` and `std::future`

## Introduction
`std::promise` and `std::future` provide a mechanism for asynchronous communication between threads. A `std::promise` is used to set a value that a `std::future` will receive. This allows one thread to produce a value at some point in time and another thread to consume that value when it becomes available.

## Code Example
### Basic Example of `std::promise` and `std::future`
```cpp
#include <iostream>
#include <thread>
#include <future>

void async_task(std::promise<int> p) {
    std::this_thread::sleep_for(std::chrono::seconds(2));
    p.set_value(42);
}

int main() {
    std::promise<int> p;
    std::future<int> f = p.get_future();

    std::thread t(async_task, std::move(p));

    std::cout << "Waiting for the result...\n";
    int result = f.get();
    std::cout << "Result: " << result << std::endl;

    t.join();

    return 0;
}
```

### Explanation
1. **Creating a Promise and Future**: 
    - `std::promise<int> p;` creates a promise that will hold an integer.
    - `std::future<int> f = p.get_future();` creates a future associated with the promise. This future will eventually hold the value set by the promise.
2. **Passing the Promise to a Thread**: 
    - `std::thread t(async_task, std::move(p));` creates a thread that runs `async_task` and passes the promise to it.
3. **Setting the Promise Value**: 
    - Inside `async_task`, `p.set_value(42);` sets the value of the promise to 42. This makes the value available to the associated future.
4. **Getting the Future Value**: 
    - `int result = f.get();` waits for the promise to be set and then retrieves the value. If the promise hasn't been set yet, `get()` will block until the value is available.
5. **Thread Synchronization**: 
    - `t.join();` ensures that the main thread waits for `t` to finish before exiting.

### Using `std::promise` and `std::future` for Exception Handling
Promises and futures can also be used to propagate exceptions between threads.

```cpp
#include <iostream>
#include <thread>
#include <future>
#include <stdexcept>

void async_task(std::promise<int> p) {
    try {
        std::this_thread::sleep_for(std::chrono::seconds(2));
        throw std::runtime_error("An error occurred");
        p.set_value(42); // This line won't be reached
    } catch (...) {
        p.set_exception(std::current_exception());
    }
}

int main() {
    std::promise<int> p;
    std::future<int> f = p.get_future();

    std::thread t(async_task, std::move(p));

    try {
        int result = f.get();
        std::cout << "Result: " << result << std::endl;
    } catch (const std::exception& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }

    t.join();

    return 0;
}
```

### Explanation
1. **Exception Handling**:
    - `throw std::runtime_error("An error occurred");` throws an exception inside the asynchronous task.
    - `p.set_exception(std::current_exception());` captures and sets the current exception in the promise.
2. **Retrieving the Exception**:
    - `int result = f.get();` tries to get the value from the future. If an exception was set, `get()` will rethrow the exception, which can then be caught and handled in the main thread.

### Conclusion
Using `std::promise` and `std::future` allows for clean and effective communication between threads. These tools are particularly useful for handling asynchronous computations and propagating exceptions across threads.
```
