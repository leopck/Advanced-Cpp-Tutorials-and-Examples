# Thread Local Storage

Thread Local Storage allows variables to be unique to each thread. The `thread_local` keyword is used to declare thread-local variables.

## Example: Using `thread_local` Storage

```cpp
#include <iostream>
#include <thread>

thread_local int local_var = 0;

void increment_and_print(const std::string& thread_name) {
    local_var++;
    std::cout << "Thread " << thread_name << ": " << local_var << std::endl;
}

int main() {
    std::thread t1(increment_and_print, "A");
    std::thread t2(increment_and_print, "B");

    t1.join();
    t2.join();

    return 0;
}
```


