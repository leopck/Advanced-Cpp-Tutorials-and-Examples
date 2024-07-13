# Exercise 42: Implement a Custom `std::error_code` for a Custom Library

In this exercise, we will create a custom `std::error_code` for a custom library to provide detailed error information. This allows for more descriptive and standardized error handling in your applications.

## Goal

- Define custom error codes and categories.
- Implement functions that use these custom error codes.
- Handle errors using `std::system_error`.

## Step-by-Step Solution

### Step 1: Define the Custom Error Codes

Define an enumeration for your custom error codes.

```cpp
#include <iostream>
#include <system_error>

enum class LibraryError {
    None,
    FileNotFound,
    InvalidFormat
};
```

### Step 2: Create a Custom Error Category

Create a custom error category by inheriting from `std::error_category`.

```cpp
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
```

### Step 3: Define a Global Instance of the Error Category

Define a global instance of your custom error category.

```cpp
const LibraryErrorCategory library_error_category{};
```

### Step 4: Create `std::error_code` Instances

Create `std::error_code` instances for your custom error codes.

```cpp
std::error_code make_error_code(LibraryError e) {
    return {static_cast<int>(e), library_error

_category};
}

namespace std {
    template <>
    struct is_error_code_enum<LibraryError> : true_type {};
}
```

### Step 5: Use the Custom Error Codes

Use the custom error codes in your functions to signal errors.

```cpp
void function_that_fails() {
    throw std::system_error(make_error_code(LibraryError::FileNotFound));
}
```

### Step 6: Handle Errors Using `std::system_error`

Handle errors using `std::system_error` in your application.

```cpp
int main() {
    try {
        function_that_fails();
    } catch (const std::system_error& e) {
        std::cerr << "Caught system_error: " << e.what() << " (" << e.code() << ")" << std::endl;
    }

    return 0;
}
```

### Full Example Code

Here's the full example code:

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
    template <>
    struct is_error_code_enum<LibraryError> : true_type {};
}

void function_that_fails() {
    throw std::system_error(make_error_code(LibraryError::FileNotFound));
}

int main() {
    try {
        function_that_fails();
    } catch (const std::system_error& e) {
        std::cerr << "Caught system_error: " << e.what() << " (" << e.code() << ")" << std::endl;
    }

    return 0;
}
```

## Conclusion

By creating custom `std::error_code` instances and categories, you can provide more detailed and standardized error handling in your applications. This exercise demonstrates how to define custom error codes, use them in functions, and handle them using `std::system_error`.

Feel free to experiment with this example and integrate custom error codes into your own projects for more robust error handling.
```
