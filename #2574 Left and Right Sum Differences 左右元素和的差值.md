# 2574 Left and Right Sum Differences 左右元素和的差值

__Description:__

You are given a __0-indexed__ integer array `nums` of size `n`.

Define two arrays `leftSum` and `rightSum` where:

- `leftSum[i]` is the sum of elements to the left of the index `i` in the array `nums`. If there is no such element, `leftSum[i] = 0`.
- `rightSum[i]` is the sum of elements to the right of the index `i` in the array `nums`. If there is no such element, `rightSum[i] = 0`.

Return an integer array `answer` of size `n` where `answer[i] = |leftSum[i] - rightSum[i]|`.

__Example:__

Example 1:

```text
Input: nums = [10,4,8,3]
Output: [15,1,11,22]
Explanation: The array leftSum is [0,10,14,22] and the array rightSum is [15,11,3,0].
The array answer is [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22].
```

Example 2:

```text
Input: nums = [1]
Output: [0]
Explanation: The array leftSum is [0] and the array rightSum is [0].
The array answer is [|0 - 0|] = [0].
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的长度为 `n` 的整数数组 `nums`。

定义两个数组 `leftSum` 和 `rightSum`，其中:

- `leftSum[i]` 是数组 `nums` 中下标 `i` 左侧元素之和。如果不存在对应的元素， `leftSum[i] = 0` 。
- `rightSum[i]` 是数组 `nums` 中下标 `i` 右侧元素之和。如果不存在对应的元素， `rightSum[i] = 0` 。

返回长度为 `n` 数组 `answer`，其中 `answer[i] = |leftSum[i] - rightSum[i]|`。

__示例:__

示例 1：

```text
输入：nums = [10,4,8,3]
输出：[15,1,11,22]
解释：数组 leftSum 为 [0,10,14,22] 且数组 rightSum 为 [15,11,3,0] 。
数组 answer 为 [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22] 。
```

示例 2：

```text
输入：nums = [1]
输出：[0]
解释：数组 leftSum 为 [0] 且数组 rightSum 为 [0] 。
数组 answer 为 [|0 - 0|] = [0] 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
模拟
先计算数组的和记为 right
每次遍历到 nums[i], 记录 x = nums[i]
right -= x
然后计算 left 和 right 的差值, 赋值给 nums[i]
left += x
最后返回 nums 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> leftRightDifference(vector<int>& nums) 
    {
        int left = 0, right = accumulate(nums.begin(), nums.end(), 0), x = 0, n = nums.size();
        for (int i = 0; i < n; i++) 
        {
            right -= (x = nums[i]);
            nums[i] = abs(left - right);
            left += x;
        }
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] leftRightDifference(int[] nums) {
        int left = 0, right = (int)Arrays.stream(nums).sum(), n = nums.length, x = 0;
        for (int i = 0; i < n; i++) {
            right -= (x = nums[i]);
            nums[i] = Math.abs(left - right);
            left += x;
        }
        return nums;
    }
}
```

__Python__:

```Python
class Solution:
    def leftRightDifference(self, nums: List[int]) -> List[int]:
        return [] if not (pre := [0] + list(accumulate(nums))) else [abs(pre[-1] - pre[i + 1] - pre[i]) for i in range(len(nums))]
```
