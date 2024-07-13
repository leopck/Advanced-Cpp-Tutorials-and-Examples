Create a custom std::error_code for a custom library to provide detailed error information.

```cpp
#include <iostream>
#include <system_error>

enum class LibraryError {
    None,
    FileNotFound,
    InvalidFormat
};

class LibraryErrorCategory : public std::error_category {
public:
    const char* name() const noexcept override {
        return "LibraryError";
    }

    std::string message(int ev) const override {
        switch (static_cast<LibraryError>(ev)) {
            case LibraryError::None:
                return "No error";
            case LibraryError::FileNotFound:
                return "File not found";
            case LibraryError::InvalidFormat:
                return "Invalid format";
            default:
                return "Unknown error";
        }
    }
};

const LibraryErrorCategory library_error_category{};

std::error_code make_error_code(LibraryError e) {
    return {static_cast<int>(e), library_error_category};
}

namespace std {
    template<>
    struct is_error_code_enum<LibraryError> : true_type {};
}

void function_that_fails() {
    throw std::system_error(make_error_code(LibraryError::FileNotFound));
}

int main() {
    try {
        function_that_fails();
    } catch (const std::system_error& e) {
        std::cerr << "Caught system_error: " << e.what() << " (" << e.code() << ")\n";
    }

    return 0;
}

```
