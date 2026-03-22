# 2593 Find Score of an Array After Marking All Elements 标记所有元素后数组的分数

__Description:__

You are given an array `nums` consisting of positive integers.

Starting with `score = 0`, apply the following algorithm:

- Choose the smallest integer of the array that is not marked. If there is a tie, choose the one with the smallest index.
- Add the value of the chosen integer to `score`.
- Mark __the chosen element and its two adjacent elements if they exist__.
- Repeat until all the array elements are marked.

Return the score you get after applying the above algorithm.

__Example:__

Example 1:

```text
Input: nums = [2,1,3,4,5,2]
Output: 7
Explanation: We mark the elements as follows:
```

- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,1,3,4,5,2].
- 2 is the smallest unmarked element, so we mark it and its left adjacent element: [2,1,3,4,5,2].
- 4 is the only remaining unmarked element, so we mark it: [2,1,3,4,5,2].

Our score is 1 + 2 + 4 = 7.

Example 2:

```text
Input: nums = [2,3,5,1,3,2]
Output: 5
Explanation: We mark the elements as follows:
```

- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,3,5,1,3,2].
- 2 is the smallest unmarked element, since there are two of them, we choose the left-most one, so we mark the one at index 0 and its right adjacent element: [2,3,5,1,3,2].
- 2 is the only remaining unmarked element, so we mark it: [2,3,5,1,3,2].

Our score is 1 + 2 + 2 = 5.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个数组 `nums` ，它包含若干正整数。

一开始分数 `score = 0` ，请你按照下面算法求出最后分数:

- 从数组中选择最小且没有被标记的整数。如果有相等元素，选择下标最小的一个。
- 将选中的整数加到 `score` 中。
- 标记 __被选中元素__，如果有相邻元素，则同时标记 __与它相邻的两个元素__ 。
- 重复此过程直到数组中所有元素都被标记。

请你返回执行上述算法后最后的分数。

__示例:__

示例 1：

```text
输入：nums = [2,1,3,4,5,2]
输出：7
解释：我们按照如下步骤标记元素：
```

- 1 是最小未标记元素，所以标记它和相邻两个元素：[2,1,3,4,5,2] 。
- 2 是最小未标记元素，所以标记它和左边相邻元素：[2,1,3,4,5,2] 。
- 4 是仅剩唯一未标记的元素，所以我们标记它：[2,1,3,4,5,2] 。

总得分为 1 + 2 + 4 = 7 。

示例 2：

```text
输入：nums = [2,3,5,1,3,2]
输出：5
解释：我们按照如下步骤标记元素：
```

- 1 是最小未标记元素，所以标记它和相邻两个元素：[2,3,5,1,3,2] 。
- 2 是最小未标记元素，由于有两个 2 ，我们选择最左边的一个 2 ，也就是下标为 0 处的 2 ，以及它右边相邻的元素：[2,3,5,1,3,2] 。
- 2 是仅剩唯一未标记的元素，所以我们标记它：[2,3,5,1,3,2] 。

总得分为 1 + 2 + 2 = 5 。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
1. 排序
按照下标进行排序
如果未标记, 则标记两侧的元素, 并累加值
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 贪心
按照严格单调递减分组
[2, 1, 3, 4, 5, 2] 可分为
[2, 1] [3] [4] [5, 2]
从左至右遍历数组
每次遇到一个新分组, 累加当前分组的最小值
这个最小值一定比前一个数小, 也不会大于后一个分组的第一个数
当选择了这个数 nums[i], 那么由于分组都是严格递减的
nums[i - 2], nums[i - 4], ... 都是严格大于 nums[i] 的
都可以被标记
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long findScore(vector<int>& nums) 
    {
        long long result = 0LL;
        for (int i = 0, n = nums.size(), j = i; i < n; i += 2, j = i)
        {
            while (i + 1 < n and nums[i] > nums[i + 1]) ++i;
            for (int k = i; k >= j; k -= 2) result += nums[k];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long findScore(int[] nums) {
        int n = nums.length;
        var idx = new Integer[n];
        for (int i = 0; i < n; i++) idx[i] = i;
        var visited = new boolean[n + 2];
        Arrays.sort(idx, (a, b) -> nums[a] - nums[b]);
        long result = 0L;
        for (int i : idx) {
            if (!visited[i + 1]) {
                visited[i] = visited[i + 2] = true;
                result += nums[i];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findScore(self, nums: List[int]) -> int:
        result, visited = 0, [0] * ((n := len(nums)) + 2)
        for i, num in sorted(enumerate(nums, 1), key=lambda p: p[1]):
            if not visited[i]:
                visited[i + 1] = visited[i - 1] = 1
                result += num
        return result
```
