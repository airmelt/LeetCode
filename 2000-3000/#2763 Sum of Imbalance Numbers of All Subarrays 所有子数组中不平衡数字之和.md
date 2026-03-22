# 2763 Sum of Imbalance Numbers of All Subarrays 所有子数组中不平衡数字之和

__Description:__

The __imbalance number__ of a __0-indexed__ integer array `arr` of length `n` is defined as the number of indices in `sarr = sorted(arr)` such that:

- `0 <= i < n - 1`, and
- `sarr[i+1] - sarr[i] > 1`

Here, `sorted(arr)` is the function that returns the sorted version of `arr`.

Given a __0-indexed__ integer array `nums`, return _the __sum of imbalance numbers__ of all its __subarrays___.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [2,3,1,4]
Output: 3
Explanation: There are 3 subarrays with non-zero imbalance numbers:
```

- Subarray [3, 1] with an imbalance number of 1.
- Subarray [3, 1, 4] with an imbalance number of 1.
- Subarray [1, 4] with an imbalance number of 1.

The imbalance number of all other subarrays is 0. Hence, the sum of imbalance numbers of all the subarrays of nums is 3.

Example 2:

```text
Input: nums = [1,3,3,3,5]
Output: 8
Explanation: There are 7 subarrays with non-zero imbalance numbers:
```

- Subarray [1, 3] with an imbalance number of 1.
- Subarray [1, 3, 3] with an imbalance number of 1.
- Subarray [1, 3, 3, 3] with an imbalance number of 1.
- Subarray [1, 3, 3, 3, 5] with an imbalance number of 2.
- Subarray [3, 3, 3, 5] with an imbalance number of 1.
- Subarray [3, 3, 5] with an imbalance number of 1.
- Subarray [3, 5] with an imbalance number of 1.

The imbalance number of all other subarrays is 0. Hence, the sum of imbalance numbers of all the subarrays of nums is 8.

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= nums.length`

__题目描述:__

一个长度为 `n` 下标从 __0__ 开始的整数数组 `arr` 的 __不平衡数字__ 定义为，在 `sarr = sorted(arr)` 数组中，满足以下条件的下标数目:

- `0 <= i < n - 1` ，和
- `sarr[i+1] - sarr[i] > 1`

这里， `sorted(arr)` 表示将数组 `arr` 排序后得到的数组。

给你一个下标从 __0__ 开始的整数数组 `nums` ，请你返回它所有 __子数组__ 的 __不平衡数字__ 之和。

子数组指的是一个数组中连续一段 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,3,1,4]
输出：3
解释：总共有 3 个子数组有非 0 不平衡数字：
```

- 子数组 [3, 1] ，不平衡数字为 1 。
- 子数组 [3, 1, 4] ，不平衡数字为 1 。
- 子数组 [1, 4] ，不平衡数字为 1 。

其他所有子数组的不平衡数字都是 0 ，所以所有子数组的不平衡数字之和为 3 。

示例 2：

```text
输入：nums = [1,3,3,3,5]
输出：8
解释：总共有 7 个子数组有非 0 不平衡数字：
```

- 子数组 [1, 3] ，不平衡数字为 1 。
- 子数组 [1, 3, 3] ，不平衡数字为 1 。
- 子数组 [1, 3, 3, 3] ，不平衡数字为 1 。
- 子数组 [1, 3, 3, 3, 5] ，不平衡数字为 2 。
- 子数组 [3, 3, 3, 5] ，不平衡数字为 1 。
- 子数组 [3, 3, 5] ，不平衡数字为 1 。
- 子数组 [3, 5] ，不平衡数字为 1 。

其他所有子数组的不平衡数字都是 0 ，所以所有子数组的不平衡数字之和为 8 。

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= nums.length`

__思路:__

```text
1. 暴力法(无法通过)
遍历所有排序后子数组
检查相邻元素是否差值大于 1
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N)
2. 枚举
注意到 n 取值为 [1, 1000]
nums[i] 取值为 [1, n]
用一个 visited 数组记录子数组中出现的元素
统计可能出现的不平衡数
每次放入一个新元素的时候
如果相邻没有元素, 说明它到其他元素的距离大于 1, 不平衡数加 1
如果相邻只有一个元素, 不平衡数不变
如果有两个元素, 加入之后会使得原本有距离大于 1 的两个元素消失, 故不平衡数刷要减 1
结果累加上所有子数组的不平衡数
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
3. 贡献法
令 right[i] 表示 nums[i] 右侧的 nums[i] 和 nums[i] - 1 的最近下标
初始化 right[i] 为 0, 只考虑右侧的情况
idx[i] 记录 nums[i] 的位置, idx[nums[i]] = i
初始化 idx[i] 为 n, 表示右侧的最大值
对每个 nums[i] 计算能作为不平衡数的左端点和右端点相乘
最后单个的子数组不合法需要减去
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumImbalanceNumbers(vector<int>& nums) 
    {
        int n = nums.size(), result = 0, right[n], idx[n + 1];
        fill(idx, idx + n + 1, n);
        for (int i = n - 1; i > -1; i--) 
        {
            right[i] = min(idx[nums[i]], idx[nums[i] - 1]);
            idx[nums[i]] = i;
        }
        memset(idx, -1, sizeof(idx));
        for (int i = 0; i < n; i++) 
        {
            result += (i - idx[nums[i] - 1]) * (right[i] - i);
            idx[nums[i]] = i;
        }
        return result - (n * (n + 1) >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int sumImbalanceNumbers(int[] nums) {
        int n = nums.length, right[] = new int[n], idx[] = new int[n + 1], result = 0;
        Arrays.fill(idx, n);
        for (int i = n - 1; i > -1; i--) {
            right[i] = Math.min(idx[nums[i]], idx[nums[i] - 1]);
            idx[nums[i]] = i;
        }
        Arrays.fill(idx, -1);
        for (int i = 0; i < n; i++) {
            result += (i - idx[nums[i] - 1]) * (right[i] - i);
            idx[nums[i]] = i;
        }
        return result - (n * (n + 1) >>> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def sumImbalanceNumbers(self, nums: List[int]) -> int:
        n, result = len(nums), 0
        for i, num in enumerate(nums):
            visited = [False] * (n + 2)
            visited[num], cur = True, 0
            for j in range(i + 1, n):
                if not visited[nums[j]]:
                    cur += 1 - visited[nums[j] - 1] - visited[nums[j] + 1]
                    visited[nums[j]] = True
                result += cur
        return result
```
