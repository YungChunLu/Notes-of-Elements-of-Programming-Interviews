### BootCamp

* Tips

  * What makes DP different than divide-and-conquer is that the same subproblem may reoccur.
  * The key to solving a DP problem efficiently is finding a way to break the problem into subproblems such that

    * the original problem can be solved relatively easily once solutions to the subproblems are available

    * these subproblem solutions are cached

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



