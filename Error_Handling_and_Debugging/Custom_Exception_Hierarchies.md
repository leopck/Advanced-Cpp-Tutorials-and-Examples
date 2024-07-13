# Custom Exception Hierarchies

Custom exception hierarchies allow you to create specific exception types that provide more detailed and organized error handling.

## Example: Creating Custom Exceptions

```cpp
#include <iostream>
#include <exception>
#include <string>

class BaseException : public std::exception {
public:
    explicit BaseException(const std::string& message) : msg(message) {}

    const char* what() const noexcept override {
        return msg.c_str();
    }

private:
    std::string msg;
};

class DerivedException : public BaseException {
public:
    explicit DerivedException(const std::string& message) : BaseException(message) {}
};

int main() {
    try {
        throw DerivedException("A derived exception occurred!");
    } catch (const BaseException& e) {
        std::cerr << "Caught BaseException: " << e.what() << std::endl;
    }

    return 0;
}
```
