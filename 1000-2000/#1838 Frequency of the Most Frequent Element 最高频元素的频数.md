# 1838 Frequency of the Most Frequent Element 最高频元素的频数

__Description:__

The frequency of an element is the number of times it occurs in an array.

You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.

Return _the __maximum possible frequency__ of an element after performing __at most___ `k` _operations_.

__Example:__

Example 1:

```text
Input: nums = [1,2,4], k = 5
Output: 3
Explanation: Increment the first element three times and the second element two times to make nums = [4,4,4].
4 has a frequency of 3.
```

Example 2:

```text
Input: nums = [1,4,8,13], k = 5
Output: 2
Explanation: There are multiple optimal solutions:
- Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.
- Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2.
- Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.
```

Example 3:

```text
Input: nums = [3,9,6], k = 2
Output: 1
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 5`

__题目描述:__

元素的 频数 是该元素在一个数组中出现的次数。

给你一个整数数组 `nums` 和一个整数 `k` 。在一步操作中，你可以选择 `nums` 的一个下标，并将该下标对应元素的值增加 `1` 。

执行最多 `k` 次操作后，返回数组中最高频元素的 __最大可能频数__ _。_

__示例:__

示例 1：

```text
输入：nums = [1,2,4], k = 5
输出：3
解释：对第一个元素执行 3 次递增操作，对第二个元素执 2 次递增操作，此时 nums = [4,4,4] 。
4 是数组中最高频元素，频数是 3 。
```

示例 2：

```text
输入：nums = [1,4,8,13], k = 5
输出：2
解释：存在多种最优解决方案：
- 对第一个元素执行 3 次递增操作，此时 nums = [4,4,8,13] 。4 是数组中最高频元素，频数是 2 。
- 对第二个元素执行 4 次递增操作，此时 nums = [1,8,8,13] 。8 是数组中最高频元素，频数是 2 。
- 对第三个元素执行 5 次递增操作，此时 nums = [1,4,13,13] 。13 是数组中最高频元素，频数是 2 。
```

示例 3：

```text
输入：nums = [3,9,6], k = 2
输出：1
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 5`

__思路:__

```text
滑动窗口
将数组排序
最高频数的元素一定是已经存在的元素
优先将小元素增加到一个最近的大元素
如果操作数少于 k, 则用该区间长度更新结果
否则窗口向右移动
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxFrequency(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int s = 0, j = 0, result = 1, n = nums.size();
        for (int i = 1; i < n; i++) {
            s += (long long)(i - j) * (nums[i] - nums[i - 1]);
            while (s > k) s -= nums[i] - nums[j++];
            result = max(result, i - j + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxFrequency(int[] nums, int k) {
        Arrays.sort(nums);
        int s = 0, j = 0, result = 1, n = nums.length;
        for (int i = 1; i < n; i++) {
            s += (i - j) * (nums[i] - nums[i - 1]);
            while (s > k) s -= nums[i] - nums[j++];
            result = Math.max(result, i - j + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxFrequency(self, nums: List[int], k: int) -> int:
        nums.sort()
        s, j, result, n = 0, 0, 1, len(nums)
        for i in range(1, n):
            s += (i - j) * (nums[i] - nums[i - 1])
            while s > k:
                s -= nums[i] - nums[j]
                j += 1
            result = max(result, i - j + 1)
        return result
```
