# A* Algorithm

A* (A Star) is a pathfinding and graph traversal algorithm, which is often used in many fields of computer science due to its completeness, optimality, and optimal efficiency.

## Example: Implementing A* Search Algorithm

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>
#include <limits>

using namespace std;

struct Node {
    int x, y;
    double g, h;
    Node* parent;
    Node(int x, int y, double g = 0, double h = 0, Node* parent = nullptr)
        : x(x), y(y), g(g), h(h), parent(parent) {}

    double f() const { return g + h; }

    bool operator>(const Node& other) const { return f() > other.f(); }
};

using Grid = vector<vector<int>>;

double heuristic(int x1, int y1, int x2, int y2) {
    return sqrt(pow(x1 - x2, 2) + pow(y1 - y2, 2));
}

vector<Node> get_neighbors(const Grid& grid, Node* node) {
    vector<Node> neighbors;
    int directions[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    for (auto& dir : directions) {
        int nx = node->x + dir[0];
        int ny = node->y + dir[1];
        if (nx >= 0 && ny >= 0 && nx < grid.size() && ny < grid[0].size() && grid[nx][ny] == 0) {
            neighbors.emplace_back(nx, ny);
        }
    }
    return neighbors;
}

vector<Node> reconstruct_path(Node* node) {
    vector<Node> path;
    while (node) {
        path.push_back(*node);
        node = node->parent;
    }
    reverse(path.begin(), path.end());
    return path;
}

vector<Node> a_star(const Grid& grid, Node start, Node goal) {
    priority_queue<Node, vector<Node>, greater<Node>> open_set;
    start.h = heuristic(start.x, start.y, goal.x, goal.y);
    open_set.push(start);

    vector<vector<double>> g_score(grid.size(), vector<double>(grid[0].size(), numeric_limits<double>::infinity()));
    g_score[start.x][start.y] = 0;

    while (!open_set.empty()) {
        Node current = open_set.top();
        open_set.pop();

        if (current.x == goal.x && current.y == goal.y) {
            return reconstruct_path(&current);
        }

        for (auto& neighbor : get_neighbors(grid, &current)) {
            double tentative_g_score = g_score[current.x][current.y] + 1;
            if (tentative_g_score < g_score[neighbor.x][neighbor.y]) {
                neighbor.g = tentative_g_score;
                neighbor.h = heuristic(neighbor.x, neighbor.y, goal.x, goal.y);
                neighbor.parent = new Node(current);
                g_score[neighbor.x][neighbor.y] = tentative_g_score;
                open_set.push(neighbor);
            }
        }
    }
    return {};
}

int main() {
    Grid grid = {
        {0, 1, 0, 0, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0}
    };

    Node start(0, 0);
    Node goal(4, 4);

    vector<Node> path = a_star(grid, start, goal);

    if (!path.empty()) {
        for (const auto& node : path) {
            cout << "(" << node.x << "," << node.y << ") ";
        }
        cout << endl;
    } else {
        cout << "No path found" << endl;
    }

    return 0;
}
```
