# Segment Tree

A Segment Tree is a data structure that allows efficient querying and updating of ranges of elements in an array.

## Example: Implementing a Segment Tree

```cpp
#include <iostream>
#include <vector>

class SegmentTree {
public:
    SegmentTree(const std::vector<int>& data) {
        n = data.size();
        tree.resize(2 * n);
        build(data);
    }

    void update(int pos, int value) {
        pos += n;
        tree[pos] = value;
        while (pos > 1) {
            pos /= 2;
            tree[pos] = tree[2 * pos] + tree[2 * pos + 1];
        }
    }

    int query(int left, int right) {
        left += n;
        right += n;
        int sum = 0;
        while (left < right) {
            if (left % 2 == 1) {
                sum += tree[left++];
            }
            if (right % 2 == 1) {
                sum += tree[--right];
            }
            left /= 2;
            right /= 2;
        }
        return sum;
    }

private:
    int n;
    std::vector<int> tree;

    void build(const std::vector<int>& data) {
        for (int i = 0; i < n; ++i) {
            tree[n + i] = data[i];
        }
        for (int i = n - 1; i > 0; --i) {
            tree[i] = tree[2 * i] + tree[2 * i + 1];
        }
    }
};

int main() {
    std::vector<int> data = {1, 2, 3, 4, 5, 6, 7, 8};
    SegmentTree segTree(data);

    std::cout << "Sum of range [1, 5): " << segTree.query(1, 5) << std::endl;

    segTree.update(2, 10);
    std::cout << "Sum of range [1, 5) after update: " << segTree.query(1, 5) << std::endl;

    return 0;
}
```
