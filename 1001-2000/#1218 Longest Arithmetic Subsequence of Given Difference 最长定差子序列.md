# 1218 Longest Arithmetic Subsequence of Given Difference 最长定差子序列

__Description:__

Given an integer array arr and an integer difference, return the length of the longest subsequence in arr which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals difference.

A subsequence is a sequence that can be derived from arr by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

Input: arr = [1,2,3,4], difference = 1
Output: 4
Explanation: The longest arithmetic subsequence is [1,2,3,4].

Example 2:

Input: arr = [1,3,5,7], difference = 1
Output: 1
Explanation: The longest arithmetic subsequence is any single element.

Example 3:

Input: arr = [1,5,7,8,5,3,4,2,1], difference = -2
Output: 4
Explanation: The longest arithmetic subsequence is [7,5,3,1].

__Constraints:__

1 <= arr.length <= 10^5
-10^4 <= arr[i], difference <= 10^4

__题目描述：__

给你一个整数数组 arr 和一个整数 difference，请你找出并返回 arr 中最长等差子序列的长度，该子序列中相邻元素之间的差等于 difference 。

子序列 是指在不改变其余元素顺序的情况下，通过删除一些元素或不删除任何元素而从 arr 派生出来的序列。

__示例：__

示例 1：

输入：arr = [1,2,3,4], difference = 1
输出：4
解释：最长的等差子序列是 [1,2,3,4]。

示例 2：

输入：arr = [1,3,5,7], difference = 1
输出：1
解释：最长的等差子序列是任意单个元素。

示例 3：

输入：arr = [1,5,7,8,5,3,4,2,1], difference = -2
输出：4
解释：最长的等差子序列是 [7,5,3,1]。

__提示：__

1 <= arr.length <= 10^5
-10^4 <= arr[i], difference <= 10^4

__思路：__

动态规划
设 dp[i] 表示以 i 结尾的最长子序列
dp[i] = dp[i - difference] + 1
返回 max(dp)
dp 可以用哈希表加快查找
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int longestSubsequence(vector<int>& arr, int difference) 
    {
        map<int, int> m;
        for (const auto& num : arr) m[num] = m[num - difference] + 1;
        return max_element(m.begin(), m.end(), [] (const pair<int, int>& a, const pair<int, int>& b) -> bool { return a.second < b.second; } ) -> second;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubsequence(int[] arr, int difference) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : arr) map.put(num, map.getOrDefault(num - difference, 0) + 1);
        int result = 1;
        for (int value : map.values()) result = Math.max(result, value);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        d = defaultdict(int)
        for num in arr:
            d[num] = d[num - difference] + 1
        return max(d.values())
```
