### Bootcamp

* Tips
  * When no collision occurs, inserts, deletes and lookups run in **O\(1\)**
  * When a collision occur, inserts, deletes and lookups run in **O\(1+n/m\)** when n is the number of elements, m is the number of buckets
  * Rehashing is expensive, **O\(n+m\)**
  * One disadvantage of hash tables is the need for a good hash function but this is rarely an issue in practice.
  * Hard requirement - equal keys should have equal hash codes.
  * Soft requirement - generating uniformly keys
  * Hash codes are often cached for performance
  * [Why needing equal function for hashing?](https://stackoverflow.com/questions/39107488/what-is-keyequal-in-stdunordered-set-for)
  * Hash tables have the **best theoretical and real-world performance** for lookup, insert and delete.
  * Consider using a hash code as a **signature** to enhance performance.
* Know the hash table libraries

```markdown
1. Two common hash table-based data structures: unorded_set and unorded_map
2. For unorded_set, following are the important functions:
    * insert(val): returns a pair of iterator and boolean where the iterator points to 
    the newly inserted element or the element whose key is equivalent, and the boolean
    indicating if the element was added successfully.
    * find(k): returns the iterator to the element if it was present;
    otherwise, a special iterator end() is returned.
3. The order in which keys are traversed by the iterator returned by **begin()** is unspecified;
it may even change with time.
```

* Find anagram from a collection of strings. Note: the insertion is O\(m\) because it copy a string

```cpp
// n: The number of strings, m: The max length of a string
// Time Complexity: [sort string] O(nmlogm) + [insertion] O(nm) => O(nmlogm)
// Space Complexity: O(n)

vector<vector<string>> FindAnagrams_A(const vector<string>& dictionary) {
    unordered_map<string, vector<string>> sorted_string_to_anagrams;
    for (const string& s: dictionary) {
        string sorted_str(s);
        // Use sorted string a key
        sort(sorted_str.begin(), sorted_str.end());
        sorted_string_to_anagrams[sorted_str].emplace_back(s);
    }
    vector<vector<string>> anagram_groups;
    for (const auto& p : sorted_string_to_anagrams) {
        if (p.second.size() >= 2) {
            anagram_groups.emplace_back(p.second);
        }
    }
    return anagram_groups;
}
```

* Find anagram from a collection of lower case English characters

```cpp
// n: The number of strings, m: The max length of a string
// Time Complexity: [hashing] O(nm) + [insertion] O(nm) => O(nm)
// Space Complexity: O(n)

int StringHash(const string& s, int modulus) {
    int val = 0;
    for (char c : s) {
        val += c % modulus;
    }
    return val;
}

vector<vector<string>> FindAnagrams_M(const vector<string>& dictionary) {
    unordered_map<int, vector<string>> sorted_string_to_anagrams;
    for (const string& s: dictionary) {
        // Use sorted string a key
        int hash_key = StringHash(s, 10000);
        sorted_string_to_anagrams[hash_key].emplace_back(s);
    }
    vector<vector<string>> anagram_groups;
    for (const auto& p : sorted_string_to_anagrams) {
        if (p.second.size() >= 2) {
            anagram_groups.emplace_back(p.second);
        }
    }
    return anagram_groups;
}
```

* Merge duplicated contacts

```cpp
// n: The number of strings
// Time Complexity: O(n)
// Space Complexity: O(1)

struct ContactList {
    // Equal function for hash.
    bool operator==(const ContactList& that) const {
        return unordered_set<string>(names.begin(), names.end()) == unordered_set<string>(that.names.begin(), that.names.end());
    }
    vector<string> names;
};

// Hash function for ContactList
struct HashContactList {
    size_t operator()(const ContactList& contacts) const {
        size_t hash_code = 0;
        for (const string& name : unordered_set<string>(contacts.names.begin(), contacts.names.end())) {
            hash_code ^= hash<string>()(name);
        }
        return hash_code;
    }
};

vector<ContactList> MergeContactLists(const vector<ContactList>& contacts) {
    unordered_set<ContactList, HashContactList> unique_contacts(contacts.begin(), contacts.end());
    return {unique_contacts.begin(), unique_contacts.end()};
}
```

### 12-2 Is An Anonymous Letter Constructible

* Author's Solution

```cpp
// n: The number of chars in letter text, m: The number of chars in magazine text
// Time Complexity: O(m+n)
// L: The number of distinct chars in letter text
// Space Complexity: O(L)

bool IsLetterConstructibleFromMagazine(const string& letter_text, const string& magazine_text) {
    unordered_map<char, int> char_freq_for_letter_text;
    // Compute the frequency for all char in lettter_text
    for (char c : letter_text) {
        char_freq_for_letter_text[c]++;
    }
    // Check if char in magazine_text can be found in letter_text
    for (char c : magazine_text) {
        if (char_freq_for_letter_text.empty()) {
            break;
        }
        auto it = char_freq_for_letter_text.find(c);
        if (it != char_freq_for_letter_text.cend()) {
            --it->second;
            if (it->second == 0) {
                char_freq_for_letter_text.erase(it);
            }
        }
    }
    return char_freq_for_letter_text.empty();
}
```

### 12-3 Implement An ISBN Cache

```cpp
// n: The value of capacity
// Time Complexity: O(1) for lookup hash table, O(1) for update the queue
// Space Complexity: O(1)

class LRUCache {
public:
    LRUCache(size_t capacity) {}
    explicit LRUCache(int capacity): capacity_(capacity) {}

    int Lookup(int isbn) {
        auto it = isbn_price_table_.find(isbn);
        if (it == isbn_price_table_.end()) {
            return -1;
        }
        int price = it->second.second;
        MoveToFront(isbn, it);
        return price;
    }

    void Insert(int isbn, int price) {
        auto it = isbn_price_table_.find(isbn);
        if (it == isbn_price_table_.end()) {
            MoveToFront(isbn, it);
        } else {
            if (isbn_price_table_.size() == capacity_) {
                isbn_price_table_.erase(lru_queue_.back());
                lru_queue_.pop_back();
            }
            lru_queue_.emplace_front(isbn);
            isbn_price_table_[isbn] = {lru_queue_.begin(), price};
        }
    }

    bool Erase(int isbn) {
        auto it = isbn_price_table_.find(isbn);
        if (it == isbn_price_table_.end()) {
            return false;
        }
        lru_queue_.erase(it->second.first);
        isbn_price_table_.erase(it);
        return true;
    }

private:
    typedef unordered_map<int, pair<list<int>::iterator, int>> Table;

    // Forces this key-value pair to move to the front
    void MoveToFront(int isbn, const Table::iterator& it) {
        lru_queue_.erase(it->second.first);
        lru_queue_.emplace_front(isbn);
        it->second.first = lru_queue_.begin();
    }

    int capacity_;
    Table isbn_price_table_;
    list<int> lru_queue_;
};
```

### 12-5 Find The Nearest Repeated Entries In An Array

```cpp
// n: The number of strings in a vector, d: The number of unique strings
// Time Complexity: O(n)
// Space Complexity: O(d)

int FindNearestRepetition(const vector<string>& paragraph) {
    unordered_map<string, int> word_index;
    int distance = numeric_limits<int>::max();
    for (int index = 0; index < paragraph.size(); index++) {
        auto it = word_index.find(paragraph[index]);
        if (it != word_index.end()) {
            distance = min(distance, index - it->second);
        }
        word_index[paragraph[index]] = index;
    }
    return distance == numeric_limits<int>::max() ? -1 : distance;
}
```



