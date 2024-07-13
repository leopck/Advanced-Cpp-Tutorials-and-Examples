# Cooperative Cancellation with `std::stop_token`

`std::stop_token` is a feature introduced in C++20 that provides a mechanism for cooperative thread cancellation. It allows threads to request a stop and for other threads to check and respond to this stop request.

## Overview

The `std::stop_token` works in conjunction with `std::jthread` to provide a convenient way to manage thread cancellation. When a `std::jthread` is destroyed, it automatically requests a stop for the thread it manages. The thread can periodically check the stop token to see if a stop has been requested and respond appropriately.

## Example: Cooperative Cancellation with `std::stop_token`

Here's an example demonstrating how to use `std::stop_token` for cooperative thread cancellation:

```cpp
#include <iostream>
#include <thread>
#include <chrono>
#include <stop_token>

void worker(std::stop_token stoken) {
    while (!stoken.stop_requested()) {
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
        std::cout << "Working..." << std::endl;
    }
    std::cout << "Worker stopped" << std::endl;
}

int main() {
    std::jthread t(worker);
    std::this_thread::sleep_for(std::chrono::seconds(1));
    t.request_stop();

    return 0;
}
```

### Explanation

1. **Worker Function**: The `worker` function takes a `std::stop_token` as an argument. Inside the loop, it periodically checks if a stop has been requested using `stoken.stop_requested()`. If a stop is requested, the loop exits and the function ends.

2. **Main Function**: A `std::jthread` is created with the `worker` function. The main thread sleeps for 1 second and then calls `t.request_stop()` to request a stop for the worker thread.

3. **Automatic Stop Request**: When the `std::jthread` object `t` goes out of scope, it automatically requests a stop for the thread it manages. This ensures that the thread is properly cleaned up.

## Advantages

- **Cooperative Cancellation**: Threads can be notified of a stop request and handle cleanup or termination gracefully.
- **Automatic Cleanup**: `std::jthread` ensures that threads are joined and cleaned up automatically, reducing the risk of resource leaks.

## Use Cases

- **Long-Running Tasks**: Use `std::stop_token` for tasks that may need to be stopped or interrupted, such as background processing or monitoring tasks.
- **Responsive Applications**: Improve the responsiveness of applications by allowing threads to be stopped in a controlled manner.

## Conclusion

`std::stop_token` provides a powerful and flexible way to manage thread cancellation in modern C++ applications. By using `std::jthread` and `std::stop_token`, you can implement cooperative cancellation and ensure that threads are managed and cleaned up efficiently.

```

