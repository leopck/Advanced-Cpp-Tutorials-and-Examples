# State Machine

## Introduction
A state machine transitions between states based on conditions.

## Code Example
```cpp
#include <iostream>
#include <unordered_map>
#include <functional>
#include <string>

class StateMachine {
public:
    using StateFunction = std::function<void()>;
    using Condition = std::function<bool()>;

    void add_state(const std::string& state_name, StateFunction state_function) {
        states[state_name] = state_function;
    }

    void add_transition(const std::string& from, const std::string& to, Condition condition) {
        transitions[from].emplace_back(to, condition);
    }

    void set_state(const std::string& state_name) {
        current_state = state_name;
    }

    void execute() {
        if (current_state.empty() || states.find(current_state) == states.end()) {
            std::cerr << "Invalid state: " << current_state << std::endl;
            return;
        }

        states[current_state]();

        for (const auto& [next_state, condition] : transitions[current_state]) {
            if (condition()) {
                current_state = next_state;
                break;
            }
        }
    }

private:
    std::unordered_map<std::string, StateFunction> states;
    std::unordered_map<std::string, std::vector<std::pair<std::string, Condition>>> transitions;
    std::string current_state;
};

int main() {
    StateMachine machine;

    machine.add_state("idle", []() { std::cout << "Idle state\n"; });
    machine.add_state("running", []() { std::cout << "Running state\n"; });
    machine.add_state("stopped", []() { std::cout << "Stopped state\n"; });

    bool should_run = true;
    machine.add_transition("idle", "running", [&should_run]() { return should_run; });
    machine.add_transition("running", "stopped", [&should_run]() { return !should_run; });

    machine.set_state("idle");
    machine.execute();  // Output: Idle state

    should_run = false;
    machine.execute();  // Output: Running state, then Stopped state

    return 0;
}
```
