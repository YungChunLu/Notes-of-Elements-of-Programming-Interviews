### BootCamp

* Tips

  * Two key ingredients to a successful use recursion are identifying the base cases and ensuring progress of converging to the solution.
  * Recursion is especially suitable when the **input is expressed using recursive rules** such as a computer grammar.

  * Recursion is good choice for **search, enumeration and divide-and-conquer**

  * Use recursion as **alternative to deeply nested iteration** loops

  * When **removing recursion** from a program, consider mimicking call stack with the **stack data structure**.

  * Recursion can be easily removed from a **tail-recursive** program by using a while-loop - no stack is needed.

  * If a recursive function may end up being called with the **same arguments** more than once, **cache** the results.

### 15-1 The Towers Of Hanoi Problem

* Author's Solution

```cpp
// n: The number of rings
// Time Complexity: O(2^n)
// Space Complexity: O(n)

void MoveHanoiRing(array<stack<int>, kNumPegs> &pegs, int from_peg, int to_peg,
                  vector<vector<int>> *result_ptr) {
    if ((pegs[from_peg].size() == 0) ||
        (pegs[to_peg].size() > 0 && pegs[from_peg].top() > pegs[to_peg].top())) {
        swap(from_peg, to_peg);
    }
    int ring = pegs[from_peg].top();
    pegs[from_peg].pop();
    pegs[to_peg].push(ring);
    result_ptr->emplace_back(vector<int>{ring, from_peg, to_peg});
}

void ComputeTowerHanoiRecursive (int num_rings_to_move, array<stack<int>, kNumPegs> &pegs,
                                 int from_peg, int to_peg, int use_peg,
                                 vector<vector<int>> *result_ptr){
    if (num_rings_to_move > 0) {
        ComputeTowerHanoiRecursive(num_rings_to_move-1, pegs, from_peg, use_peg, to_peg, result_ptr);
        MoveHanoiRing(pegs, from_peg, to_peg, result_ptr);
        ComputeTowerHanoiRecursive(num_rings_to_move-1, pegs, use_peg, to_peg, from_peg, result_ptr);
    }
}

vector<vector<int>> ComputeTowerHanoi (int num_rings,
                                       function<void(int, array<stack<int>, kNumPegs>&, int, int, int, vector<vector<int>>*)> f) {
    array<stack<int>, kNumPegs> pegs;
    // Initialize pegs
    for (int i = num_rings; i > 0; i--) {
        pegs[0].push(i);
    }
    vector<vector<int>> result;
    f(num_rings, pegs, 0, 1, 2, &result);
    return result;
}
```

### 15-1 Variant

* Solve the same problem without using recursion

```cpp
// n: The number of rings
// Time Complexity: O(2^n)
// Space Complexity: O(1)

void ComputeTowerHanoiIterative(int num_rings_to_move, array<stack<int>, kNumPegs> &pegs,
                                int from_peg, int to_peg, int use_peg,
                                vector<vector<int>> *result_ptr) {
    int step = 0, dst_pos = to_peg;
    if (pegs[from_peg].size() % 2 == 0) {
        swap(to_peg, use_peg);
    }
    while (pegs[dst_pos].size() != num_rings_to_move) {
        if (step % 3 == 0) {
            MoveHanoiRing(pegs, from_peg, to_peg, result_ptr);
        } else if (step % 3 == 1) {
            MoveHanoiRing(pegs, from_peg, use_peg, result_ptr);
        } else {
            MoveHanoiRing(pegs, to_peg, use_peg, result_ptr);
        }
        step += 1;
    }
}
```

* Find the minimum number of operations subject to the constraint that each operation must involve P3. The number of operations is double than the normal one.

```cpp
// n: The number of rings
// Time Complexity: O(2^(n+1))
// Space Complexity: O(n)

void ComputeTowerHanoiRecursive_Constraint1 (int num_rings_to_move, array<stack<int>, kNumPegs> &pegs,
                                             int from_peg, int to_peg, int use_peg,
                                             vector<vector<int>> *result_ptr){
    if (num_rings_to_move > 0) {
        ComputeTowerHanoiRecursive_Constraint1(num_rings_to_move-1, pegs, from_peg, to_peg, use_peg, result_ptr);
        MoveHanoiRing(pegs, from_peg, use_peg, result_ptr);
        ComputeTowerHanoiRecursive_Constraint1(num_rings_to_move-1, pegs, to_peg, from_peg, use_peg, result_ptr);
        MoveHanoiRing(pegs, use_peg, to_peg, result_ptr);
    }
}
```

* Find the minimum number of operations subject to the constraint each transfer must be from P1 to P2, P2 to P3, or P3 to P1. The number of operations is 

```cpp
// n: The number of rings
// Time Complexity: O(2^2n)
// Space Complexity: O(n)

void ComputeTowerHanoiRecursive_Constraint2 (int num_rings_to_move, array<stack<int>, kNumPegs> &pegs,
                                             int from_peg, int to_peg, int use_peg,
                                             vector<vector<int>> *result_ptr){
    if (num_rings_to_move > 0) {
        ComputeTowerHanoiRecursive_Constraint2(num_rings_to_move-1, pegs, from_peg, to_peg, use_peg, result_ptr);
        ComputeTowerHanoiRecursive_Constraint2(num_rings_to_move-1, pegs, to_peg, use_peg, from_peg, result_ptr);
        MoveHanoiRing(pegs, from_peg, to_peg, result_ptr);
        ComputeTowerHanoiRecursive_Constraint2(num_rings_to_move-1, pegs, use_peg, from_peg, to_peg, result_ptr);
        ComputeTowerHanoiRecursive_Constraint2(num_rings_to_move-1, pegs, from_peg, to_peg, use_peg, result_ptr);
    }
}
```

* Find the minimum number of operations subject to the constraint that a ring can never be transferred directly from P1 to P2.

```cpp
// n: The number of rings
// Time Complexity: O(3^n)
// Space Complexity: O(n)

void ComputeTowerHanoiRecursive_Constraint3 (int num_rings_to_move, array<stack<int>, kNumPegs> &pegs,
                                             int from_peg, int to_peg, int use_peg,
                                             vector<vector<int>> *result_ptr){
    if (num_rings_to_move > 0) {
        ComputeTowerHanoiRecursive_Constraint3(num_rings_to_move-1, pegs, from_peg, to_peg, use_peg, result_ptr);
        if (from_peg > to_peg) {
            MoveHanoiRing(pegs, from_peg, to_peg, result_ptr);
        } else {
            MoveHanoiRing(pegs, from_peg, use_peg, result_ptr);
            ComputeTowerHanoiRecursive_Constraint3(num_rings_to_move-1, pegs, to_peg, from_peg, use_peg, result_ptr);
            MoveHanoiRing(pegs, use_peg, to_peg, result_ptr);
        }
        ComputeTowerHanoiRecursive_Constraint3(num_rings_to_move-1, pegs, from_peg, to_peg, use_peg, result_ptr);
    }
}
```

* Compute the minimum number of moves to transfer the 2n disks from P1 to P2.

```cpp
// n: The number of size
// Time Complexity: O(2^(n+1))
// Space Complexity: O(n)

void MoveDisk(array<stack<int>, kNumPegs> &pegs, int from_peg, int to_peg,
              vector<vector<int>> *result_ptr) {
    int ring = pegs[from_peg].top();
    for (int i = 0; i < 2; i++) {
        pegs[from_peg].pop();
        pegs[to_peg].push(ring);
        result_ptr->emplace_back(vector<int>{ring, from_peg, to_peg});
    }
}

void ComputeDiskRecursive(int num_disks_to_move, array<stack<int>, kNumPegs> &pegs,
                          int from_peg, int to_peg, int use_peg,
                          vector<vector<int>> *result_ptr){
    if (num_disks_to_move > 0) {
        ComputeDiskRecursive(num_disks_to_move-2, pegs, from_peg, use_peg, to_peg, result_ptr);
        MoveDisk(pegs, from_peg, to_peg, result_ptr);
        ComputeDiskRecursive(num_disks_to_move-2, pegs, use_peg, to_peg, from_peg, result_ptr);
    }
}

vector<vector<int>> ComputeDisk (int num_size_to_move) {
    array<stack<int>, kNumPegs> pegs;
    // Initialize pegs
    for (int i = num_size_to_move; i > 0; i--) {
        pegs[0].push(i);
        pegs[0].push(i);
    }
    vector<vector<int>> result;
    ComputeDiskRecursive(2*num_size_to_move, pegs, 0, 1, 2, &result);
    return result;
}
```



