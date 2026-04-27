# 2817 Minimum Absolute Difference Between Elements With Constraint 限制条件下元素之间的最小绝对差

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `x`.

Find the __minimum absolute difference__ between two elements in the array that are at least `x` indices apart.

In other words, find two indices `i` and `j` such that `abs(i - j) >= x` and `abs(nums[i] - nums[j])` is minimized.

Return _an integer denoting the __minimum__ absolute difference between two elements that are at least_ `x` _indices apart_.

__Example:__

Example 1:

```text
Input: nums = [4,3,2,4], x = 2
Output: 0
Explanation: We can select nums[0] = 4 and nums[3] = 4. 
They are at least 2 indices apart, and their absolute difference is the minimum, 0. 
It can be shown that 0 is the optimal answer.
```

Example 2:

```text
Input: nums = [5,3,2,10,15], x = 1
Output: 1
Explanation: We can select nums[1] = 3 and nums[2] = 2.
They are at least 1 index apart, and their absolute difference is the minimum, 1.
It can be shown that 1 is the optimal answer.
```

Example 3:

```text
Input: nums = [1,2,3,4], x = 3
Output: 3
Explanation: We can select nums[0] = 1 and nums[3] = 4.
They are at least 3 indices apart, and their absolute difference is the minimum, 3.
It can be shown that 3 is the optimal answer.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= x < nums.length`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `x` 。

请你找到数组中下标距离至少为 `x` 的两个元素的 __差值绝对值__ 的 __最小值__ 。

换言之，请你找到两个下标 `i` 和 `j` ，满足 `abs(i - j) >= x` 且 `abs(nums[i] - nums[j])` 的值最小。

请你返回一个整数，表示下标距离至少为 `x` 的两个元素之间的差值绝对值的 __最小值__ 。

__示例:__

示例 1：

```text
输入：nums = [4,3,2,4], x = 2
输出：0
解释：我们选择 nums[0] = 4 和 nums[3] = 4 。
它们下标距离满足至少为 2 ，差值绝对值为最小值 0 。
0 是最优解。
```

示例 2：

```text
输入：nums = [5,3,2,10,15], x = 1
输出：1
解释：我们选择 nums[1] = 3 和 nums[2] = 2 。
它们下标距离满足至少为 1 ，差值绝对值为最小值 1 。
1 是最优解。
```

示例 3：

```text
输入：nums = [1,2,3,4], x = 3
输出：3
解释：我们选择 nums[0] = 1 和 nums[3] = 4 。
它们下标距离满足至少为 3 ，差值绝对值为最小值 3 。
3 是最优解。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= x < nums.length`

__思路:__

```text
平衡树
使用平衡树保证搜索和添加都是 O(logN) 的
从下标为 x 的位置开始遍历
将平衡树视为滑动窗口
往里面添加元素并查询边界值
更新结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minAbsoluteDifference(vector<int>& nums, int x) 
    {
        if (!x) return x;
        int n = nums.size(), result = INT_MAX;
        set<int> tree{ INT_MIN >> 1, INT_MAX };
        for (int i = x, y = 0; i < n; i++) 
        {
            tree.insert(nums[i - x]);
            auto it = tree.lower_bound(y = nums[i]);
            result = min(result, min(*it - y, y - *prev(it)));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAbsoluteDifference(List<Integer> nums, int x) {
        var a = nums.stream().mapToInt(i -> i).toArray();
        int result = Integer.MAX_VALUE, n = a.length;
        var tree = new TreeSet<Integer>() {{ add(Integer.MAX_VALUE); add(Integer.MIN_VALUE >> 1); }};
        for (int i = x, y = 0; i < n; i++) {
            tree.add(a[i - x]);
            result = Math.min(result, Math.min(tree.ceiling(y = a[i]) - y, y - tree.floor(y)));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minAbsoluteDifference(self, nums: List[int], x: int) -> int:
        result, sl = inf, SortedList((-inf, inf))
        for v, y in zip(nums, nums[x:]):
            sl.add(v)
            result = min(result, sl[j := sl.bisect_left(y)] - y, y - sl[j - 1])
        return result
```
