# Dynamic Programming

Dynamic programming is an optimization technique that solves complex problems by breaking them down into simpler subproblems. It is particularly useful for problems with overlapping subproblems and optimal substructure.

## Example: Memoization and Tabulation Techniques

### Memoization

```cpp
#include <iostream>
#include <vector>

using namespace std;

int fib(int n, vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
    return memo[n];
}

int main() {
    int n = 10;
    vector<int> memo(n + 1, -1);
    cout << "Fibonacci of " << n << " is " << fib(n, memo) << endl;
    return 0;
}
```

### Tabulation

```cpp

#include <iostream>
#include <vector>

using namespace std;

int fib(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}

int main() {
    int n = 10;
    cout << "Fibonacci of " << n << " is " << fib(n) << endl;
    return 0;
}

```
