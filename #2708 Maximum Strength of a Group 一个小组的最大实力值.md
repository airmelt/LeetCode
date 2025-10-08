# 2708 Maximum Strength of a Group 一个小组的最大实力值

__Description:__

You are given a __0-indexed__ integer array `nums` representing the score of students in an exam. The teacher would like to form one __non-empty__ group of students with maximal __strength__, where the strength of a group of students of indices `i0`, `i1`, `i2`, ... , `ik` is defined as `nums[i0] * nums[i1] * nums[i2] * ... * nums[ik​]`.

Return the maximum strength of a group the teacher can create.

__Example:__

Example 1:

```text
Input: nums = [3,-1,-5,2,5,-9]
Output: 1350
Explanation: One way to form a group of maximal strength is to group the students at indices [0,2,3,4,5]. Their strength is 3 * (-5) * 2 * 5 * (-9) = 1350, which we can show is optimal.
```

Example 2:

```text
Input: nums = [-4,-5,-4]
Output: 20
Explanation: Group the students at indices [0, 1] . Then, we’ll have a resulting strength of 20. We cannot achieve greater strength.
```

__Constraints:__

- `1 <= nums.length <= 13`
- `-9 <= nums[i] <= 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它表示一个班级中所有学生在一次考试中的成绩。老师想选出一部分同学组成一个 __非空__ 小组，且这个小组的 __实力值__ 最大，如果这个小组里的学生下标为 `i0`, `i1`, `i2`, ... , `ik` ，那么这个小组的实力值定义为 `nums[i0] * nums[i1] * nums[i2] * ... * nums[ik​]` 。

请你返回老师创建的小组能得到的最大实力值为多少。

__示例:__

示例 1：

```text
输入：nums = [3,-1,-5,2,5,-9]
输出：1350
解释：一种构成最大实力值小组的方案是选择下标为 [0,2,3,4,5] 的学生。实力值为 3 * (-5) * 2 * 5 * (-9) = 1350 ，这是可以得到的最大实力值。
```

示例 2：

```text
输入：nums = [-4,-5,-4]
输出：20
解释：选择下标为 [0, 1] 的学生。得到的实力值为 20 。我们没法得到更大的实力值。
```

__提示：__

- `1 <= nums.length <= 13`
- `-9 <= nums[i] <= 9`

__思路:__

```text
数学
分情况讨论
如果当前遍历的数 num 为负数
则最大值为前面的负数最小值和 num 的乘积
如果当前遍历的数 num 为正数
则最大值为前面的正数最大值和 num 的乘积
所以最小值为 min({ mn, num, mn * num, mx * num }), 注意 num 自身也可能是最小值
最大值 max({ mx, num, mn * num, mx * num })
这两个值需要同时计算, 也就是要提前记录下当前的值 cur = mn
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxStrength(vector<int>& nums) 
    {
        long long mn = nums.front(), mx = nums.front();
        for (int i = 1, n = nums.size(); i < n; i++) 
        {
            long long cur = mn, num = nums[i];
            mn = min({ mn, num, mn * num, mx * num });
            mx = max({ mx, num, cur * num, mx * num });
        }
        return mx;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxStrength(int[] nums) {
        long mn = nums[0], mx = nums[0];
        for (int i = 1, n = nums.length; i < n; i++) {
            long cur = mn, num = nums[i];
            mn = Math.min(Math.min(mn, num), Math.min(mn * num, mx * num));
            mx = Math.max(Math.max(mx, num), Math.max(cur * num, mx * num));
        }
        return mx;
    }
}
```

__Python__:

```Python
class Solution:
    def maxStrength(self, nums: List[int]) -> int:
        mn = mx = nums[0]
        for num in nums[1:]:
            mn, mx = min(mn, num, mn * num, mx * num), max(mx, num, mn * num, mx * num)
        return mx
```
