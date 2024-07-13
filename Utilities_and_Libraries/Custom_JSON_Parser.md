# Implementing a Basic JSON Parser

Implement a simple JSON parser to handle basic JSON structures.

## Example: Basic JSON Parser

```cpp
#include <iostream>
#include <string>
#include <variant>
#include <vector>
#include <unordered_map>
#include <sstream>

using JsonValue = std::variant<std::nullptr_t, bool, int, double, std::string,
                               std::vector<JsonValue>, std::unordered_map<std::string, JsonValue>>;

class JsonParser {
public:
    JsonValue parse(const std::string& json) {
        std::istringstream stream(json);
        return parse_value(stream);
    }

private:
    JsonValue parse_value(std::istringstream& stream) {
        char ch;
        stream >> ch;

        if (ch == 'n') {
            stream.ignore(3); // null
            return nullptr;
        } else if (ch == 't') {
            stream.ignore(3); // true
            return true;
        } else if (ch == 'f') {
            stream.ignore(4); // false
            return false;
        } else if (ch == '\"') {
            return parse_string(stream);
        } else if (ch == '[') {
            return parse_array(stream);
        } else if (ch == '{') {
            return parse_object(stream);
        } else if (std::isdigit(ch) || ch == '-') {
            stream.putback(ch);
            return parse_number(stream);
        } else {
            throw std::runtime_error("Invalid JSON");
        }
    }

    std::string parse_string(std::istringstream& stream) {
        std::string result;
        char ch;
        while (stream >> ch) {
            if (ch == '\"') break;
            result += ch;
        }
        return result;
    }

    JsonValue parse_number(std::istringstream& stream) {
        double value;
        stream >> value;
        return value;
    }

    std::vector<JsonValue> parse_array(std::istringstream& stream) {
        std::vector<JsonValue> result;
        char ch;
        while (stream >> ch) {
            if (ch == ']') break;
            stream.putback(ch);
            result.push_back(parse_value(stream));
            stream >> ch; // , or ]
            if (ch == ']') break;
        }
        return result;
    }

    std::unordered_map<std::string, JsonValue> parse_object(std::istringstream& stream) {
        std::unordered_map<std::string, JsonValue> result;
        char ch;
        while (stream >> ch) {
            if (ch == '}') break;
            stream.putback(ch);
            auto key = std::get<std::string>(parse_value(stream));
            stream >> ch; // :
            result[key] = parse_value(stream);
            stream >> ch; // , or }
            if (ch == '}') break;
        }
        return result;
    }
};

void print_json(const JsonValue& value, int indent = 0) {
    std::visit([indent](auto&& arg) {
        using T = std::decay_t<decltype(arg)>;
        if constexpr (std::is_same_v<T, std::nullptr_t>) {
            std::cout << "null";
        } else if constexpr (std::is_same_v<T, bool>) {
            std::cout << (arg ? "true" : "false");
        } else if constexpr (std::is_same_v<T, int> || std::is_same_v<T, double>) {
            std::cout << arg;
        } else if constexpr (std::is_same_v<T, std::string>) {
            std::cout << "\"" << arg << "\"";
        } else if constexpr (std::is_same_v<T, std::vector<JsonValue>>) {
            std::cout << "[\n";
            for (const auto& elem : arg) {
                std::cout << std::string(indent + 2, ' ');
                print_json(elem, indent + 2);
                std::cout << ",\n";
            }
            std::cout << std::string(indent, ' ') << "]";
        } else if constexpr (std::is_same_v<T, std::unordered_map<std::string, JsonValue>>) {
            std::cout << "{\n";
            for (const auto& [key, val] : arg) {
                std::cout << std::string(indent + 2, ' ') << "\"" << key << "\": ";
                print_json(val, indent + 2);
                std::cout << ",\n";
            }
            std::cout << std::string(indent, ' ') << "}";
        }
    }, value);
}

int main() {
    std::string json = R"({"name": "John", "age": 30, "is_student": false, "scores": [85, 90, 92], "address": {"city": "New York", "zip": "10001"}})";
    JsonParser parser;
    JsonValue value = parser.parse(json);
    print_json(value);
    std::cout << std::endl;

    return 0;
}
```
