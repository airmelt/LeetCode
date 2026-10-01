# 3194 Minimum Average of Smallest and Largest Elements 最小元素和最大元素的最小平均值

__Description:__

You have an array of floating point numbers `averages` which is initially empty. You are given an array `nums` of `n` integers where `n` is even.

You repeat the following procedure `n / 2` times:

- Remove the __smallest__ element, `minElement`, and the __largest__ element `maxElement`, from `nums`.
- Add `(minElement + maxElement) / 2` to `averages`.

Return the __minimum__ element in `averages`.

__Example:__

Example 1:

```text
Input: nums = [7,8,3,4,15,13,4,1]

Output: 5.5

Explanation:
```

| step | nums | averages |
| ---- | ---- | -------- |
| 0 | [7,8,3,4,15,13,4,1] | [] |
| 1 | [7,8,3,4,13,4] | [8] |
| 2 | [7,8,4,4] | [8,8] |
| 3 | [7,4] | [8,8,6] |
| 4 | [] | [8,8,6,5.5] |

The smallest element of averages, 5.5, is returned.

Example 2:

```text
Input: nums = [1,9,8,3,10,5]

Output: 5.5

Explanation:
```

| step | nums | averages |
| ---- | ---- | -------- |
| 0 | [1,9,8,3,10,5] | [] |
| 1 | [9,8,3,5] | [5.5] |
| 2 | [8,5] | [5.5,6] |
| 3 | [] | [5.5,6,6.5] |

Example 3:

```text
Input: nums = [1,2,3,7,8,9]

Output: 5.0

Explanation:
```

| step | nums | averages |
| ---- | ---- | -------- |
| 0 | [1,2,3,7,8,9] | [] |
| 1 | [2,3,7,8] | [5] |
| 2 | [3,7] | [5,5] |
| 3 | [] | [5,5,5] |

__Constraints:__

- `2 <= n == nums.length <= 50`
- `n` is even.
- `1 <= nums[i] <= 50`

__题目描述:__

你有一个初始为空的浮点数数组 `averages`。另给你一个包含 `n` 个整数的数组 `nums`，其中 `n` 为偶数。

你需要重复以下步骤 `n / 2` 次：

- 从 `nums` 中移除 __最小__ 的元素 `minElement` 和 __最大__ 的元素 `maxElement`。
- 将 `(minElement + maxElement) / 2` 加入到 `averages` 中。

返回 `averages` 中的 __最小__ 元素。

__示例:__

示例 1：

```text
输入： nums = [7,8,3,4,15,13,4,1]

输出： 5.5

解释：
```

| 步骤 | nums | averages |
| ---- | ---- | -------- |
| 0 | [7,8,3,4,15,13,4,1] | [] |
| 1 | [7,8,3,4,13,4] | [8] |
| 2 | [7,8,4,4] | [8,8] |
| 3 | [7,4] | [8,8,6] |
| 4 | [] | [8,8,6,5.5] |

返回 averages 中最小的元素，即 5.5。

示例 2：

```text
输入： nums = [1,9,8,3,10,5]

输出： 5.5

解释：
```

| 步骤 | nums | averages |
| ---- | ---- | -------- |
| 0 | [1,9,8,3,10,5] | [] |
| 1 | [9,8,3,5] | [5.5] |
| 2 | [8,5] | [5.5,6] |
| 3 | [] | [5.5,6,6.5] |

示例 3：

```text
输入： nums = [1,2,3,7,8,9]

输出： 5.0

解释：
```

| 步骤 | nums | averages |
| ---- | ---- | -------- |
| 0 | [1,2,3,7,8,9] | [] |
| 1 | [2,3,7,8] | [5] |
| 2 | [3,7] | [5,5] |
| 3 | [] | [5,5,5] |

__提示：__

- `2 <= n == nums.length <= 50`
- `n` 为偶数。
- `1 <= nums[i] <= 50`

__思路:__

```text
模拟
先将数组进行排序
然后取一头一尾的最小和
最后求平均值
除以浮点数 2.0
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    double minimumAverage(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), result = INT_MAX;
        for (int i = 0; i < (n >> 1); i++) result = min(result, nums[i] + nums[n - 1 - i]);
        return result / 2.0;
    }
};
```

__Java__:

```Java
class Solution {
    public double minimumAverage(int[] nums) {
        Arrays.sort(nums);
        return IntStream.range(0, nums.length >>> 1).map(i -> nums[i] + nums[nums.length - 1 - i]).min().getAsInt() / 2.0;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumAverage(self, nums: List[int]) -> float:
        return 0 if not (nums := sorted(nums)) else min(nums[i] + nums[-1 - i] for i in range(len(nums) >> 1)) / 2
```
