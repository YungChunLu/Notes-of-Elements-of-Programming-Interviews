### BootCamp

* Tips

  * What makes DP different than divide-and-conquer is that the same subproblem may reoccur.
  * The key to solving a DP problem efficiently is finding a way to break the problem into subproblems such that
    * the original problem can be solved relatively easily once solutions to the subproblems are available
    * these subproblem solutions are cached
  * Consider using DP whenever you have to \*\*make choices\*\* to arrive at the solution. Specifically, DP is applicable when you can construct a solution to the given instance from solutions to sub-instances of smaller problems of the same kind.
  * In addition to optimization problems, DP is also applicable to **counting and decision problems** - any problem where you can express a solution recursively in terms of the same computation on smaller instances.
  * Although conceptually DP involves recursion, often for efficiency **the cache is built bottom-up**.
  * To save space, **cache space** may be **recycled** once it is known that a set of entries will not be looked up again.
  * Sometimes, **recursion may out-perform a bottom-up DP solution**, e.g., when the solution is found early or subproblems can be **pruned** through bounding.

* Find the maximum sum over all subarrays of a given array of integer

```cpp
// n: The number of integer
// Time Complexity: O(n)
// Space Complexity: O(1)

int FindMaximumSubarray(const vector<int>& A) {
    int min_sum = 0, running_sum = 0, max_sum = 0;
    for (int i = 0; i < A.size(); i++) {
        running_sum += A[i];
        if (running_sum < min_sum) {
            min_sum = running_sum;
        }
        if (running_sum - min_sum > max_sum) {
            max_sum = running_sum - min_sum;
        }
    }
    return max_sum;
}
```



