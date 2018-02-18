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
// n: The number of elements in the list
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Cases:
//     1. L = {1, 2, 3, 4, 5} => {5, 4, 3, 2, 1}
//     2. L = {1} => {1}
//     3. L = {1, 2, 3} => {3, 2, 1}

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



