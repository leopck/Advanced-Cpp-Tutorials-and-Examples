# Factory Pattern

The Factory Pattern is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

## Example: Implementing a Factory Pattern

```cpp
#include <iostream>
#include <memory>

class Product {
public:
    virtual void use() const = 0;
};

class ConcreteProductA : public Product {
public:
    void use() const override {
        std::cout << "Using ConcreteProductA" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void use() const override {
        std::cout << "Using ConcreteProductB" << std::endl;
    }
};

class Factory {
public:
    enum class ProductType { A, B };

    std::unique_ptr<Product> createProduct(ProductType type) const {
        switch (type) {
            case ProductType::A:
                return std::make_unique<ConcreteProductA>();
            case ProductType::B:
                return std::make_unique<ConcreteProductB>();
            default:
                throw std::invalid_argument("Unknown product type");
        }
    }
};

int main() {
    Factory factory;
    auto productA = factory.createProduct(Factory::ProductType::A);
    auto productB = factory.createProduct(Factory::ProductType::B);

    productA->use();
    productB->use();

    return 0;
}
```
