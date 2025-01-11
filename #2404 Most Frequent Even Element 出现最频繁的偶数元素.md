# 2404 Most Frequent Even Element 出现最频繁的偶数元素

__Description:__

Given an integer array `nums`, return _the most frequent even element_.

If there is a tie, return the __smallest__ one. If there is no such element, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [0,1,2,2,4,4,1]
Output: 2
Explanation:
The even elements are 0, 2, and 4. Of these, 2 and 4 appear the most.
We return the smallest one, which is 2.
```

Example 2:

```text
Input: nums = [4,4,4,9,2,4]
Output: 4
Explanation: 4 is the even element appears the most.
```

Example 3:

```text
Input: nums = [29,47,21,41,13,37,25,7]
Output: -1
Explanation: There is no even element.
```

__Constraints:__

- `1 <= nums.length <= 2000`
- `0 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` ，返回出现最频繁的偶数元素。

如果存在多个满足条件的元素，只需要返回 __最小__ 的一个。如果不存在这样的元素，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [0,1,2,2,4,4,1]
输出：2
解释：
数组中的偶数元素为 0、2 和 4 ，在这些元素中，2 和 4 出现次数最多。
返回最小的那个，即返回 2 。
```

示例 2：

```text
输入：nums = [4,4,4,9,2,4]
输出：4
解释：4 是出现最频繁的偶数元素。
```

示例 3：

```text
输入：nums = [29,47,21,41,13,37,25,7]
输出：-1
解释：不存在偶数元素。
```

__提示：__

- `1 <= nums.length <= 2000`
- `0 <= nums[i] <= 10 ^ 5`

__思路:__

```text
哈希表
遍历数组
将偶数放入哈希表
同时比较出现次数
更新最小的偶数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int mostFrequentEven(vector<int>& nums) 
    {
        int result = -1, c = 0;
        unordered_map<int, int> cnt;
        for (const auto& num: nums) 
        {
            if (!(num & 1))
            {
                c = ++cnt[num];
                if (c > cnt[result] or (c == cnt[result] and num < result)) result = num;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int mostFrequentEven(int[] nums) {
        int result = -1, c = 0;
        var count = new HashMap<Integer, Integer>();
        for (int num : nums) {
            if ((num & 1) == 0) {
                c = count.merge(num, 1, Integer::sum);
                if (result < 0 || c > count.get(result) || (c == count.get(result) && num < result)) result = num;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostFrequentEven(self, nums: List[int]) -> int: 
        return c.most_common()[0][0] if (c := Counter(i for i in sorted(nums) if not i & 1)) else -1
```
