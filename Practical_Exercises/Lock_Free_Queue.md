Create a lock-free queue using std::atomic to allow concurrent access without locks.

```cpp
#include <iostream>
#include <atomic>
#include <thread>
#include <vector>

template<typename T>
class LockFreeQueue {
public:
    LockFreeQueue() : head(new Node), tail(head.load()) {}

    ~LockFreeQueue() {
        while (Node* const old_head = head.load()) {
            head.store(old_head->next);
            delete old_head;
        }
    }

    void enqueue(T value) {
        Node* new_node = new Node(value);
        Node* old_tail = tail.exchange(new_node);
        old_tail->next = new_node;
    }

    bool dequeue(T& result) {
        Node* old_head = head.load();
        Node* next = old_head->next;
        if (next == nullptr) return false;
        result = next->value;
        head.store(next);
        delete old_head;
        return true;
    }

private:
    struct Node {
        T value;
        Node* next = nullptr;
        Node() = default;
        Node(T val) : value(val) {}
    };
    std::atomic<Node*> head;
    std::atomic<Node*> tail;
};

int main() {
    LockFreeQueue<int> queue;

    auto producer = [&queue]() {
        for (int i = 0; i < 1000; ++i) {
            queue.enqueue(i);
        }
    };

    auto consumer = [&queue]() {
        int value;
        for (int i = 0; i < 1000; ++i) {
            while (!queue.dequeue(value)) {
                // Busy-wait
            }
            std::cout << "Dequeued: " << value << std::endl;
        }
    };

    std::thread t1(producer);
    std::thread t2(consumer);

    t1.join();
    t2.join();

    return 0;
}

```

