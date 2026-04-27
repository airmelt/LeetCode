# 2134 Minimum Swaps to Group All 1's Together II 最少交换次数来组合所有的 1 II

__Description:__

A swap is defined as taking two distinct positions in an array and swapping the values in them.

A circular array is defined as an array where we consider the first element and the last element to be adjacent.

Given a __binary__ __circular__ array `nums`, return _the minimum number of swaps required to group all_ `1`_'s present in the array together at __any location___.

__Example:__

Example 1:

```text
Input: nums = [0,1,0,1,1,0,0]
Output: 1
Explanation: Here are a few of the ways to group all the 1's together:
[0,0,1,1,1,0,0] using 1 swap.
[0,1,1,1,0,0,0] using 1 swap.
[1,1,0,0,0,0,1] using 2 swaps (using the circular property of the array).
There is no way to group all 1's together with 0 swaps.
Thus, the minimum number of swaps required is 1.
```

Example 2:

```text
Input: nums = [0,1,1,1,0,0,1,1,0]
Output: 2
Explanation: Here are a few of the ways to group all the 1's together:
[1,1,1,0,0,0,0,1,1] using 2 swaps (using the circular property of the array).
[1,1,1,1,1,0,0,0,0] using 2 swaps.
There is no way to group all 1's together with 0 or 1 swaps.
Thus, the minimum number of swaps required is 2.
```

Example 3:

```text
Input: nums = [1,1,0,0,1]
Output: 0
Explanation: All the 1's are already grouped together due to the circular property of the array.
Thus, the minimum number of swaps required is 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` is either `0` or `1`.

__题目描述:__

交换 定义为选中一个数组中的两个 互不相同 的位置并交换二者的值。

环形 数组是一个数组，可以认为 第一个 元素和 最后一个 元素 相邻 。

给你一个 __二进制环形__ 数组 `nums` ，返回在 __任意位置__ 将数组中的所有 `1` 聚集在一起需要的最少交换次数。

__示例:__

示例 1：

```text
输入：nums = [0,1,0,1,1,0,0]
输出：1
解释：这里列出一些能够将所有 1 聚集在一起的方案：
[0,0,1,1,1,0,0] 交换 1 次。
[0,1,1,1,0,0,0] 交换 1 次。
[1,1,0,0,0,0,1] 交换 2 次（利用数组的环形特性）。
无法在交换 0 次的情况下将数组中的所有 1 聚集在一起。
因此，需要的最少交换次数为 1 。
```

示例 2：

```text
输入：nums = [0,1,1,1,0,0,1,1,0]
输出：2
解释：这里列出一些能够将所有 1 聚集在一起的方案：
[1,1,1,0,0,0,0,1,1] 交换 2 次（利用数组的环形特性）。
[1,1,1,1,1,0,0,0,0] 交换 2 次。
无法在交换 0 次或 1 次的情况下将数组中的所有 1 聚集在一起。
因此，需要的最少交换次数为 2 。
```

示例 3：

```text
输入：nums = [1,1,0,0,1]
输出：0
解释：得益于数组的环形特性，所有的 1 已经聚集在一起。
因此，需要的最少交换次数为 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `nums[i]` 为 `0` 或者 `1`

__思路:__

```text
滑动窗口
若数组中 1 的个数为 total
枚举 1 开始的位置 i, 则 1 结束的位置应为 (i + total - 1) % n
只需要统计 [i, (i + total - 1) % n] 区间内有几个 0 即可
记滑动窗口内的 0 的个数为 cur, 初始化的时候计算 [0, total) 区间内的 0 的个数 
然后枚举 i, 更新 cur 的值即可
最后返回最小的 cur 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSwaps(vector<int>& nums) 
    {
        int total = accumulate(nums.begin(), nums.end(), 0), n = nums.size(), cur = 0, result = 0;
        for (int i = 0; i < total; i++) cur += 1 - nums[i];
        result = cur;
        for (int i = 1; i < n; i++) 
        {
            if (!(nums[i - 1])) --cur;
            if (!(nums[(i + total - 1) % n])) ++cur;
            result = min(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwaps(int[] nums) {
        int total = Arrays.stream(nums).sum(), n = nums.length, cur = 0, result = 0;
        for (int i = 0; i < total; i++) cur += 1 - nums[i];
        result = cur;
        for (int i = 1; i < n; i++) {
            if (nums[i - 1] == 0) --cur;
            if (nums[(i + total - 1) % n] == 0) ++cur;
            result = Math.min(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        if not (total := sum(nums)):
            return total
        n, cur = len(nums), 0
        for i in range(total):
            cur += 1 - nums[i]
        result = cur
        for i in range(1, n):
            if not nums[i - 1]:
                cur -= 1
            if not nums[(i + total - 1) % n]:
                cur += 1
            result = min(result, cur)
        return result
```
