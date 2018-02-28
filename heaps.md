### Bootcamp

* Tips

  * A heap is a specialized binary tree which is a complete binary tree.

  * The important property of a **max-heap** is that the key at each node is at least as great as the keys stored at its children. A **min-heap **has completely symmetric property.

  * A max-heap supports $$O(\log{n})$$ insertion, $$O(1)$$ lookup for max element, and $$O(\log{n})$$ deletion of the max element.

  * Use heap when only caring about the max- or min-element

  * A heap is good choice when computing the k **largest** or k **smallest **elements in a collection.

* Find the k longest string

```cpp
// n: The number of strings
// Time Complexity: O(nlogk)
// Space Complexity: O(1)

vector<string> TopK(int k, vector<string>::const_iterator stream_begin,
                    const vector<string>::const_iterator stream_end) {
    priority_queue<string, vector<string>, function<bool(string, string)>>\
        min_heap([](const string& a, const string& b){
            return a.size() >= b.size();
        });
    while (stream_begin != stream_end) {
        min_heap.emplace(*stream_begin);
        if (min_heap.size() > k) {
            // Remove the shortest string
            min_heap.pop();
        }
    }
    vector<string> result;
    while (!min_heap.empty()) {
        result.emplace_back(min_heap.top());
        min_heap.pop();
    }
    return result;
}
```



