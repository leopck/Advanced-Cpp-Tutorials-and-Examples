# Dijkstra's Algorithm

Dijkstra's Algorithm is used to find the shortest path between nodes in a graph, which may represent, for example, road networks.

## Example: Implementing Dijkstra's Shortest Path Algorithm

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>

using namespace std;

const int INF = numeric_limits<int>::max();

struct Edge {
    int to, weight;
};

using Graph = vector<vector<Edge>>;

vector<int> dijkstra(const Graph& graph, int start) {
    vector<int> distances(graph.size(), INF);
    distances[start] = 0;

    using P = pair<int, int>;
    priority_queue<P, vector<P>, greater<P>> pq;
    pq.emplace(0, start);

    while (!pq.empty()) {
        int dist, u;
        tie(dist, u) = pq.top();
        pq.pop();

        if (dist > distances[u]) continue;

        for (const Edge& edge : graph[u]) {
            int v = edge.to, weight = edge.weight;
            if (distances[u] + weight < distances[v]) {
                distances[v] = distances[u] + weight;
                pq.emplace(distances[v], v);
            }
        }
    }

    return distances;
}

int main() {
    int n = 5;
    Graph graph(n);
    graph[0].push_back({1, 10});
    graph[0].push_back({4, 5});
    graph[1].push_back({2, 1});
    graph[1].push_back({4, 2});
    graph[2].push_back({3, 4});
    graph[3].push_back({0, 7});
    graph[3].push_back({2, 6});
    graph[4].push_back({1, 3});
    graph[4].push_back({2, 9});
    graph[4].push_back({3, 2});

    vector<int> distances = dijkstra(graph, 0);

    for (int i = 0; i < distances.size(); ++i) {
        cout << "Distance to node " << i << " is " << distances[i] << endl;
    }

    return 0;
}
```
