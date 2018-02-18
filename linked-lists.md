### Bootcamp

* Tips
  * **list** class is a doubly-linked list
  * **forward\_list** class is a singly-linked list
  * Consider using a **dummy head** to avoid having to check for empty lists
  * Use **sort** to sort lists
* Know the array libraries

```markdown
1. insert and delete elements: push_front, emplace_front, pop_front, push_back, emplace_back, pop_back
2.
```

### 7-1 Merge Two Sorted Lists

* Author's solution

```cpp
// m, n: The size of the two input lists
// Time Complexity: O(m+n)
// Space Complexity: O(1)

// Test Cases:
//     1. L1 = {1, 2}, L2 = {3, 4} => {1, 2, 3, 4}
//     2. L1 = {1, 5}, L2 = {3, 4} => {1, 3, 4, 5}
//     3. L1 = {2, 5}, L2 = {1, 3} => {1, 2, 3, 5}

shared_ptr<ListNode<int>> MergeTwoSortedLists(shared_ptr<ListNode<int>> L1,
                                              shared_ptr<ListNode<int>> L2) {
    shared_ptr<ListNode<int>> dummy_head(new ListNode<int>);
    auto tail = dummy_head;
    while (L1 && L2) {
        AppendNode(L1->data <= L2->data ? &L1 : &L2, &tail);
    }
    tail->next = L1 ? L1 : L2;
    return dummy_head->next;
}
```

### 7-2 Reverse A Single Sublist

* Author's solution

```cpp
// n: The number of finish
// Time Complexity: O(n)
// Space Complexity: O(1)


// Test Cases:
//     1. L = {1, 2, 3, 4, 5}, start = 1, finish = 2 => {2, 1, 3, 4, 5}
//     2. L = {1, 2, 3, 4, 5}, start = 1, finish = 5 => {5, 4, 3, 2, 1}
//     3. L = {1, 2, 3, 4, 5}, start = 2, finish = 4 => {1, 4, 3, 2, 5}

shared_ptr<ListNode<int>> ReverseSublist(shared_ptr<ListNode<int>> L, int start, int finish) {
    shared_ptr<ListNode<int>> dummy_head(new ListNode<int>({0, L}));
    auto sublist_head = dummy_head;
    for (int k = 1; k < start; k++) {
        sublist_head = sublist_head->next;
    }
    auto sublist_tail = sublist_head->next;
    while (start++ < finish) {
        auto temp = sublist_tail->next;
        sublist_tail->next = temp->next;
        temp->next = sublist_head->next;
        sublist_head->next = temp;
    }
    return dummy_head->next;
}
```

### 7-2 Variant

* Reverse a singly linked list

```cpp
// n: The number of elements in the list, k: The number of nodes at a time
// Time Complexity: O(n(n+k)/k)
// Space Complexity: O(1)

// Test Cases:
//     1. L = {1, 2, 3, 4, 5, 6} => {2, 1, 4, 3, 6, 5}
//     2. L = {1, 2, 3, 4, 5, 6} => {3, 2, 1, 6, 5, 4}
//     3. L = {1, 2, 3, 4, 5, 6} => {4, 3, 2, 1, 5, 6}

shared_ptr<ListNode<int>> ReverseList(shared_ptr<ListNode<int>> L) {
    auto dummy_head(new ListNode<int>({0, L}));
    auto tail = dummy_head->next;
    while (tail->next) {
        auto temp = tail->next;
        tail->next = temp->next;
        temp->next = dummy_head->next;
        dummy_head->next = temp;
    }
    return dummy_head->next;
}
```

* Reverse k nodes at a time

```cpp
// n: The number of elements in the list
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Cases:
//     1. L = {1, 2, 3, 4, 5} => {5, 4, 3, 2, 1}
//     2. L = {1} => {1}
//     3. L = {1, 2, 3} => {3, 2, 1}

shared_ptr<ListNode<int>> ReverseKNodes(shared_ptr<ListNode<int>> L, int k) {
    auto iter = L;
    int start = 1, count = 0;
    while (iter) {
        iter = iter->next;
        if (++count % k == 0) {
            int finish = start + k - 1;
            L = ReverseSublist(L, start, finish);
            start += k;
        }
    }
    return L;
}
```

### 7-3 Test For Cyclicity \(Tricky!\)

* Complex Solution

```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)

shared_ptr<ListNode<int>> HasCycle(const shared_ptr<ListNode<int>>& head) {
    auto fast_iter = head, slow_iter = head;
    while (fast_iter && fast_iter->next) {
        fast_iter = fast_iter->next->next;
        slow_iter = slow_iter->next;
        // It has cycle
        if (fast_iter == slow_iter) {
            // Count the length of the cycle
            int cycle_length = 0;
            do {
                slow_iter = slow_iter->next;
                cycle_length++;
            } while (fast_iter != slow_iter);
            // Make iter ahead of head
            auto iter = head;
            while (cycle_length--) {
                iter = iter->next;
            }
            // Find the head of cycle
            auto cycle_head = head;
            while (cycle_head != iter) {
                iter = iter->next;
                cycle_head = cycle_head->next;
            }
            return cycle_head;
        }
    }
    return nullptr;
}
```

* Neat Solution

```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)

shared_ptr<ListNode<int>> HasCycleNeat(const shared_ptr<ListNode<int>>& head) {
    auto fast_iter = head, slow_iter = head;
    while (fast_iter && fast_iter->next && fast_iter->next->next) {
        slow_iter = slow_iter->next;
        fast_iter = fast_iter->next->next;
        // It has cycle
        if (slow_iter == fast_iter) {
            auto cycle_head = head;
            while (cycle_head != fast_iter) {
                cycle_head = cycle_head->next;
                fast_iter = fast_iter->next;
            }
            return cycle_head;
        }
    }
    return nullptr;
}
```



