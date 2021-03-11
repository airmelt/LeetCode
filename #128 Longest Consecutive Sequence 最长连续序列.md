# 128 Longest Consecutive Sequence 最长连续序列

__Description__:
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(n) complexity.

__Example:__

Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

__题目描述__:
给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 O(n)。

__示例 :__

输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。

__思路__:

要确定一个数是否存在, 需要用 set, 可以将查询时间将为 O(1)
如果一个连续子序列存在, 假定从 x开始, 长度为 result
则 x, x + 1, x + 2, ... , x + result - 1均在 set中,
第一个不在 set中的元素为 x - 1
所以从第一个 x - 1不在 set中的元素开始向后查找
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestConsecutive(vector<int>& nums) 
    {
        int result = 0;
        unordered_set<int> s(nums.begin(), nums.end());
        for (auto num : s)
        {
            if (s.find(num - 1) == s.end())
            {
                int cur = num, temp = 1;
                while (s.find(++cur) != s.end()) ++temp;
                result = max(result, temp);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestConsecutive(int[] nums) {
        int result = 0;
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int cur = num, temp = 1;
                while (set.contains(++cur)) ++temp;
                result = result < temp ? temp : result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        d, result = defaultdict(int), 0
        for num in nums:
            if not d[num]:
                result = max(cur := (left := d[num - 1]) + (right := d[num + 1]) + 1, result)
                d[num] = d[num - left] = d[num + right] = cur
        return result
```
