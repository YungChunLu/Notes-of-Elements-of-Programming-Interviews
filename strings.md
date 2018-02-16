### Bootcamp

* Tips
  * Advanced string processing algorithms often use **hash tables** and **dynamic programming**.
* Know the array libraries

```markdown
1. Basic methods: append("12"), push_back('v'), pop_back(), insert(2, "12"), substr(pos, len), compare("12")
```

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



