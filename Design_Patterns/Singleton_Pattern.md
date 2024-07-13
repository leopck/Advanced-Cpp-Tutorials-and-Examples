# Singleton Pattern

## Introduction
The singleton pattern ensures that a class has only one instance and provides a global point of access to it.

## Code Example
```cpp
#include <iostream>

class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    void showMessage() {
        std::cout << "Singleton instance\n";
    }

private:
    Singleton() {}
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

int main() {
    Singleton& instance = Singleton::getInstance();
    instance.showMessage();

    return 0;
}
```
