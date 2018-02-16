### Bootcamp

* Tips
  * Advanced string processing algorithms often use **hash tables** and **dynamic programming**.
* Know the array libraries

```markdown
1. Basic methods: append("12"), push_back('v'), pop_back(), insert(2, "12"), substr(pos, len), compare("12")
2. Updating a mutable string from the front is slow
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

### 6-1 Interconvert Strings And Integers

* Mine Solution

```cpp
// n: The number of digits 
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Cases:
//     1. x = 123 => "123"
//     2. x = -123 => "-123"
//     3. x = 0 => "0"
//     4. s = "123" => 123
//     5. s = "-123" => -123
//     6. s = "0" => 0

string IntToString(int x) {
    bool is_negative = x < 0;
    if (is_negative) {
        x *= -1;
    }
    string s = "";
    do {
        char c = '0' + (x % 10);
        s.push_back(c);
        x /= 10;
    } while (x);
    if (is_negative) {
        s.push_back('-');
    }
    return {s.rbegin(), s.rend()};
}

int StringToInt(const string& s) {
    bool is_negative = s[0] == '-';
    int x = 0;
    for (int i = is_negative ? 1 : 0; i < s.size(); i++) {
        x = x * 10 + (s[i] - '0');
    }
    return is_negative ? -1 * x : x;
}
```



