### Bootcamp

* Tips
  * Advanced string processing algorithms often use **hash tables** and **dynamic programming**.
* Determine whether a string is palindromic

```cpp
// n: The size of input string
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Cases:
//     1. s = "a" => true
//     2. s = "abba" => true
//     3. s = "abcd" => false

bool IsPalindromic(const string& s){
    for (int head = 0, tail = (int)s.size()-1; head < tail; head++, tail--) {
        if (s[head] != s[tail]) {
            return false;
        }
    }
    return true;
}
```

### 6-1



