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

### 16-1 Count The Number Of Score Combinations

* Mine Solution

```cpp
// n: The number of play scores, s: The value of final score
// Time Complexity: O(sn)
// Space Complexity: O(sn)

int NumCombinationsForFinalScore_M(int final_score, const vector<int>& individual_play_scores) {
    vector<vector<int>> num_combinations_for_score(individual_play_scores.size(),
                                                   vector<int>(final_score+1, 0));
    if (individual_play_scores.size() == 0) {
        return 0;
    }
    // Initialize the first row
    for (int score = 0; score <= final_score; score++) {
        int play_score = individual_play_scores[0];
        if (score % play_score == 0) {
            num_combinations_for_score[0][score] = 1;
        }
    }
    for (int score = 0; score <= final_score; score++) {
        for (int play_index = 1; play_index < individual_play_scores.size(); play_index++) {
            int play_score = individual_play_scores[play_index];
            num_combinations_for_score[play_index][score] = num_combinations_for_score[play_index-1][score];
            if (score - play_score >= 0) {
                num_combinations_for_score[play_index][score] += num_combinations_for_score[play_index][score - play_score];
            }
        }
    }
    return num_combinations_for_score[individual_play_scores.size()-1][final_score];
}
```

* Author's Solution

```cpp
// n: The number of play scores, s: The value of final score
// Time Complexity: O(sn)
// Space Complexity: O(sn)

int NumCombinationsForFinalScore_A(int final_score, const vector<int>& individual_play_scores) {
    vector<vector<int>> num_combinations_for_score(individual_play_scores.size(),
                                                   vector<int>(final_score+1, 0));
    for (int i = 0; i < individual_play_scores.size(); i++) {
        for (int j = 0; j <= final_score; j++) {
            int without_this_play = i >= 1 ? num_combinations_for_score[i-1][j] : 0;
            int this_play = individual_play_scores[i];
            int with_this_play = j >= this_play ? num_combinations_for_score[i][j-this_play] : 0;
            num_combinations_for_score[i][j] = with_this_play + without_this_play;
        }
    }
    return num_combinations_for_score.back().back();
}
```

### 16-1 Variant

* Solve the same problem using O\(s\) space.

```cpp
// n: The number of play scores, s: The value of final score
// Time Complexity: O(sn)
// Space Complexity: O(s)

int NumCombinationsForFinalScoreReduced(int final_score, const vector<int>& individual_play_scores) {
    vector<int> num_combinations_for_score(vector<int>(final_score+1, 0));
    num_combinations_for_score[0] = 1;
    for (int i = 0; i < individual_play_scores.size(); i++) {
        for (int j = 0; j <= final_score; j++) {
            int this_play = individual_play_scores[i];
            int with_this_play = j >= this_play ? num_combinations_for_score[j-this_play] : 0;
            num_combinations_for_score[j] += with_this_play;
        }
    }
    return num_combinations_for_score.back();
}
```

* Write a problem returning the sequences of plays

```cpp
// n: The number of play scores, s: The value of final score
// Time Complexity: O(sn+nlogn)
// Space Complexity: O(s)

vector<vector<int>> CombinationsForFinalScoreHelper(int score, const vector<int> &play_scores,
                                                    unordered_map<int, vector<vector<int>>> &cached_combinations) {
    if (score < 0) {
        return {};
    }
    if (cached_combinations.find(score) == cached_combinations.end()) {
        vector<vector<int>> total_combinations = {};
        for (int play_score : play_scores) {
            vector<vector<int>> combinations = CombinationsForFinalScoreHelper(score - play_score, play_scores,
                                                                               cached_combinations);
            for (vector<int> combination : combinations) {
                combination.push_back(play_score);
                total_combinations.push_back(combination);
            }
        }
        cached_combinations[score] = total_combinations;
    }
    return cached_combinations[score];
}

vector<vector<int>> CombinationsForFinalScore(int final_score, vector<int> individual_play_scores) {
    sort(individual_play_scores.begin(), individual_play_scores.end());
    unordered_map<int, vector<vector<int>>> cached_combinations;
    cached_combinations[0] = {{}};
    for (int play_score : individual_play_scores) {
        cached_combinations[play_score] = CombinationsForFinalScoreHelper(play_score, individual_play_scores, cached_combinations);
    }
    return CombinationsForFinalScoreHelper(final_score, individual_play_scores, cached_combinations);
}
```

* Compute the number of distinct scoring sequences in the form \(s, s'\)

```cpp
// n: The number of play scores, s, p: The score of each team
// Time Complexity: O(sn+pn+nlogn)
// Space Complexity: O(sp)


typedef tuple<int, int> team_score;
struct key_hash : public unary_function<team_score, size_t>
{
    size_t operator()(const team_score& k) const
    {
        return get<0>(k) ^ get<1>(k);
    }
};
typedef unordered_map<const team_score, int, key_hash> team_score_cached;

int NumCombinationsForTeamsHelper(team_score team_scores, vector<int> &individual_play_scores,
                                  team_score_cached cached_combinations){
    int teamA = get<0>(team_scores), teamB = get<1>(team_scores);
    if (teamA < 0 || teamB < 0) {
        return 0;
    }
    if (cached_combinations.find(team_scores) == cached_combinations.end()) {
        int total_combinations = 0;
        for (int play_score : individual_play_scores) {
            team_score score1 = {teamA-play_score, teamB}, score2 = {teamA, teamB-play_score};
            total_combinations += NumCombinationsForTeamsHelper(score1, individual_play_scores, cached_combinations);
            total_combinations += NumCombinationsForTeamsHelper(score2, individual_play_scores, cached_combinations);
        }
        cached_combinations[team_scores] = total_combinations;
    }
    return cached_combinations[team_scores];
}

int NumCombinationsForTeams(team_score team_scores, vector<int> individual_play_scores) {
    sort(individual_play_scores.begin(), individual_play_scores.end());
    team_score_cached cached_combinations;
    cached_combinations[{0, 0}] = 1;
    for (int play_score : individual_play_scores) {
        team_score score1 = {play_score, 0}, score2 = {0, play_score};
        cached_combinations[score1] = NumCombinationsForTeamsHelper(score1, individual_play_scores, cached_combinations);
        cached_combinations[score2] = NumCombinationsForTeamsHelper(score2, individual_play_scores, cached_combinations);
    }
    return NumCombinationsForTeamsHelper(team_scores, individual_play_scores, cached_combinations);
}
```



