# Modules

C++20 introduces modules, which are a modern replacement for header files and provide better compilation performance and modularity.

## Example: Introduction to C++20 Modules

### Creating a Module

#### `math.ixx`

```cpp
export module math;

export int add(int a, int b) {
    return a + b;
}

export int subtract(int a, int b) {
    return a - b;
}
```

Using a Module

`main.cpp`

```cpp
import math;
#include <iostream>

int main() {
    std::cout << "5 + 3 = " << add(5, 3) << std::endl;
    std::cout << "5 - 3 = " << subtract(5, 3) << std::endl;
    return 0;
}

```

Building and Running

```sh
g++ -std=c++20 -fmodules-ts math.ixx -c -o math.o
g++ -std=c++20 main.cpp math.o -o main
./main
```


