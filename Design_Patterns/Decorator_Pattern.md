# Decorator Pattern

The Decorator Pattern is a structural design pattern that allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class.

## Example: Implementing the Decorator Pattern

```cpp
#include <iostream>
#include <memory>

class Component {
public:
    virtual void operation() const = 0;
};

class ConcreteComponent : public Component {
public:
    void operation() const override {
        std::cout << "ConcreteComponent operation" << std::endl;
    }
};

class Decorator : public Component {
protected:
    std::shared_ptr<Component> component;

public:
    Decorator(std::shared_ptr<Component> comp) : component(comp) {}

    void operation() const override {
        if (component) {
            component->operation();
        }
    }
};

class ConcreteDecoratorA : public Decorator {
public:
    ConcreteDecoratorA(std::shared_ptr<Component> comp) : Decorator(comp) {}

    void operation() const override {
        Decorator::operation();
        std::cout << "ConcreteDecoratorA operation" << std::endl;
    }
};

class ConcreteDecoratorB : public Decorator {
public:
    ConcreteDecoratorB(std::shared_ptr<Component> comp) : Decorator(comp) {}

    void operation() const override {
        Decorator::operation();
        std::cout << "ConcreteDecoratorB operation" << std::endl;
    }
};

int main() {
    auto component = std::make_shared<ConcreteComponent>();
    auto decoratorA = std::make_shared<ConcreteDecoratorA>(component);
    auto decoratorB = std::make_shared<ConcreteDecoratorB>(decoratorA);

    decoratorB->operation();

    return 0;
}
```
