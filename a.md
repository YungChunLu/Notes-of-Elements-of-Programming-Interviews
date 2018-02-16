### BootCamp

* Tips
  * Take advantage of operating efficiently on both ends.
  * Array problems often have simple brute-force solutions that use O\(n\) space. But we sometimes can reduce it to O\(1\).
  * Try to write values from the back because of better efficiency.
  * Try to overwrite an entry instead of deleting it because of better efficiency.
  * Avoid making off-by-1 error - reading past the last element of an array. Otherwise, the consequence is catastrophic.
  * Don't worry about preserving the integrity of the array \(sortedness, keeping equal entries together, etc.\) until it's time to return.
* Know the array libraries

```markdown
1. Declare an array: array A = {1, 2, 3}
2. Declare a vector: vector<int> A = {1, 2, 3}
3. Construct a subarray from an array: vector(A.begin() + i, A.begin() + j)
4. Instantiate a 2D array: vector<vector<int>> A = {{1, 2}, {3, 4}, {5, 6}}
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

### 5-2 Increment An Arbitrary-Precision Integer

* Mine Solution

```cpp
// n: The number of digits in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {9} => {1, 0}
//     2. A = {0} => {1}
//     3. A = {1, 9} => {2, 0}
//     4. A = {9, 9, 9} => {1, 0, 0, 0}

vector<int> PlusOne(vector<int> A){
    int carry = 1, precision = (int)A.size() - 1;
    while (precision >= 0) {
        int new_val = A[precision] + carry;
        if (new_val > 9) {
            A[precision] = new_val - 10;
            precision--;
        }
        else {
            A[precision] = new_val;
            break;
        }
    }
    if (precision < 0){
        A.insert(A.begin(), 1);
    }
    return A;
}
```

### 5-2 Variant

* Mine Solution

```cpp
// n: The number of digits in the longer string
// Time Complexity: O(n)
// Space Complexity: O(n)

// Test Case:
//     1. Bs = "1", Bt = "1" => "10"
//     2. Bs = "11", Bt = "10" => "101"
//     3. Bs = "101", Bt = "110" => "1011"

string AddTwoStrings(string Bs, string Bt){
    // Reverse the two strings
    reverse(Bs.begin(), Bs.end());
    reverse(Bt.begin(), Bt.end());
    int k = 0, carry = 0;
    string ans = "";
    // Iterate through the common bits of two strings
    while ((k < Bs.size())&(k < Bt.size())) {
        int add = (Bs[k] - '0') + (Bt[k] - '0') + carry;
        ans.push_back('0' + (add % 2));
        carry = add / 2;
        k++;
    }
    // Iterate through the bits of the longer string
    string B = (Bs.size() > Bt.size()) ? Bs : Bt;
    while (k < B.size()){
        int add = (B[k] - '0') + carry;
        ans.push_back('0' + (add % 2));
        carry = add / 2;
        k++;
    }
    // Add back the carry bit
    if (carry > 0) {
        ans.push_back('1');
    }
    reverse(ans.begin(), ans.end());
    return ans;
}
```

### 5-5 Delete Duplicates From A Sorted Array

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {1,} => 1
//     2. A = {1, 2, 3} => 3
//     3. A = {1, 2, 3, 3, 4} => 4
//     4. A = {1, 2, 3, 3, 4, 5, 5, 6} => 6

int DeleteDuplicates(vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    if (A.size() == 0) {
        return 0;
    }
    int num_element = 0, current_val = A[0] - 1;
    for (int pos = 0; pos < A.size(); pos++) {
        if (A[pos] != current_val) {
            current_val = A[pos];
            A[num_element++] = A[pos];
        }
    }
    A = vector<int>(A.begin(), A.begin() + num_element);
    return num_element;
}
```

### 5-5 Variant

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {1,}, key = 1 => 0
//     2. A = {1, 2, 3}, key = 2 => 2
//     3. A = {1, 2, 3, 3, 4}, key = 3 => 3
//     4. A = {1, 2, 3, 3, 4, 5, 5, 6}, key = 3 => 6

int RemoveElement(vector<int>* A_ptr, int key) {
    vector<int>& A = *A_ptr;
    int num_element = 0;
    for (int pos = 0; pos < A.size(); pos++) {
        if (A[pos] != key) {
            A[num_element++] = A[pos];
        }
    }
    A = vector<int>(A.begin(), A.begin() + num_element);
    return num_element;
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

### 5-9 Enumerate All Primes To n

* Mine Solution

```cpp
// n: The value of input
// Time Complexity: O(n^3/2)
// Space Complexity: O(1)

// Test Cases:
//     1. n = 3 => {2, 3}
//     2. n = 6 => {2, 3, 5}
//     3. n = 1 => {}

bool IsPrime(int n) {
    if (n == 1) {
        return false;
    }
    for (int divisor = 2; divisor <= sqrt(n); divisor++) {
        if (n % divisor == 0) {
            return false;
        }
    }
    return true;
}

vector<int> GeneratePrimesMine(int n) {
    vector<int> primes;
    for (int val = 2; val <= n; val++) {
        if (IsPrime(val)) {
            primes.emplace_back(val);
        }
    }
    return primes;
}
```

* Author's Solution

```cpp
// n: The value of input
// Time Complexity: O(nloglogn)
// Space Complexity: O(n)

// Test Cases:
//     1. n = 3 => {2, 3}
//     2. n = 6 => {2, 3, 5}
//     3. n = 1 => {}

vector<int> GeneratePrimesAuthor(int n) {
    if (n < 2) {
        return {};
    }
    int size = floor(0.5 * (n - 3)) + 1;
    vector<int> primes = {2};
    deque<bool> is_prime = deque<bool>(size, true);
    for (int i = 0; i < size; i++) {
        long p = 2 * i + 3;
        if (is_prime[i]) {
            primes.emplace_back((int) p);
        }
        for (int j = 2 * i * i + 6 * i + 3; j < n; j += p){
            is_prime[j] = false;
        }
    }
    return primes;
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

### 5-17 The Sudoku Checker Problem

* Author's Solution

```cpp
// n: The dimension of a n*n Sodoku
// Time Complexity: O(n*2)
// Space Complexity: O(n)

bool HasDuplicate(const vector<vector<int>>& partial_assignment,
                  int start_row, int end_row, int start_col, int end_col){
    deque<bool> is_duplicate(partial_assignment.size()+1, false);
    for (int row = start_row; row < end_row; row++) {
        for (int col = start_col; col < end_col; col++) {
            int val = partial_assignment[row][col];
            if (val != 0 && is_duplicate[val]) {
                return true;
            }
            is_duplicate[val] = true;
        }
    }
    return false;
}

bool IsValidSudoku(const vector<vector<int>>& partial_assignment){
    int n = (int)partial_assignment.size();
    // Check each row
    for (int start_row = 0; start_row < n; start_row++) {
        if (HasDuplicate(partial_assignment, start_row, start_row+1, 0, n)) {
            return false;
        }
    }
    // Check each column
    for (int start_col = 0; start_col < n; start_col++) {
        if (HasDuplicate(partial_assignment, 0, n, start_col, start_col+1)) {
            return false;
        }
    }
    // Check each region
    int region_size = (int)sqrt(n);
    for (int start_row = 0; start_row < n; start_row += region_size) {
        for (int start_col = 0; start_col < n; start_col += region_size) {
            if (HasDuplicate(partial_assignment, start_row, start_row + region_size,
                             start_col, start_col + region_size)) {
                return false;
            }
        }
    }
    return true;
}
```

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

* Given a number n, generate first n pairs of integers in spiral order

```cpp
// n: The number of pairs
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. n = 5 => {{0, 0}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}}
//     2. n = 10 => {{0, 0}, {1, 0}, {1, -1}, {0, -1}, {-1, -1},
//                   {-1, 0}, {-1, 1}, {0, 1}, {1, 1}, {2, 1}}

vector<vector<int>> GenerateSpiralOrderPairs(int n){
    const array<array<int, 2>, 4> kShift = {{{1, 0}, {0, -1}, {-1, 0}, {0, 1}}};
    int dir = 0, x = 0, y = 0, boundary = 2;
    vector<vector<int>> pairs;
    for (int i = 0; i < n; i++) {
        pairs.emplace_back(vector<int> {x, y});
        int next_x = x + kShift[dir][0], next_y = y + kShift[dir][1];
        if (next_x >= boundary || next_x <= -boundary ||
            next_y >= boundary || next_y <= -boundary){
            if (dir == 3){
                boundary++;
            }
            dir = ++dir % 4;
            next_x = x + kShift[dir][0];
            next_y = y + kShift[dir][1];
        }
        x = next_x;
        y = next_y;
    }
    return pairs;
}
```

* Compute the spiral order for an m by n 2D matrix

```cpp
// m, n: The dimension of the rectangle m by n matrix
// Time Complexity: O(m*n)
// Space Complexity: O(1)

// Test Case:
//     1. rectangle_matrix = {{1, 2, 3}, {4, 5, 6}} => {1, 2, 3, 6, 5, 4}
//     2. rectangle_matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 12}} => {1, 2, 3, 6, 9, 12, 11, 10, 7, 4, 5, 8}

vector<int> RectangleMatrixInSpiralOrder(vector<vector<int>> rectangle_matrix){
    const array<array<int, 2>, 4> kShift = {{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}};
    int dir = 0, x = 0, y = 0;
    vector<int> spiral_ordering;
    for (int i = 0; i < rectangle_matrix[0].size() * rectangle_matrix.size(); i++) {
        spiral_ordering.emplace_back(rectangle_matrix[x][y]);
        rectangle_matrix[x][y] = 0;
        int next_x = x + kShift[dir][0], next_y = y + kShift[dir][1];
        if (next_x < 0 || next_x >= rectangle_matrix.size() ||
            next_y < 0 || next_y >= rectangle_matrix[0].size() ||
            rectangle_matrix[next_x][next_y] == 0){
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

* Compute the last element in spiral order for an m by n 2D matrix

```cpp
// m, n: The dimension of the rectangle m by n matrix
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//     1. rectangle_matrix = {{1, 2, 3}, {4, 5, 6}} => 4
//     2. rectangle_matrix = {{1, 2}, {3, 4}} => 3
//     3. rectangle_matrix = {{1, 2}, {3, 4}, {5, 6}} => 3
//     4. rectangle_matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 12}} => 8
//     5. rectangle_matrix = {{1, 2, 3, 4}, {5, 6, 7, 8}, {9, 10, 11, 12}} => 7

int LastElementInSpiralOrder(vector<vector<int>> rectangle_matrix){
    int m = (int)rectangle_matrix.size(), n = (int)rectangle_matrix[0].size();
    int x = floor(m * 0.5), y = floor(n * 0.5);
    if (m < n){
        if (m % 2 == 1){
            y = n - x - 1;
        }
        else{
            y = x - 1;
        }
    }
    else if (m == n && m % 2 == 0){
        y = x - 1;
    }
    else if (m > n){
        if (n % 2 == 0){
            x = y--;
        }
        else{
            x = m - y - 1;
        }
    }
    return rectangle_matrix[x][y];
}
```

* Compute the kth element in spiral order for an m by n 2D matrix

```cpp
// m, n: The dimension of the rectangle m by n matrix
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//     1. k = 4, rectangle_matrix = {{1, 2, 3}, {4, 5, 6}}=> 6
//     2. k = 2, rectangle_matrix = {{1, 2}, {3, 4}} => 2
//     3. k = 5, rectangle_matrix = {{1, 2}, {3, 4}, {5, 6}} => 5
//     4. k = 11, rectangle_matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 12}} => 5

int KthElementInSpiralOrder(int k, vector<vector<int>> rectangle_matrix){
    int x = 0, y = 0, m = (int)rectangle_matrix.size(), n = (int)rectangle_matrix[0].size();
    while (k > 2 * (m + n - 2)) {
        k -= 2 * (m + n - 2);
        m -= 2;
        n -= 2;
        x++;
        y++;
    }
    if (k <= n) {
        y += k - 1;
    }
    else if (k <= m + n - 1){
        y += n - 1;
        x += k - n;
    }
    else if (k <= m + 2 * n - 2){
        y += m + 2 * n - 2 - k;
        x += m - 1;
    }
    else{
        x += 2 * m + 2 * n - 3 - k;
    }
    return rectangle_matrix[x][y];
}
```



