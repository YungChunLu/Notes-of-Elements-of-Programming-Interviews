### Boot Camp

* Tips
  * Take advantage of operating efficiently on both ends
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



