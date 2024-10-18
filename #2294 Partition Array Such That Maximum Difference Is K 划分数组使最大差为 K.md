# 2294 Partition Array Such That Maximum Difference Is K 划分数组使最大差为 K

__Description:__

You are given an integer array `nums` and an integer `k`. You may partition `nums` into one or more __subsequences__ such that each element in `nums` appears in __exactly__ one of the subsequences.

Return _the __minimum__ number of subsequences needed such that the difference between the maximum and minimum values in each subsequence is __at most___ `k`_._

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [3,6,1,2,5], k = 2
Output: 2
Explanation:
We can partition nums into the two subsequences [3,1,2] and [6,5].
The difference between the maximum and minimum value in the first subsequence is 3 - 1 = 2.
The difference between the maximum and minimum value in the second subsequence is 6 - 5 = 1.
Since two subsequences were created, we return 2. It can be shown that 2 is the minimum number of subsequences needed.
```

Example 2:

```text
Input: nums = [1,2,3], k = 1
Output: 2
Explanation:
We can partition nums into the two subsequences [1,2] and [3].
The difference between the maximum and minimum value in the first subsequence is 2 - 1 = 1.
The difference between the maximum and minimum value in the second subsequence is 3 - 3 = 0.
Since two subsequences were created, we return 2. Note that another optimal solution is to partition nums into the two subsequences [1] and [2,3].
```

Example 3:

```text
Input: nums = [2,2,4,5], k = 0
Output: 3
Explanation:
We can partition nums into the three subsequences [2,2], [4], and [5].
The difference between the maximum and minimum value in the first subsequences is 2 - 2 = 0.
The difference between the maximum and minimum value in the second subsequences is 4 - 4 = 0.
The difference between the maximum and minimum value in the third subsequences is 5 - 5 = 0.
Since three subsequences were created, we return 3. It can be shown that 3 is the minimum number of subsequences needed.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`
- `0 <= k <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。你可以将 `nums` 划分成一个或多个 __子序列__ ，使 `nums` 中的每个元素都 __恰好__ 出现在一个子序列中。

在满足每个子序列中最大值和最小值之间的差值最多为 `k` 的前提下，返回需要划分的 __最少__ 子序列数目。

子序列 本质是一个序列，可以通过删除另一个序列中的某些元素（或者不删除）但不改变剩下元素的顺序得到。

__示例:__

示例 1：

```text
输入：nums = [3,6,1,2,5], k = 2
输出：2
解释：
可以将 nums 划分为两个子序列 [3,1,2] 和 [6,5] 。
第一个子序列中最大值和最小值的差值是 3 - 1 = 2 。
第二个子序列中最大值和最小值的差值是 6 - 5 = 1 。
由于创建了两个子序列，返回 2 。可以证明需要划分的最少子序列数目就是 2 。
```

示例 2：

```text
输入：nums = [1,2,3], k = 1
输出：2
解释：
可以将 nums 划分为两个子序列 [1,2] 和 [3] 。
第一个子序列中最大值和最小值的差值是 2 - 1 = 1 。
第二个子序列中最大值和最小值的差值是 3 - 3 = 0 。
由于创建了两个子序列，返回 2 。注意，另一种最优解法是将 nums 划分成子序列 [1] 和 [2,3] 。
```

示例 3：

```text
输入：nums = [2,2,4,5], k = 0
输出：3
解释：
可以将 nums 划分为三个子序列 [2,2]、[4] 和 [5] 。
第一个子序列中最大值和最小值的差值是 2 - 2 = 0 。
第二个子序列中最大值和最小值的差值是 4 - 4 = 0 。
第三个子序列中最大值和最小值的差值是 5 - 5 = 0 。
由于创建了三个子序列，返回 3 。可以证明需要划分的最少子序列数目就是 3 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 5`
- `0 <= k <= 10 ^ 5`

__思路:__

```text
排序 ➕ 贪心
由于选择的是子序列与数组顺序无关
先将数组排序
这样最大的差值一定是在每个组的头尾之间
如果每个组的头尾两个元素之间的差值大于 k, 则需要划分
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int partitionArray(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int result = 1, cur = nums.front();
        for (int num : nums) 
        {
            if (num - cur > k) 
            {
                ++result;
                cur = num;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int partitionArray(int[] nums, int k) {
        Arrays.sort(nums);
        int result = 1, cur = nums[0];
        for (int num : nums) {
            if (num - cur > k) {
                ++result;
                cur = num;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def partitionArray(self, nums: List[int], k: int) -> int:
        nums.sort()
        result, cur = 1, nums[0]
        for num in nums:
            if num - cur > k:
                result += 1
                cur = num
        return result
```
