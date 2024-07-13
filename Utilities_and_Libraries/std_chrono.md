# Using `std::chrono` for Time Measurement

`std::chrono` provides tools for measuring time intervals, creating timers, and working with different time units.

## Example: Measuring Elapsed Time with `std::chrono`

```cpp
#include <iostream>
#include <chrono>
#include <thread>

void simulate_work() {
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main() {
    auto start = std::chrono::high_resolution_clock::now();

    simulate_work();

    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> duration = end - start;

    std::cout << "Elapsed time: " << duration.count() << " seconds" << std::endl;

    return 0;
}
```
