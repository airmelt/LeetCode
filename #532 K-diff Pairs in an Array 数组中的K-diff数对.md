# 532 K-diff Pairs in an Array 数组中的K-diff数对

__Description__:
Given an array of integers and an integer k, you need to find the number of unique k-diff pairs in the array. Here a k-diff pair is defined as an integer pair (i, j), where i and j are both numbers in the array and their absolute difference is k.

__Example:__

Example 1:
Input: [3, 1, 4, 1, 5], k = 2
Output: 2
Explanation: There are two 2-diff pairs in the array, (1, 3) and (3, 5).
Although we have two 1s in the input, we should only return the number of unique pairs.

Example 2:
Input:[1, 2, 3, 4, 5], k = 1
Output: 4
Explanation: There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).

Example 3:
Input: [1, 3, 1, 5, 4], k = 0
Output: 1
Explanation: There is one 0-diff pair in the array, (1, 1).

__Note:__
The pairs (i, j) and (j, i) count as the same pair.
The length of the array won't exceed 10,000.
All the integers in the given input belong to the range: [-1e7, 1e7].

__题目描述__:
给定一个整数数组和一个整数 k, 你需要在数组里找到不同的 k-diff 数对。这里将 k-diff 数对定义为一个整数对 (i, j), 其中 i 和 j 都是数组中的数字，且两数之差的绝对值是 k.

__示例 :__

示例 1:

输入: [3, 1, 4, 1, 5], k = 2
输出: 2
解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。

示例 2:

输入:[1, 2, 3, 4, 5], k = 1
输出: 4
解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。

示例 3:

输入: [1, 3, 1, 5, 4], k = 0
输出: 1
解释: 数组中只有一个 0-diff 数对，(1, 1)。

__注意:__
数对 (i, j) 和数对 (j, i) 被算作同一数对。
数组的长度不超过10,000。
所有输入的整数的范围在 [-1e7, 1e7]。

__思路__:

k < 0直接输出 0
用一个 map计数, 保存数字和出现的次数
当 k = 0时, 出现 2次以上的数字才被算做一次数组
否则出现 1次就可以算作一个数组
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findPairs(vector<int>& nums, int k) 
    {
        if (k < 0) return 0;
        map<int, int> m;
        for (int num : nums) m[num]++;
        int result = 0;
        for (auto p : m) {
            if (!k) if (p.second > 1) result++;
            else if (m.find(p.first + k) != m.end()) ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findPairs(int[] nums, int k) {
        if (k < 0) return 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
        int result = 0;
        for (Integer key : map.keySet()) {
            if (k == 0) {
                if (map.get(key) > 1) result++;
            }
            else if (map.getOrDefault(key + k, 0) != 0) result++;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findPairs(self, nums: List[int], k: int) -> int:
        result = 0
        if k < 0:
            return result
        elif not k:
            count = collections.Counter(nums)
            for i, v in count.items():
                if v > 1:
                    result += 1
            return result
        else:
            nums = set(nums)
            for num in nums:
                if num + k in nums:
                    result += 1
            return result
```
