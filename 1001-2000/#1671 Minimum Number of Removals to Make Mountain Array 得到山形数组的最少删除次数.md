# 1671 Minimum Number of Removals to Make Mountain Array 得到山形数组的最少删除次数

__Description:__

You may recall that an array `arr` is a __mountain array__ if and only if:

- `arr.length >= 3`
- There exists some index `i` (__0-indexed__) with `0 < i < arr.length - 1` such that:
  - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
  - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`
- `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
- `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

- `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
- `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given an integer array `nums`​​​, return _the __minimum__ number of elements to remove to make_ `nums_​​​_` _a __mountain array__._

__Example:__

Example 1:

```text
Input: nums = [1,3,1]
Output: 0
Explanation: The array itself is a mountain array so we do not need to remove any elements.
```

Example 2:

```text
Input: nums = [2,1,1,5,6,2,3,1]
Output: 3
Explanation: One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].
```

__Constraints:__

- `3 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- It is guaranteed that you can make a mountain array out of `nums`.

__题目描述:__

我们定义 `arr` 是 _山形数组_ 当且仅当它满足:

- `arr.length >= 3`
- 存在某个下标 `i` （__从 0 开始__） 满足 `0 < i < arr.length - 1` 且:
  - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
  - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`
- `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
- `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

- `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
- `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

给你整数数组 `nums`​ ，请你返回将 `nums` 变成 __山形状数组__ 的​ __最少__ 删除次数。

__示例:__

示例 1：

```text
输入：nums = [1,3,1]
输出：0
解释：数组本身就是山形数组，所以我们不需要删除任何元素。
```

示例 2：

```text
输入：nums = [2,1,1,5,6,2,3,1]
输出：3
解释：一种方法是将下标为 0，1 和 5 的元素删除，剩余元素为 [1,5,6,3,1] ，是山形数组。
```

__提示：__

- `3 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 9`
- 题目保证 `nums` 删除一些元素后一定能得到山形数组。

__思路:__

```text
动态规划
转化为两个最长上升子序列问题, 分别是从左往右和从右往左
left[i] 表示以 nums[i] 开始的从左往右最长上升子序列的长度
right[i] 表示以 nums[i] 开始的从右往左最长上升子序列的长度
left[i] = max(left[i], left[j] + 1), i < j < n, 且 nums[j] < nums[i], right 类似
初始化所有值为 1
最后返回的时候注意必须要是山脉数组所以 left[i] 和 right[i] 都要大于 1
返回 n - (left[i] + right[i] - 1) 的最大值, 减 1 是因为重复计算了 nums[i]
最长上升子序列可以用二分查找优化, 所以时间复杂度可以降到 O(NlogN)
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int minimumMountainRemovals(vector<int>& nums) {
        int n = nums.size(), result = 0;
        vector<int> left(n, 1), right(n, 1);
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (nums[j] > nums[i]) left[j] = max(left[j], left[i] + 1);
        for (int i = n - 1; i > -1; i--) for (int j = i - 1; j > -1; j--) if (nums[j] > nums[i]) right[j] = max(right[j], right[i] + 1);
        for (int i = 0; i < n; i++) if (left[i] > 1 and right[i] > 1) result = max(result, left[i] + right[i] - 1);
        return n - result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        int n = nums.length, left[] = new int[n], right[] = new int[n], result = 0;
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (nums[j] > nums[i]) left[j] = Math.max(left[j], left[i] + 1);
        for (int i = n - 1; i > -1; i--) for (int j = i - 1; j > -1; j--) if (nums[j] > nums[i]) right[j] = Math.max(right[j], right[i] + 1);
        for (int i = 0; i < n; i++) if (left[i] > 1 && right[i] > 1) result = Math.max(result, left[i] + right[i] - 1);
        return n - result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMountainRemovals(self, nums: List[int]) -> int:
        pos1, pos2, pre, sub = [], [], [1] * (n := len(nums)), [1] * n
        for i in range(n):
            if (j := bisect_left(pos1, nums[i])) == len(pos1):
                pos1.append(nums[i])
            else: 
                pos1[j] = nums[i]
            if (k := bisect_left(pos2, nums[n - 1 - i])) == len(pos2):
                pos2.append(nums[n - 1 - i])
            else: 
                pos2[k] = nums[n - 1 - i]
            pre[i], sub[n - 1 - i] = j + 1, k + 1
        return min(n + 1 - x - y for x, y in zip(pre, sub) if x > 1 and y > 1)
```
