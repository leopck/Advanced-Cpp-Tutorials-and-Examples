# Visitor Pattern

The Visitor pattern is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

## Example: Visitor Pattern

```cpp
#include <iostream>
#include <vector>
#include <memory>

// Forward declaration
class ConcreteElementA;
class ConcreteElementB;

// Visitor interface
class Visitor {
public:
    virtual void visit(ConcreteElementA& element) = 0;
    virtual void visit(ConcreteElementB& element) = 0;
};

// Element interface
class Element {
public:
    virtual void accept(Visitor& visitor) = 0;
};

// Concrete element A
class ConcreteElementA : public Element {
public:
    void accept(Visitor& visitor) override {
        visitor.visit(*this);
    }

    void operationA() const {
        std::cout << "Operation A\n";
    }
};

// Concrete element B
class ConcreteElementB : public Element {
public:
    void accept(Visitor& visitor) override {
        visitor.visit(*this);
    }

    void operationB() const {
        std::cout << "Operation B\n";
    }
};

// Concrete visitor
class ConcreteVisitor : public Visitor {
public:
    void visit(ConcreteElementA& element) override {
        element.operationA();
    }

    void visit(ConcreteElementB& element) override {
        element.operationB();
    }
};

int main() {
    std::vector<std::unique_ptr<Element>> elements;
    elements.push_back(std::make_unique<ConcreteElementA>());
    elements.push_back(std::make_unique<ConcreteElementB>());

    ConcreteVisitor visitor;
    for (auto& element : elements) {
        element->accept(visitor);
    }

    return 0;
}
```
