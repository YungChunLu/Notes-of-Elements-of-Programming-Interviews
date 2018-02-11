### BootCamp

* Tips
  * Take advantage of operating efficiently on both ends.
  * Array problems often have simple brute-force solutions that use O\(n\) space. But we sometimes can reduce it to O\(1\).
  * Try to write values from the back because of better efficiency.
  * Try to overwrite an entry instead of deleting it because of better efficiency.
  * Avoid making off-by-1 error - reading past the last element of an array. Otherwise, the consequence is catastrophic.
  * Don't worry about preserving the integrity of the array \(sortedness, keeping equal entries together, etc.\) until it's time to return.
* Know the array libraries

```cpp
// Declare an array: array A = {1, 2, 3}
// Declare a vector: vector<int> A = {1, 2, 3}
// Construct a subarray from an array: vector(A.begin() + i, A.begin() + j)
// Instantiate a 2D array: vector<vector<int>> A = {{1, 2}, {3, 4}, {5, 6}}
```

* Even entries appear first - Solution

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {1} => {1}
//     2. A = {1, 2, 3} => {2, 3, 1}
//     3. A = {1, 2, 3, 4} => {4, 2, 3, 1}
//     4. A = {1, 2} => {2, 1}

void Swap(int& a, int& b){
    int temp = b;
    b = a;
    a = temp;
}

void EvenOdd(vector<int> *A_ptr){
    vector<int>& A = *A_ptr;
    int next_even = 0, next_odd = (int)A.size() - 1;
    while (next_even < next_odd) {
        if ((A[next_even] & 1) == 1){
            Swap(A[next_even], A[next_odd--]);
        }
        else{
            next_even++;
        }
    }
}
```

### 5-1 The Dutch National Flag Problem

* Solution 1 - Brute-force

```cpp
// n: The number of values in array
// Time Complexity: O(n^2)
// Space Complexity: O(1)

// Test Case:
//     1. pivot_index = 2, A = {1, 2, 0, 1} => {0, 1, 1, 2}
//     2. pivot_index = 0, A = {1, 2, 0, 1} => {0, 1, 1, 2}
//     3. pivot_index = 1, A = {1, 2, 1, 0} => {1, 1, 0, 2}

typedef enum {RED, WHITE, BLUE} Color;

void DutchFlagPartition1(int pivot_index, vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    Color pivot = A[pivot_index];
    // Order elements smaller pivot
    for (int i = 0; i < A.size(); i++){
        for (int j = i + 1; j < A.size(); j++){
            if (A[j] < pivot){
                Swap(A[i], A[j]);
                break;
            }
        }
    }
    // Order elements larger than pivot
    for (int i = (int)A.size() - 1; i >= 0 && A[i] >= pivot; i--) {
        for (int j = i - 1; j >= 0 && A[j] >= pivot; j--) {
            if (A[j] > pivot) {
                Swap(A[i], A[j]);
                break;
            }
        }
    }
}
```

* Solution 2 - Single Pass

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. pivot_index = 1, A = {1, 1, 0, 2} => {0, 1, 1, 2}
//     2. pivot_index = 3, A = {0, 0, 1, 2, 1, 1} => {0, 0, 1, 1, 1, 2}
//     3. pivot_index = 1, A = {1, 2, 1, 0, 1, 2} => {1, 1, 0, 1, 2, 2}

void DutchFlagPartition2(int pivot_index, vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    Color pivot = A[pivot_index];
    // Order elements smaller pivot
    int next_smaller = 0;
    for (int i = 0; i < A.size(); i++){
        if (A[i] < pivot){
            Swap(A[i], A[next_smaller++]);
        }
    }
    // Order elements larger than pivot
    int next_larger = (int)A.size() - 1;
    for (int i = (int)A.size() - 1; i >= 0 && A[i] >= pivot; i--) {
        if (A[i] > pivot) {
            Swap(A[i], A[next_larger--]);
        }
    }
}
```

* Solution 3 - Tricky Implementation

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. pivot_index = 1, A = {1, 1, 0, 2} => {0, 1, 1, 2}
//     2. pivot_index = 3, A = {0, 0, 1, 2, 1, 1} => {0, 0, 1, 1, 1, 2}
//     3. pivot_index = 1, A = {1, 2, 1, 0, 1, 2} => {1, 1, 0, 1, 2, 2}

void DutchFlagPartition3(int pivot_index, vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    Color pivot = A[pivot_index];
    int less_index = 0, equal_index = 0, larger_index = (int)A.size();
    while (equal_index < larger_index){
        if (A[equal_index] < pivot) {
            Swap(A[equal_index++], A[less_index++]);
        }
        else if (A[equal_index] == pivot){
            equal_index++;
        }
        else{
            Swap(A[equal_index], A[--larger_index]);
        }
    }
}
```

### 5-1 Variant

* Group the same color

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {1, 1} => {1, 1}
//     2. A = {0, 0, 1, 2, 1, 1} => {0, 0, 1, 1, 1, 2}
//     3. A = {1, 2, 1, 2} => {1, 1, 2, 2}

typedef enum {RED, WHITE, BLUE} Color;

void Variant5_1_1(vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    int head = 0, tail = (int)A.size(), unclassfied = 0;
    while (unclassfied < tail) {
        if (A[unclassfied] < WHITE){
            Swap(A[unclassfied++], A[head++]);
        }
        else if (A[unclassfied] > WHITE){
            Swap(A[unclassfied], A[--tail]);
        }
        else{
            unclassfied++;
        }
    }
}
```

* Group the same key

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {0} => {0}
//     2. A = {0, 3} => {0, 3}
//     3. A = {0, 3, 2, 1} => {0, 1, 2, 3}
//     4. A = {0, 3, 1, 2, 3, 0} => {0, 0, 1, 3, 3, 2}

typedef enum {K0, K1, K2, K3} Variant5_1_2_Key;

void Variant5_1_2(vector<Variant5_1_2_Key>* A_ptr){
    vector<Variant5_1_2_Key> &A = *A_ptr;
    int K0_index = 0, K1_index = 0, K2_index = (int)A.size(), K3_index = (int)A.size(), unclassified = 0;
    while ((unclassified < K2_index) && (unclassified < K3_index)) {
        if (A[unclassified] == K0) {
            Swap(A[unclassified++], A[K0_index++]);
            K1_index++;
        }
        else if (A[unclassified] == K1){
            Swap(A[unclassified++], A[K1_index++]);
        }
        else if (A[unclassified] == K3){
            Swap(A[unclassified], A[--K3_index]);
        }
        else{
            Swap(A[unclassified], A[--K2_index]);
        }
    }
}
```

* Group the same boolean key

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {false} => {false}
//     2. A = {true, false} => {false, true}
//     3. A = {true, false, false} => {0, 1, 2, 3}
//     4. A = {true, false, true, false} => {false, false, true, true}

void Variant5_1_3(vector<bool>* A_ptr){
    vector<bool> &A = *A_ptr;
    int true_index = (int)A.size(), unclassified_index = 0;
    while (unclassified_index < true_index) {
        if (A[unclassified_index]) {
            swap(A[unclassified_index], A[--true_index]);
        }
        else{
            unclassified_index++;
        }
    }
}
```

### 5-6 Buy and Sell a stock once

* Solution 1 - Mine implementation

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. prices = {310, 315, 275, 295, 260, 270, 290, 230, 255, 250} => 30
//     2. prices = {310} => 0
//     3. prices = {310, 315, 275} => 5
//     4. prices = {310, 315, 275, 310} => 35

double BuyAndSellStockOnceMine(const vector<double>& prices){
    double profit = 0;
    if (prices.size() > 1){
        double buy = prices[0];
        for (int i = 1; i < prices.size(); i++){
            double diff = prices[i] - buy;
            if (diff > profit) {
                profit = diff;
            }
            else if (prices[i] < buy){
                buy = prices[i];
            }
        }
    }
    return profit;
}
```

* Solution 2 - author's implementation

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. prices = {310, 315, 275, 295, 260, 270, 290, 230, 255, 250} => 30
//     2. prices = {310} => 0
//     3. prices = {310, 315, 275} => 5
//     4. prices = {310, 315, 275, 310} => 35

double BuyAndSellStockOnceAuthor(const vector<double>& prices){
    double min_price_so_far = numeric_limits<double>::max(), profit = 0;
    for (const double& price : prices) {
        double max_profit_so_far = price - min_price_so_far;
        profit = max(profit, max_profit_so_far);
        min_price_so_far = min(min_price_so_far, price);
    }
    return profit;
}
```

### 5-6 Variant

* The length of a longest subarray

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. vals = {} => 0
//     2. vals = {1} => 1
//     3. vals = {2, 1, 1} => 2
//     4. vals = {2, 1, 1, 1, 3} => 3

int LongestSubArrayLength(const vector<int>& vals){
    int longest_length = 0, length = 0, current_val = numeric_limits<int>::max();
    for (const int& val : vals){
        if (val == current_val){
            length++;
        }
        else{
            longest_length = max(length, longest_length);
            current_val = val;
            length = 1;
        }
    }
    longest_length = max(length, longest_length);
    return longest_length;
}
```

### 5-12 Sample Offline Data

* Given an array of distinct elements and a size, returns a subset of the given size of the array elements. All subsets should be equally likely.

```cpp
// k: The size of a selected subset
// Time Complexity: O(k)
// Space Complexity: O(1)

void RandomSampling(int k, vector<int>* A_ptr){
    vector<int>& A = *A_ptr;
    default_random_engine seed((random_device())());
    for (int i = 0; i < k; i++) {
        swap(A[i], A[uniform_int_distribution<int>{i, static_cast<int>(A.size()) - 1}(seed)]);
    }
}

// Test helper function
map<vector<int>, int> Counts = {};
vector<int> nums = {1, 4, 3};
int k = 2;
for (int n = 0; n < 200; n++) {
    RandomSampling(k, &nums);
    vector<int>::const_iterator head = nums.begin();
    vector<int> key(head, head+k);
    if (Counts.find(key)==Counts.end()){
        Counts[key] = 1;
    }
    else{
        Counts[key] += 1;
    }
}
for (map<vector<int>, int>::iterator it = Counts.begin(); it != Counts.end(); it++) {
    cout << "Key: ";
    for (vector<int>::const_iterator val_it = it->first.begin(); val_it != it->first.end(); val_it++) {
        cout << *val_it << " ";
    }
    cout << "Counts: " << it->second << endl;
}
```

### 5-12 Variant

* It's not correct. If **RAND\_MAX** can not evenly divided by n, **rand\(\) mod n** can not generate uniform distribution.

### 5-18 Compute the spiral ordering of a 2D array

* Mine Solution

```cpp
// n: The size of a square matrix, n by n
// Time Complexity: O(n^2)
// Space Complexity: O(1)

// Test Case:
//     1. square_matrix = {{1, 2}, {3, 4}} => {1, 2, 4, 3}
//     2. square_matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}} => {1, 2, 3, 6, 9, 8, 7, 4, 5}

vector<int> MatrixInSpiralOrderMine(vector<vector<int>>& square_matrix){
    int start = 0, end = static_cast<int>(square_matrix.size());
    vector<int> spiral_ordering;
    while (start < end) {
        for (int i = start, j = start; i < end - 1; i++){
            spiral_ordering.emplace_back(square_matrix[j][i]);
        }
        for (int i = end - 1, j = start; j < end - 1; j++){
            spiral_ordering.emplace_back(square_matrix[j][i])
        }
        for (int i = end - 1, j = end - 1; i > start; i--){
            spiral_ordering.emplace_back(square_matrix[j][i]);
        }
        for (int i = start, j = end - 1; j > start; j--){
            spiral_ordering.emplace_back(square_matrix[j][i]);
        }
        start++;
        end--;
    }
    // Handle the center case when the size of sqaure matrix is odd
    if (start - 1 == end) {
        spiral_ordering.emplace_back(square_matrix[start][start]);
    }
    cout << endl;
    return spiral_ordering;
}
```

* Author's Solution

```cpp
// n: The size of a square matrix, n by n
// Time Complexity: O(n^2)
// Space Complexity: O(1)

// Test Case:
//     1. square_matrix = {{1, 2}, {3, 4}} => {1, 2, 4, 3}
//     2. square_matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}} => {1, 2, 3, 6, 9, 8, 7, 4, 5}

vector<int> MatrixInSpiralOrderAuthor(vector<vector<int>> square_matrix){
    const array<array<int, 2>, 4> kShift = {{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}};
    int dir = 0, x = 0, y = 0;
    vector<int> spiral_ordering;
    for (int i = 0; i < square_matrix.size() * square_matrix.size(); i++) {
        spiral_ordering.emplace_back(square_matrix[x][y]);
        square_matrix[x][y] = 0;
        int next_x = x + kShift[dir][0], next_y = y + kShift[dir][1];
        if (next_x < 0 || next_x >= square_matrix.size() ||
            next_y < 0 || next_y >= square_matrix.size() ||
            square_matrix[next_x][next_y] == 0){
            dir = (dir + 1) % 4;
            next_x = x + kShift[dir][0];
            next_y = y + kShift[dir][1];
        }
        x = next_x;
        y = next_y;
    }
    return spiral_ordering;
}
```

### 5-18 Variant

* Given a dimension d, generate a d by d 2D matrix which in spiral order is \(1, 2, 3, ... d^2\).

```cpp
// d: The size of a square matrix, d by d
// Time Complexity: O(d^2)
// Space Complexity: O(1)

// Test Case:
//     1. d = 2 => {1, 2, 3, 4}
//     2. d = 3 => {1, 2, 3, 4, 5, 6, 7, 8, 9}

vector<vector<int>> GenerateSpiralOrderMatrix(int d){
    int dir = 0, x = 0, y = 0;
    const array<array<int, 2>, 4> kShift = {{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}};
    vector<vector<int>> spiral_matrix(d, vector<int>(d, 0));
    for (int val = 1; val <= d * d; val++){
        spiral_matrix[x][y] = val;
        int next_x = x + kShift[dir][0], next_y = y + kShift[dir][1];
        if (next_x < 0 || next_x >= d ||
            next_y < 0 || next_y >= d ||
            spiral_matrix[next_x][next_y] != 0){
            dir = (dir + 1) % 4;
            next_x = x + kShift[dir][0];
            next_y = y + kShift[dir][1];
        }
        x = next_x;
        y = next_y;
    }
    return spiral_matrix;
}
```

* Given a sequence, generate a 2D matrix which in spiral order is same as the sequence.

```cpp
// n: The size of the sequence
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. sequence = {1, 8, 3, 5} => {{1, 8}, {5, 3}}
//     2. sequence = {1, 2, 3, 4, 5, 6, 7, 8, 9} => {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}

vector<vector<int>> GenerateSpiralOrderMatrixFromSequence(vector<int>* sequence_ptr){
    vector<int>& sequence = *sequence_ptr;
    int dir = 0, x = 0, y = 0, d = floor(sqrt(sequence.size()));
    const array<array<int, 2>, 4> kShift = {{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}};
    vector<vector<int>> spiral_matrix(d, vector<int>(d, 0));
    for (int i = 0; i < sequence.size(); i++) {
        spiral_matrix[x][y] = sequence[i];
        int next_x = x + kShift[dir][0], next_y = y + kShift[dir][1];
        if (next_x < 0 || next_x >= d ||
            next_y < 0 || next_y >= d ||
            spiral_matrix[next_x][next_y] != 0){
            dir = (dir + 1) % 4;
            next_x = x + kShift[dir][0];
            next_y = y + kShift[dir][1];
        }
        x = next_x;
        y = next_y;
    }
    return spiral_matrix;
}
```



