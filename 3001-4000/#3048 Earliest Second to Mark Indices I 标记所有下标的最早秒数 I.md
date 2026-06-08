# 3048 Earliest Second to Mark Indices I 标记所有下标的最早秒数 I

__Description:__

You are given two __1-indexed__ integer arrays, `nums` and, `changeIndices`, having lengths `n` and `m`, respectively.

Initially, all indices in `nums` are unmarked. Your task is to mark __all__ indices in `nums`.

In each second, `s`, in order from `1` to `m` (__inclusive__), you can perform __one__ of the following operations:

- Choose an index `i` in the range `[1, n]` and __decrement__ `nums[i]` by `1`.
- If `nums[changeIndices[s]]` is __equal__ to `0`, __mark__ the index `changeIndices[s]`.
- Do nothing.

Return _an integer denoting the __earliest second__ in the range_ `[1, m]` _when __all__ indices in_ `nums` _can be marked by choosing operations optimally, or_ `-1` _if it is impossible._

__Example:__

Example 1:

```text
Input: nums = [2,2,0], changeIndices = [2,2,2,2,3,2,2,1]
Output: 8
Explanation: In this example, we have 8 seconds. The following operations can be performed to mark all indices:
Second 1: Choose index 1 and decrement nums[1] by one. nums becomes [1,2,0].
Second 2: Choose index 1 and decrement nums[1] by one. nums becomes [0,2,0].
Second 3: Choose index 2 and decrement nums[2] by one. nums becomes [0,1,0].
Second 4: Choose index 2 and decrement nums[2] by one. nums becomes [0,0,0].
Second 5: Mark the index changeIndices[5], which is marking index 3, since nums[3] is equal to 0.
Second 6: Mark the index changeIndices[6], which is marking index 2, since nums[2] is equal to 0.
Second 7: Do nothing.
Second 8: Mark the index changeIndices[8], which is marking index 1, since nums[1] is equal to 0.
Now all indices have been marked.
It can be shown that it is not possible to mark all indices earlier than the 8th second.
Hence, the answer is 8.
```

Example 2:

```text
Input: nums = [1,3], changeIndices = [1,1,1,2,1,1,1]
Output: 6
Explanation: In this example, we have 7 seconds. The following operations can be performed to mark all indices:
Second 1: Choose index 2 and decrement nums[2] by one. nums becomes [1,2].
Second 2: Choose index 2 and decrement nums[2] by one. nums becomes [1,1].
Second 3: Choose index 2 and decrement nums[2] by one. nums becomes [1,0].
Second 4: Mark the index changeIndices[4], which is marking index 2, since nums[2] is equal to 0.
Second 5: Choose index 1 and decrement nums[1] by one. nums becomes [0,0].
Second 6: Mark the index changeIndices[6], which is marking index 1, since nums[1] is equal to 0.
Now all indices have been marked.
It can be shown that it is not possible to mark all indices earlier than the 6th second.
Hence, the answer is 6.
```

Example 3:

```text
Input: nums = [0,1], changeIndices = [2,2,2]
Output: -1
Explanation: In this example, it is impossible to mark all indices because index 1 isn't in changeIndices.
Hence, the answer is -1.
```

__Constraints:__

- `1 <= n == nums.length <= 2000`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= m == changeIndices.length <= 2000`
- `1 <= changeIndices[i] <= n`

__题目描述:__

给你两个下标从 __1__ 开始的整数数组 `nums` 和 `changeIndices` ，数组的长度分别为 `n` 和 `m` 。

一开始， `nums` 中所有下标都是未标记的，你的任务是标记 `nums` 中 __所有__ 下标。

从第 `1` 秒到第 `m` 秒（ _包括_ 第 `m` 秒），对于每一秒 `s` ，你可以执行以下操作 __之一__ :

- 选择范围 `[1, n]` 中的一个下标 `i` ，并且将 `nums[i]` __减少__ `1` 。
- 如果 `nums[changeIndices[s]]` __等于__ `0` ，__标记__ 下标 `changeIndices[s]` 。
- 什么也不做。

请你返回范围 `[1, m]` 中的一个整数，表示最优操作下，标记 `nums` 中 __所有__ 下标的 __最早秒数__ ，如果无法标记所有下标，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [2,2,0], changeIndices = [2,2,2,2,3,2,2,1]
输出：8
解释：这个例子中，我们总共有 8 秒。按照以下操作标记所有下标：
第 1 秒：选择下标 1 ，将 nums[1] 减少 1 。nums 变为 [1,2,0] 。
第 2 秒：选择下标 1 ，将 nums[1] 减少 1 。nums 变为 [0,2,0] 。
第 3 秒：选择下标 2 ，将 nums[2] 减少 1 。nums 变为 [0,1,0] 。
第 4 秒：选择下标 2 ，将 nums[2] 减少 1 。nums 变为 [0,0,0] 。
第 5 秒，标记 changeIndices[5] ，也就是标记下标 3 ，因为 nums[3] 等于 0 。
第 6 秒，标记 changeIndices[6] ，也就是标记下标 2 ，因为 nums[2] 等于 0 。
第 7 秒，什么也不做。
第 8 秒，标记 changeIndices[8] ，也就是标记下标 1 ，因为 nums[1] 等于 0 。
现在所有下标已被标记。
最早可以在第 8 秒标记所有下标。
所以答案是 8 。
```

示例 2：

```text
输入：nums = [1,3], changeIndices = [1,1,1,2,1,1,1]
输出：6
解释：这个例子中，我们总共有 7 秒。按照以下操作标记所有下标：
第 1 秒：选择下标 2 ，将 nums[2] 减少 1 。nums 变为 [1,2] 。
第 2 秒：选择下标 2 ，将 nums[2] 减少 1 。nums 变为 [1,1] 。
第 3 秒：选择下标 2 ，将 nums[2] 减少 1 。nums 变为 [1,0] 。
第 4 秒：标记 changeIndices[4] ，也就是标记下标 2 ，因为 nums[2] 等于 0 。
第 5 秒：选择下标 1 ，将 nums[1] 减少 1 。nums 变为 [0,0] 。
第 6 秒：标记 changeIndices[6] ，也就是标记下标 1 ，因为 nums[1] 等于 0 。
现在所有下标已被标记。
最早可以在第 6 秒标记所有下标。
所以答案是 6 。
```

示例 3：

```text
Input: nums = [0,1], changeIndices = [2,2,2]
Output: -1
Explanation: 这个例子中，无法标记所有下标，因为下标 1 不在 changeIndices 中。
所以答案是 -1 。
```

__提示：__

- `1 <= n == nums.length <= 2000`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= m == changeIndices.length <= 2000`
- `1 <= changeIndices[i] <= n`

__思路:__

```text
二分
如果 mid 能够标记完所有下标
那么 mid + 1 肯定也能标记完
所以可以使用二分查找
当 nums 比 changeIndices 还长的时候
或者 nums 的和比 changeIndices 还长的时候
无论如何也不能标记完, 直接返回 -1
否则二分查找
边界为 [n, m], 因为最少也需要 n 个标记标记完 nums, 最多用完 m 个标记无论如何也标记不完了, m 可以换成 n + sum(nums) 也是可以的
二分查找的时候, 需要检查 n 个数字是否都标记完成
每次标记的时候还需要检查 nums[idx] 是否已经减少到 0
所以可以直接将这次的 mid (不可能达到的时间) 赋予完成时间的记录数组
倒序遍历并记录下所有 nums 的值
最后只需要返回标记完成状态即可
时间复杂度为 O(MlogM), 空间复杂度为 O(N), N 为数组 nums 的长度, M 为数组 changeIndices 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int earliestSecondToMarkIndices(vector<int>& nums, vector<int>& changeIndices) 
    {
        int n = nums.size(), m = changeIndices.size(), left = n - 1, right = m + 1, mid = 0;
        vector<int> done(n);
        if (n > m or accumulate(nums.begin(), nums.end(), 0LL) > m) return -1;
        auto check = [&](int mid) -> bool
        {
            int exam = n, study = 0, idx = 0;
            for (int i = mid - 1; i > -1; i--)
            {
                if (done[idx = changeIndices[i] - 1] != mid)
                {
                    done[idx] = mid;
                    --exam;
                    study += nums[idx];
                }
                else if (study) --study;
            }
            return !study and !exam;
        };
        while (left + 1 < right) (check(mid) ? right : left) = (mid = left + ((right - left) >> 1));
        return right > m ? -1 : right;
    }
};
```

__Java__:

```Java
class Solution {
    public int earliestSecondToMarkIndices(int[] nums, int[] changeIndices) {
        int n = nums.length, m = changeIndices.length, done[] = new int[n], left = n - 1, right = m + 1, mid = 0;
        while (left + 1 < right) {
            if (check(mid = left + ((right - left) >>> 1), nums, changeIndices, done)) right = mid;
            else left = mid;
        }
        return right > m ? -1 : right;
    }

    private boolean check(int mid, int[] nums, int[] changeIndices, int[] done) {
        int exam = nums.length, study = 0, idx = 0;
        for (int i = mid - 1; i > -1; i--) {
            if (done[idx = changeIndices[i] - 1] != mid) {
                done[idx] = mid;
                --exam;
                study += nums[idx];
            }
            else if (study > 0) --study;
        }
        return study == 0 && exam == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def earliestSecondToMarkIndices(self, nums: List[int], changeIndices: List[int]) -> int:
        done, m = [0] * (n := len(nums)), len(changeIndices)

        def check(mid: int) -> bool:
            exam, study = n, 0
            for i in range(mid - 1, -1, -1):
                if done[idx := changeIndices[i] - 1] != mid:
                    done[idx] = mid
                    exam -= 1
                    study += nums[idx]
                elif study:
                    study -= 1
            return not exam and not study
        return -1 if (result := n + sum(nums) + bisect_left(range(n + sum(nums), m + 1), True, key=check)) > m else result
```
