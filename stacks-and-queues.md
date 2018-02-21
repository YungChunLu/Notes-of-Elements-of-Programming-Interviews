### Bootcamp

* Tips
  * Need to recognize the stack **LIFO** property when it's applicable. For example, **parsing** typically benefits from a stack.
* Know the stack libraries

```markdown
1. Basic methods: top(), push(42), emplace(42), pop(),
```

### 8-1 Implement A Stack With Max API

* Author's solution

```
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



