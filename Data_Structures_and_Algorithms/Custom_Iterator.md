# Custom Iterator

## Introduction
Custom iterators can be implemented to provide iteration capabilities for custom data structures.

## Code Example
```cpp
#include <iostream>

class Range {
public:
    Range(int start, int end) : start(start), end(end) {}

    class Iterator {
    public:
        Iterator(int current) : current(current) {}

        int operator*() const { return current; }

        Iterator& operator++() {
            ++current;
            return *this;
        }

        bool operator!=(const Iterator& other) const {
            return current != other.current;
        }

    private:
        int current;
    };

    Iterator begin() const { return Iterator(start); }
    Iterator end() const { return Iterator(end); }

private:
    int start, end;
};

int main() {
    Range range(1, 5);
    for (int value : range) {
        std::cout << value << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
