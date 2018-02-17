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

### 6-2 Base Conversion

* Mine Solution

```cpp
// n: The number of digits in the string
// Time Complexity: O(n(1 + log_b2(b1)))
// Space Complexity: O(1)

// Test Cases:
//     1. num_as_string = "1011", b1 = 2, b2 = 16 => "B"
//     2. num_as_string = "1011", b1 = 2, b2 = 9 => "12"
//     3. num_as_string = "0", b1 = 2, b2 = 9 => "0"
//     4. num_as_string = "-1011", b1 = 2, b2 = 9 => "-12"

string ConvertBaseMine(const string& num_as_string, int b1, int b2) {
    bool is_negative = num_as_string[0] == '-';
    string val_to_char = "0123456789ABCDEF";
    // Convert base b1 to base 10
    int x = 0;
    for (int i = is_negative ? 1 : 0; i < num_as_string.size(); i++) {
        x = x * b1 + (int)val_to_char.find(num_as_string[i]);
    }
    // Convert base 10 to base b2
    string s = "";
    while (x) {
        s += val_to_char[x % b2];
        x /= b2;
    }
    return (is_negative ? "-" : "") + (s.size() == 0 ? "0" : string({s.rbegin(), s.rend()}));
}
```

* Author's Solution

```cpp
// n: The number of digits in the string
// Time Complexity: O(n(1 + log_b2(b1)))
// Space Complexity: O(1)

// Test Cases:
//     1. num_as_string = "1011", b1 = 2, b2 = 16 => "B"
//     2. num_as_string = "1011", b1 = 2, b2 = 9 => "12"
//     3. num_as_string = "0", b1 = 2, b2 = 9 => "0"
//     4. num_as_string = "-1011", b1 = 2, b2 = 9 => "-12"

string ConstructFromBase(int num_as_int, int b2) {
    int remainder = num_as_int % b2;
    return num_as_int == 0 ? "" :
        ConstructFromBase(num_as_int / b2, b2) + (char)(remainder >= 10 ? 'A' + remainder - 10 : '0' + remainder);
}

string ConvertBaseAuthor(const string& num_as_string, int b1, int b2) {
    bool is_negative = num_as_string.front() == '-';
    int num_as_int = 0;
    for (int i = is_negative ? 1 : 0; i < num_as_string.size(); i++) {
        num_as_int *= b1;
        num_as_int += (num_as_string[i] - '0' > 9) ? num_as_string[i] - 'A' + 10: num_as_string[i] - '0';
    }
    return (is_negative ? "-" : "") + (num_as_string == "0" ? "0" : ConstructFromBase(num_as_int, b2));
}
```

### 6-4 Replace And Remove

* Mine Solution

```cpp
// n: The number of char in the array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Cases:
//     1. size = 3, s = "abc" => 3
//     2. size = 2, s = "ab" => 3
//     3. size = 4, s = "aabb" => 4

int ReplaceAndRemove(int size, char s[]) {
    // Remove elements
    int remove_pos = size;
    for (int i = remove_pos - 1; i >= 0; i--) {
        if (s[i] != 'b') {
            s[--remove_pos] = s[i];
        }
    }
    // Add duplicates
    int add_pos = 0;
    for (int i = remove_pos; i < size; i++) {
        if (s[i] == 'a') {
            s[add_pos++] = 'd';
            s[add_pos++] = 'd';
        }
        else {
            s[add_pos++] = s[i];
        }
    }
    return add_pos;
}
```



