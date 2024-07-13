# Smart Pointers

Smart pointers are used to manage the lifetime of dynamically allocated objects and ensure proper memory deallocation.

## Example: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`

### Using `std::unique_ptr`

```cpp
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> ptr = std::make_unique<int>(42);
    std::cout << "Unique pointer value: " << *ptr << std::endl;

    // Transfer ownership
    std::unique_ptr<int> ptr2 = std::move(ptr);
    if (ptr == nullptr) {
        std::cout << "ptr is null after move" << std::endl;
    }
    std::cout << "Transferred unique pointer value: " << *ptr2 << std::endl;

    return 0;
}
```

Using `std::shared_ptr`

```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> ptr = std::make_shared<int>(42);
    std::cout << "Shared pointer value: " << *ptr << std::endl;
    std::cout << "Shared pointer use count: " << ptr.use_count() << std::endl;

    {
        std::shared_ptr<int> ptr2 = ptr;
        std::cout << "Shared pointer use count after copy: " << ptr.use_count() << std::endl;
    }

    std::cout << "Shared pointer use count after ptr2 goes out of scope: " << ptr.use_count() << std::endl;

    return 0;
}

```

Using `std::weak_ptr`

```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> sharedPtr = std::make_shared<int>(42);
    std::weak_ptr<int> weakPtr = sharedPtr;

    std::cout << "Shared pointer value: " << *sharedPtr << std::endl;
    std::cout << "Shared pointer use count: " << sharedPtr.use_count() << std::endl;

    if (auto ptr = weakPtr.lock()) {
        std::cout << "Weak pointer value: " << *ptr << std::endl;
        std::cout << "Shared pointer use count after lock: " << sharedPtr.use_count() << std::endl;
    }

    return 0;
}

```


