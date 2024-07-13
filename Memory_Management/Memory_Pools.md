# Memory Pools

A memory pool is a collection of preallocated memory blocks that can be reused to satisfy memory allocation requests, reducing fragmentation and allocation overhead.

## Example: Implementing a Memory Pool

```cpp
#include <iostream>
#include <vector>

class MemoryPool {
public:
    MemoryPool(size_t blockSize, size_t blockCount)
        : blockSize(blockSize), blockCount(blockCount) {
        pool.resize(blockSize * blockCount);
        for (size_t i = 0; i < blockCount; ++i) {
            freeBlocks.push_back(&pool[i * blockSize]);
        }
    }

    void* allocate() {
        if (freeBlocks.empty()) {
            throw std::bad_alloc();
        }
        void* block = freeBlocks.back();
        freeBlocks.pop_back();
        return block;
    }

    void deallocate(void* block) {
        freeBlocks.push_back(block);
    }

private:
    size_t blockSize;
    size_t blockCount;
    std::vector<char> pool;
    std::vector<void*> freeBlocks;
};

template<typename T>
class PoolAllocator {
public:
    using value_type = T;

    PoolAllocator(MemoryPool& pool) : pool(pool) {}

    T* allocate(std::size_t n) {
        if (n != 1) throw std::bad_alloc();
        return static_cast<T*>(pool.allocate());
    }

    void deallocate(T* p, std::size_t) {
        pool.deallocate(p);
    }

private:
    MemoryPool& pool;
};

int main() {
    MemoryPool pool(sizeof(int), 10);
    std::vector<int, PoolAllocator<int>> vec(PoolAllocator<int>(pool));

    for (int i = 0; i < 10; ++i) {
        vec.push_back(i);
    }

    for (const auto& v : vec) {
        std::cout << v << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
