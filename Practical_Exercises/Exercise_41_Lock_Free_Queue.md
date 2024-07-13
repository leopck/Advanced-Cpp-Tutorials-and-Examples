# Exercise 41: Implement a Lock-Free Queue Using `std::atomic`

In this exercise, we will implement a lock-free queue using `std::atomic` to allow concurrent access without locks. Lock-free data structures are essential for high-performance and scalable systems.

## Goal

- Implement a lock-free queue that supports concurrent enqueues and dequeues.

## Step-by-Step Solution

### Step 1: Define the Node Structure

Define a node structure for the queue, where each node contains a value and a pointer to the next node.

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
```

### Step 2: Implement Producer and Consumer Functions

Implement producer and consumer functions to test the lock-free queue.

```cpp
void producer(LockFreeQueue<int>& queue) {
    for (int i = 0; i < 1000; ++i) {
        queue.enqueue(i);
    }
}

void consumer(LockFreeQueue<int>& queue) {
    int value;
    for (int i = 0; i < 1000; ++i) {
        while (!queue.dequeue(value)) {
            // Busy-wait
        }
        std::cout << "Dequeued: " << value << std::endl;
    }
}
```

### Step 3: Create and Run Producer and Consumer Threads

Create and run producer and consumer threads to test the lock-free queue.

```cpp
int main() {
    LockFreeQueue<int> queue;

    std::thread t1(producer, std::ref(queue));
    std::thread t2(consumer, std::ref(queue));

    t1.join();
    t2.join();

    return 0;
}
```

### Full Example Code

Here's the full example code:

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

void producer(LockFreeQueue<int>& queue) {
    for (int i = 0; i < 1000; ++i) {
        queue.enqueue(i);
    }
}

void consumer(LockFreeQueue<int>& queue) {
    int value;
    for (int i = 0; i < 1000; ++i) {
        while (!queue.dequeue(value)) {
            // Busy-wait
        }
        std::cout << "Dequeued: " << value << std::endl;
    }
}

int main() {
    LockFreeQueue<int> queue;

    std::thread t1(producer, std::ref(queue));
    std::thread t2(consumer, std::ref(queue));

    t1.join();
    t2.join();

    return 0;
}
```

## Conclusion

Implementing a lock-free queue using `std::atomic` allows for concurrent access without the need for locks, which can improve performance in multi-threaded environments. By understanding and applying these techniques, you can create efficient and scalable data structures for your applications.
```

