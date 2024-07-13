# Using `std::error_code` for Detailed Error Handling

Custom error codes can be implemented to provide more detailed error information in your programs.

## Example: Custom `std::error_code`

```cpp
#include <iostream>
#include <system_error>

enum class MyError {
    None,
    Error1,
    Error2
};

class MyErrorCategory : public std::error_category {
public:
    const char* name() const noexcept override {
        return "MyError";
    }

    std::string message(int ev) const override {
        switch (static_cast<MyError>(ev)) {
            case MyError::None:
                return "No error";
            case MyError::Error1:
                return "Error 1 occurred";
            case MyError::Error2:
                return "Error 2 occurred";
            default:
                return "Unknown error";
        }
    }
};

const MyErrorCategory my_error_category{};

std::error_code make_error_code(MyError e) {
    return {static_cast<int>(e), my_error_category};
}

namespace std {
    template<>
    struct is_error_code_enum<MyError> : true_type {};
}

void function_that_fails() {
    throw std::system_error(make_error_code(MyError::Error1));
}

int main() {
    try {
        function_that_fails();
    } catch (const std::system_error& e) {
        std::cerr << "Caught system_error: " << e.what() << " (" << e.code() << ")\n";
    }

    return 0;
}
