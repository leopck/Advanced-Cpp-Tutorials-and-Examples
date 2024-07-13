# Mutex and Locking Mechanisms

Mutexes and locks are used to synchronize access to shared resources in multithreaded applications.

## Example: `std::timed_mutex` and `std::shared_mutex`

### Using `std::timed_mutex`

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>

std::timed_mutex mtx;

void try_lock_for() {
    if (mtx.try_lock_for(std::chrono::seconds(1))) {
        std::cout << "Lock acquired by thread" << std::this_thread::get_id() << std::endl;
        std::this_thread::sleep_for(std::chrono::seconds(2));
        mtx.unlock();
    } else {
        std::cout << "Lock not acquired by thread" << std::this_thread::get_id() << std::endl;
    }
}

int main() {
    std::thread t1(try_lock_for);
    std::thread t2(try_lock_for);

    t1.join();
    t2.join();

    return 0;
}
```

Using `std::shared_mutex`

```cpp
#include <iostream>
#include <shared_mutex>
#include <thread>
#include <vector>

std::shared_mutex shared_mtx;
int shared_data = 0;

void reader() {
    std::shared_lock lock(shared_mtx);
    std::cout << "Reader: " << shared_data << std::endl;
}

void writer(int value) {
    std::unique_lock lock(shared_mtx);
    shared_data = value;
    std::cout << "Writer: " << shared_data << std::endl;
}

int main() {
    std::vector<std::thread> threads;
    for (int i = 0; i < 5; ++i) {
        threads.emplace_back(reader);
    }
    threads.emplace_back(writer, 10);
    for (auto& thread : threads) {
        thread.join();
    }
    return 0;
}

```
