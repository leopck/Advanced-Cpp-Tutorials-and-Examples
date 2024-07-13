# Greedy Algorithms

Greedy algorithms build up a solution piece by piece, always choosing the next piece that offers the most immediate benefit.

## Example: Implementing Greedy Algorithms for Optimization Problems

### Activity Selection Problem

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Activity {
    int start, finish;
};

bool activityCompare(Activity s1, Activity s2) {
    return (s1.finish < s2.finish);
}

void printMaxActivities(vector<Activity> activities) {
    sort(activities.begin(), activities.end(), activityCompare);

    cout << "Selected activities are: " << endl;
    int i = 0;
    cout << "(" << activities[i].start << ", " << activities[i].finish << ")" << endl;

    for (int j = 1; j < activities.size(); j++) {
        if (activities[j].start >= activities[i].finish) {
            cout << "(" << activities[j].start << ", " << activities[j].finish << ")" << endl;
            i = j;
        }
    }
}

int main() {
    vector<Activity> activities = {{5, 9}, {1, 2}, {3, 4}, {0, 6}, {5, 7}, {8, 9}};
    printMaxActivities(activities);
    return 0;
}
```
