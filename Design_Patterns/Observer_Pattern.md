# Observer Pattern

The Observer Pattern is a behavioral design pattern where an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes.

## Example: Implementing the Observer Pattern

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <algorithm>

class Observer {
public:
    virtual void update() = 0;
};

class Subject {
public:
    void attach(std::shared_ptr<Observer> observer) {
        observers.push_back(observer);
    }

    void detach(std::shared_ptr<Observer> observer) {
        observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
    }

    void notify() {
        for (auto& observer : observers) {
            observer->update();
        }
    }

private:
    std::vector<std::shared_ptr<Observer>> observers;
};

class ConcreteObserver : public Observer {
public:
    ConcreteObserver(const std::string& name) : name(name) {}

    void update() override {
        std::cout << "Observer " << name << " notified" << std::endl;
    }

private:
    std::string name;
};

int main() {
    auto subject = std::make_shared<Subject>();
    auto observer1 = std::make_shared<ConcreteObserver>("Observer1");
    auto observer2 = std::make_shared<ConcreteObserver>("Observer2");

    subject->attach(observer1);
    subject->attach(observer2);

    subject->notify();

    subject->detach(observer1);

    subject->notify();

    return 0;
}
```
