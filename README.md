# Introduction

### General steps for cracking an interview question

1. Ask for the input format.
2. Use some easy input to validate my own idea about the question.
3. Think a brute-force algorithm to solve the question.
4. Derive the time and space complexity functions.
5. Try to improve the brute-force algorithm.

### Questions

* Maximum Profit - Solution1

```cpp
// n: The size of input
// Time Complexity: O(nlogn)
// Space Complexity: O(nlogn)

// Test Case:
//     1. S = [] => error
//     2. S = [6] => {0, 6, 6}
//     3. S = [10, 9, 7] => {0, 10, 10}
//     4. S = [3, 9, 10, 2] => {7, 10, 3}
struct Strategy{
    int earning, highest, lowest;
};

Strategy getStrategy(vector<int> S){
    if (S.size() == 0) {
        throw length_error("Empty Input");
    } else if (S.size() == 1){
        // Base Case
        return Strategy{0, S.front(), S.front()};
    } else{
        // Split vectors in half
        int halfsize = (int) S.size() / 2;
        vector<int> left_S(S.begin(), S.begin()+halfsize);
        vector<int> right_S(S.begin()+halfsize, S.end());
        // Get results from left and right vectors
        Strategy L = getStrategy(left_S);
        Strategy R = getStrategy(right_S);
        // Merge results from left and right vectors
        Strategy Merge = Strategy{R.highest-L.lowest, R.highest, L.lowest};
        // Get the best result
        return max({L, R, Merge},
                   [](const Strategy a, const Strategy b){
                       return a.earning < b.earning;
                   });
    }
};
```

* Maximum Profit - Solution2

```cpp
// n: The size of input
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. S = [] => error
//     2. S = [6] => {0, 6, 6}
//     3. S = [10, 9, 7] => {0, 7, 7}
//     4. S = [3, 9, 10, 2] => {7, 10, 3}
struct Strategy{
    int earning, lowest;
};

Strategy getStrategy(vector<int> S){
    int best=0, lowest=S.front();
    for (vector<int>::iterator it=++S.begin(); it<S.end(); ++it) {
        int profit = *it-lowest;
        if (profit > best) {
            best = profit;
        } else if (*it < lowest){
            lowest = *it;
        }
    }
    return {best, lowest};
};
```



