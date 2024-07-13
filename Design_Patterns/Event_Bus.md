# Event Bus

## Introduction
An event bus system allows different parts of a program to communicate through events.

## Code Example
```cpp
#include <iostream>
#include <functional>
#include <unordered_map>
#include <vector>

class EventBus {
public:
    using EventHandler = std::function<void()>;

    void subscribe(const std::string& event_name, EventHandler handler) {
        handlers[event_name].push_back(handler);
    }

    void publish(const std::string& event_name) {
        if (handlers.find(event_name) != handlers.end()) {
            for (const auto& handler : handlers[event_name]) {
                handler();
            }
        }
    }

private:
    std::unordered_map<std::string, std::vector<EventHandler>> handlers;
};

int main() {
    EventBus bus;

    bus.subscribe("event1", []() { std::cout << "Handler 1 for event1\n"; });
    bus.subscribe("event1", []() { std::cout << "Handler 2 for event1\n"; });
    bus.subscribe("event2", []() { std::cout << "Handler for event2\n"; });

    bus.publish("event1");
    bus.publish("event2");

    return 0;
}
```
