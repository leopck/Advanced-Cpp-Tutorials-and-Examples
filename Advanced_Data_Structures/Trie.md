# Trie

A Trie (pronounced as "try") is a tree-like data structure used to store a dynamic set of strings, where keys are usually strings. It is used for efficient retrieval of a key in a large dataset of strings.

## Example: Implementing a Trie

```cpp
#include <iostream>
#include <unordered_map>
#include <memory>

class Trie {
public:
    Trie() : is_end_of_word(false) {}

    void insert(const std::string& word) {
        Trie* node = this;
        for (char ch : word) {
            if (!node->children.count(ch)) {
                node->children[ch] = std::make_unique<Trie>();
            }
            node = node->children[ch].get();
        }
        node->is_end_of_word = true;
    }

    bool search(const std::string& word) const {
        const Trie* node = this;
        for (char ch : word) {
            if (!node->children.count(ch)) {
                return false;
            }
            node = node->children.at(ch).get();
        }
        return node->is_end_of_word;
    }

    bool starts_with(const std::string& prefix) const {
        const Trie* node = this;
        for (char ch : prefix) {
            if (!node->children.count(ch)) {
                return false;
            }
            node = node->children.at(ch).get();
        }
        return true;
    }

private:
    std::unordered_map<char, std::unique_ptr<Trie>> children;
    bool is_end_of_word;
};

int main() {
    Trie trie;
    trie.insert("hello");
    trie.insert("world");

    std::cout << std::boolalpha;
    std::cout << "Search 'hello': " << trie.search("hello") << std::endl;
    std::cout << "Search 'hell': " << trie.search("hell") << std::endl;
    std::cout << "Starts with 'wor': " << trie.starts_with("wor") << std::endl;
    std::cout << "Starts with 'he': " << trie.starts_with("he") << std::endl;

    return 0;
}
```
