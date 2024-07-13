# Policy-Based Design

Policy-based design is a technique that allows the behavior of a class to be customized by passing different policies as template parameters.

## Example: Policy-Based Design

```cpp
#include <iostream>

// Policy classes
struct LoggingPolicy {
    void log(const std::string& message) {
        std::cout << "Log: " << message << std::endl;
    }
};

struct NoLoggingPolicy {
    void log(const std::string&) {
        // Do nothing
    }
};

// Template class with policies
template<typename LoggingPolicy>
class MyClass : private LoggingPolicy {
public:
    void do_something() {
        this->log("Doing something");
    }
};

int main() {
    MyClass<LoggingPolicy> obj1;
    obj1.do_something(); // Outputs log message

    MyClass<NoLoggingPolicy> obj2;
    obj2.do_something(); // No output

    return 0;
}

