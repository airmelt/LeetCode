# 1207 Unique Number of Occurrences 独一无二的出现次数

__Description__:
Given an array of integers arr, write a function that returns true if and only if the number of occurrences of each value in the array is unique.

__Example:__

Example 1:

Input: arr = [1,2,2,1,1,3]
Output: true
Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

Example 2:

Input: arr = [1,2]
Output: false

Example 3:

Input: arr = [-3,0,1,-3,1,1,1,-3,10,0]
Output: true

__Constraints:__

1 <= arr.length <= 1000
-1000 <= arr[i] <= 1000

__题目描述__:
给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。

如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。

__示例 :__

示例 1：

输入：arr = [1,2,2,1,1,3]
输出：true
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。

示例 2：

输入：arr = [1,2]
输出：false

示例 3：

输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]
输出：true

__提示：__

1 <= arr.length <= 1000
-1000 <= arr[i] <= 1000

__思路__:

遍历数组, 将数组中的元素和个数存在一个 map中, 遍历这个 map的值, 存在一个 set中, 比较 map和 set的大小即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool uniqueOccurrences(vector<int>& arr) 
    {
        unordered_map<int, int> count;
        for (auto item : arr) ++count[item];
        unordered_set<int> s;
        for (auto p : count) s.insert(p.second);
        return count.size() == s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int item : arr) map.put(item, map.getOrDefault(item, 0) + 1);
        return map.size() == new HashSet<Integer>(map.values()).size();
    }
}
```

__Python__:

```Python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        return len(collections.Counter(arr)) == len(dict(zip(collections.Counter(arr).values(), collections.Counter(arr).keys())))
```
