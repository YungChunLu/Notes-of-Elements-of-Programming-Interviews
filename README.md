# Introduction

### General steps for cracking an interview question

1. Ask for the input format.
2. Use some easy input to validate my own idea about the question.
3. Think a brute-force algorithm to solve the question.
4. Derive the time and space complexity functions.
5. Try to improve the brute-force algorithm.

### Questions

* Maximum Profit

```cpp
// n: The size of input
// Time Complexity O(nlogn)
// Space Complexity O(logn)
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



