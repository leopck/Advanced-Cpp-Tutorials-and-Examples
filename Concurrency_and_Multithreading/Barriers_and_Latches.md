# Barriers and Latches in Concurrency

Barriers and latches are synchronization primitives introduced in C++20 that facilitate the coordination of multiple threads. They allow threads to wait for each other to reach a certain point of execution, ensuring that certain operations are performed in a coordinated manner.

## Overview

- **Barriers**: Allow a fixed number of threads to repeatedly synchronize at certain points in the code. Useful for tasks divided into multiple phases.
- **Latches**: A one-time synchronization primitive where threads wait until a predefined number of signals are received before continuing.

## Barriers

### Example: Coordinated Thread Work with `std::barrier`

Here's an example demonstrating how to use `std::barrier` for coordinated thread work:

```cpp
#include <iostream>
#include <barrier>
#include <thread>
#include <vector>

void work(std::barrier<>& barrier, int id) {
    for (int phase = 0; phase < 3; ++phase) {
        std::cout << "Thread " << id << " working on phase " << phase << std::endl;
        barrier.arrive_and_wait();
    }
}

int main() {
    const int num_threads = 4;
    std::barrier barrier(num_threads);

    std::vector<std::thread> threads;
    for (int i = 0; i < num_threads; ++i) {
        threads.emplace_back(work, std::ref(barrier), i);
    }

    for (auto& thread : threads) {
        thread.join();
    }

    return 0;
}
```

### Explanation

1. **Barrier Initialization**: The `std::barrier` object is initialized with the number of participating threads.
2. **Work Function**: Each thread executes the `work` function, which prints a message and then waits at the barrier using `barrier.arrive_and_wait()`. This ensures all threads synchronize at the end of each phase.
3. **Thread Creation**: Multiple threads are created, each executing the `work` function.
4. **Thread Joining**: The main thread waits for all worker threads to complete by calling `join()` on each thread.

## Latches

### Example: One-Time Synchronization with `std::latch`

Here's an example demonstrating how to use `std::latch` for one-time synchronization:

```cpp
#include <iostream>
#include <latch>
#include <thread>
#include <vector>

void worker(std::latch& sync_point, int id) {
    std::cout << "Worker " << id << " is doing initial work\n";
    sync_point.arrive_and_wait();
    std::cout << "Worker " << id << " is doing post-sync work\n";
}

int main() {
    constexpr int num_threads = 4;
    std::latch sync_point(num_threads);

    std::vector<std::thread> threads;
    for (int i = 0; i < num_threads; ++i) {
        threads.emplace_back(worker, std::ref(sync_point), i);
    }

    for (auto& thread : threads) {
        thread.join();
    }

    return 0;
}
```

### Explanation

1. **Latch Initialization**: The `std::latch` object is initialized with the number of participating threads.
2. **Worker Function**: Each thread executes the `worker` function, which prints a message, waits at the latch using `sync_point.arrive_and_wait()`, and then prints another message. This ensures all threads synchronize at the latch before proceeding.
3. **Thread Creation**: Multiple threads are created, each executing the `worker` function.
4. **Thread Joining**: The main thread waits for all worker threads to complete by calling `join()` on each thread.

## Use Cases

- **Barriers**: Ideal for parallel algorithms that can be divided into phases, ensuring that all threads complete one phase before moving to the next.
- **Latches**: Useful for initializing a set of resources or starting a set of threads together, waiting until all threads are ready before proceeding.

## Conclusion

Barriers and latches provide powerful synchronization mechanisms for coordinating threads in concurrent applications. By using `std::barrier` and `std::latch`, developers can ensure that threads work together in a controlled and efficient manner, reducing the complexity of thread synchronization.

Feel free to explore more examples and experiment with these primitives in your projects to understand their capabilities and benefits fully.
```
