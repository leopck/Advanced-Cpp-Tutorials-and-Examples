# Disjoint Set (Union-Find)

A Disjoint Set (Union-Find) is a data structure that keeps track of a set of elements partitioned into non-overlapping subsets. It supports two operations: find and union.

## Example: Implementing a Disjoint Set Data Structure

```cpp
#include <iostream>
#include <vector>

class DisjointSet {
public:
    DisjointSet(int size) : parent(size), rank(size, 0) {
        for (int i = 0; i < size; ++i) {
            parent[i] = i;
        }
    }

    int find(int u) {
        if (u != parent[u]) {
            parent[u] = find(parent[u]);
        }
        return parent[u];
    }

    void union_sets(int u, int v) {
        int root_u = find(u);
        int root_v = find(v);
        if (root_u != root_v) {
            if (rank[root_u] > rank[root_v]) {
                parent[root_v] = root_u;
            } else if (rank[root_u] < rank[root_v]) {
                parent[root_u] = root_v;
            } else {
                parent[root_v] = root_u;
                ++rank[root_u];
            }
        }
    }

private:
    std::vector<int> parent;
    std::vector<int> rank;
};

int main() {
    DisjointSet ds(10);
    ds.union_sets(2, 3);
    ds.union_sets(3, 4);

    std::cout << "Find 2: " << ds.find(2) << std::endl;
    std::cout << "Find 3: " << ds.find(3) << std::endl;
    std::cout << "Find 4: " << ds.find(4) << std::endl;

    return 0;
}
```
