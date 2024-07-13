# Using `std::jthread` for Scoped Threads

C++20 introduced `std::jthread`, which automatically joins the thread when it goes out of scope, simplifying thread management.

## Example: Scoped Threads with `std::jthread`

```cpp
#include <iostream>
#include <thread>
#include <chrono>

void work(int id) {
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Worker " << id << " done\n";
}

int main() {
    std::jthread t1(work, 1);
    std::jthread t2(work, 2);

    // No need to manually join threads
    return 0;
}
```
