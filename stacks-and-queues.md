### Bootcamp

* Tips
  * A stack supports two basic operations, **push** and **pop**.
  * Need to recognize the stack **LIFO** property when it's applicable. For example, **parsing** typically benefits from a stack.
  * A queue supports two basic operations, **enqueue** and **dequeue**.
  * A** deque** is called a double-ended queue. For insertion, to the front is called **pop**, to the back is called **inject**. For deletion, from the front is called **pop**, from the back is called **eject**.
  * Need to recognize the queue **FIFO** property when it's applicable. For example, it's useful when the order needs to be preserved.
* Know the stack libraries

```markdown
1. Basic methods: top(), push(42), emplace(42), pop()
```

* Know the queue libraries

```markdown
1. Basic methods: front(), back(), push(42), emplace(42), pop()
```

### 8-1 Implement A Stack With Max API

* Author's solution1

```cpp
// n: The number of elements in the stack
// Time Complexity: O(1)
// Space Complexity: O(n)

class Stack {
public:
    bool Empty() const {
        return element_with_cached_max_.empty();
    }

    int Max() const {
        if (Empty()) {
            throw length_error("Max(): empty stack");
        }
        return element_with_cached_max_.top().max;
    }

    int Pop() {
        if (Empty()) {
            throw length_error("Pop(): empty stack");
        }
        int element = element_with_cached_max_.top().element;
        element_with_cached_max_.pop();
        return element;
    }

    void Push(int x) {
        element_with_cached_max_.emplace(ElementWithCachedMax{x, max(x, Empty() ? x : element_with_cached_max_.top().max)});
    }
private:
    struct ElementWithCachedMax {
        int element, max;
    };
    stack<ElementWithCachedMax> element_with_cached_max_;
};
```

* Author's solution2 - Improve space complexity on best-case

```cpp
// n: The number of elements in the stack
// Time Complexity: O(1)
// Space Complexity: O(n)

class Stack {
public:
    bool Empty() const {
        return element_.empty();
    }

    int Max() const {
        if (Empty()) {
            throw length_error("Max(): empty stack");
        }
        return cached_max_with_count_.top().max;
    }

    int Pop() {
        if (Empty()) {
            throw length_error("Pop(): empty stack");
        }
        int element = element_.top();
        element_.pop();
        const int current_max = cached_max_with_count_.top().max;
        if (element == current_max) {
            int max_count = cached_max_with_count_.top().count;
            if (--max_count == 0) {
                cached_max_with_count_.pop();
            }
        }
        return element;
    }

    void Push(int x) {
        element_.emplace(x);
        if (cached_max_with_count_.empty()) {
            cached_max_with_count_.emplace(MaxWithCount{x, 1});
        }
        else {
            const int current_max = cached_max_with_count_.top().max;
            if (x == current_max) {
                int& max_frequency = cached_max_with_count_.top().count;
                max_frequency++;
            }
            else if (x > current_max) {
                cached_max_with_count_.emplace(MaxWithCount{x, 1});
            }
        }
    }
private:
    stack<int> element_;
    struct MaxWithCount {
        int max, count;
    };
    stack<MaxWithCount> cached_max_with_count_;
};
```

### 8-6 Compute Binary Tree Nodes In Order Of Increasing Depth



