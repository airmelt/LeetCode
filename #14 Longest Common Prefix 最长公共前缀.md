__Description__:
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

__Example__:
Example 1:
Input: ["flower","flow","flight"]
Output: "fl"

Example 2:
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

__Note__:
All given inputs are in lowercase letters a-z.

__题目描述__:
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

__示例__:
示例 1:
输入: ["flower","flow","flight"]
输出: "fl"

示例 2:
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。

__说明__:
所有输入只包含小写字母 a-z 。

__思路__:

1, 先遍历选出最短的词, 在最短的词中用二分法选择子串
2. [字典树/前缀树](https://www.cnblogs.com/dlutxm/archive/2011/10/26/2225660.html)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
struct Node {
    char c;
    int level;
    int child;
    unordered_map<char, Node*> map;
    Node(char c) {
        this -> c = c;
        level = 0;
        child = 0;
    }
    Node() {}
};

class PreTree {
    private:
        Node* head;
    public:
        PreTree() {
            head = new Node();
        }
        void insert(string str) {
            if (str.size() == 0) return;
            int size = str.size();
            // c_str()生成一个指针, 指向'\0'结尾的数组
            // data()生成的数组不包括'\0'
            char* c = (char*)str.data();
            Node* p = head;
            for (int i = 0; i < size; i++) {
                (p -> level)++;
                // end()返回一个指向对象容器结尾的迭代器
                if (p -> map.find(c[i]) == p -> map.end()) {
                    Node* new_node = new Node(c[i]);
                    p -> map[c[i]] = new_node;
                }
                // *iterator -> (key, value)
                // second即指向value
                p = p -> map.find(c[i]) -> second;
            }
            (p -> child)++;
        }
        bool findHelper(string str, int n) {
            if (str.size() == 0) return false;
            Node* p = head;
            int size = str.size();
            char* c = (char*)str.data();
            for (int i = 0; i < size; i++) {
                if (p -> map.find(c[i]) == p -> map.end()) {
                    return false;
                } else {
                    int len = p -> map.find(c[i]) -> second -> level + p -> map.find(c[i]) -> second -> child;
                    if (len != n) {
                        return false;
                    }
                }
                p = p -> map.find(c[i]) -> second;
            }
            return true;
        }
};
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0) return "";
        if (strs.size() == 1) return strs[0];
        int size = strs.size();
        string s = strs[0];
        int len = s.size();
        int length = 0;
        PreTree pre;
        for (int i = 0; i < size; i++) {
            pre.insert(strs[i]);
        }
        string help;
        for (int i = 0; i < len; i++) {
            help = s.substr(0, i + 1);
            if (pre.findHelper(help, size)) {
                length++;
            } else {
                break;
            }
        }
        return s.substr(0, length);
    }
};
```

__Java__:
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";
        if (strs.length == 1) return strs[0];
        int min = Integer.MAX_VALUE;
        for (String str : strs) min = Math.min(min, str.length());
        int low = 1;
        int high = min;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (isStartsWith(strs, mid)) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return strs[0].substring(0, (low + high) / 2);
    }

    private boolean isStartsWith(String[] strs, int mid) {
        String start = strs[0].substring(0, mid);
        for (int i = 0; i < strs.length; i++) {
            if (!strs[i].startsWith(start)) {
                return false;
            }
        }
        return true;
    }
}
```

__Python__:
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        # Python中字符串可以按ASCII码比较大小
        s1 = min(strs)
        s2 = max(strs)
        for i, s in enumerate(s1):
            if s != s2[i]:
                return s2[:i]
        return s1
```
