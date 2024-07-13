# Serialization Libraries

Serialization libraries allow objects to be converted to a format that can be easily stored and retrieved. Boost.Serialization and Cereal are two popular libraries for serialization in C++.

## Example: Using Boost.Serialization for Serialization

```cpp
#include <iostream>
#include <sstream>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>

class MyClass {
public:
    MyClass() = default;
    MyClass(int a, double b) : a(a), b(b) {}

    template<class Archive>
    void serialize(Archive& ar, const unsigned int version) {
        ar & a;
        ar & b;
    }

    void print() const {
        std::cout << "a: " << a << ", b: " << b << std::endl;
    }

private:
    int a;
    double b;
};

int main() {
    MyClass original(1, 2.5);

    std::ostringstream oss;
    {
        boost::archive::text_oarchive oa(oss);
        oa << original;
    }

    MyClass loaded;
    std::istringstream iss(oss.str());
    {
        boost::archive::text_iarchive ia(iss);
        ia >> loaded;
    }

    loaded.print();
    return 0;
}
```
