# Lock-Free Data Structures

Lock-free data structures provide high performance by avoiding locks and enabling concurrent access by multiple threads.

## Example: Lock-Free Stack

```cpp
#include <iostream>
#include <atomic>
#include <thread>
#include <vector>

template<typename T>
class LockFreeStack {
public:
    void push(T value) {
        Node* new_node = new Node(value);
        new_node->next = head.load();
        while (!head.compare_exchange_weak(new_node->next, new_node)) {
            // Busy-wait
        }
    }

    bool pop(T& result) {
        Node* old_head = head.load();
        while (old_head && !head.compare_exchange_weak(old_head, old_head->next)) {
            // Busy-wait
        }
        if (old_head) {
            result = old_head->value;
            delete old_head;
            return true;
        }
        return false;
    }

private:
    struct Node {
        T value;
        Node* next;
        Node(T val) : value(val), next(nullptr) {}
    };
    std::atomic<Node*> head = nullptr;
};

int main() {
    LockFreeStack<int> stack;

    auto pusher = [&stack]() {
        for (int i = 0; i < 1000; ++i) {
            stack.push(i);
        }
    };

    auto popper = [&stack]() {
        int value;
        for (int i = 0; i < 1000; ++i) {
            if (stack.pop(value)) {
                std::cout << "Popped: " << value << std::endl;
            }
        }
    };

    std::thread t1(pusher);
    std::thread t2(popper);

    t1.join();
    t2.join();

    return 0;
}
```
