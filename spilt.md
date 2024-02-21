C++ Split 函数

```C++
auto split = [](const string& s, char delim) -> vector<string> {
    vector<string> ans;
    string cur;
    for (char ch: s) {
        if (ch == delim) {
            ans.push_back(move(cur));
            cur.clear();
        }
        else {
            cur += ch;
        }
    }
    ans.push_back(move(cur));
    return ans;
};
```
