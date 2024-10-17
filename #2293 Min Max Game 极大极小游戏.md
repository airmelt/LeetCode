# 2293 Min Max Game 极大极小游戏

__Description:__

You are given a __0-indexed__ integer array `nums` whose length is a power of `2`.

Apply the following algorithm on `nums`:

Return _the last number that remains in_ `nums` _after applying the algorithm._

__Example:__

Example 1:

![2293-1](https://assets.leetcode.com/uploads/2022/04/13/example1drawio-1.png)

```text
Input: nums = [1,3,5,2,4,8,2,2]
Output: 1
Explanation: The following arrays are the results of applying the algorithm repeatedly.
First: nums = [1,5,4,2]
Second: nums = [1,4]
Third: nums = [1]
1 is the last remaining number, so we return 1.
```

Example 2:

```text
Input: nums = [3]
Output: 3
Explanation: 3 is already the last remaining number, so we return 3.
```

__Constraints:__

- `1 <= nums.length <= 1024`
- `1 <= nums[i] <= 10 ^ 9`
- `nums.length` is a power of `2`.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，其长度是 `2` 的幂。

对 `nums` 执行下述算法:

执行算法后，返回 `nums` 中剩下的那个数字。

__示例:__

示例 1：

![2293-2](https://assets.leetcode.com/uploads/2022/04/13/example1drawio-1.png)

```text
输入：nums = [1,3,5,2,4,8,2,2]
输出：1
解释：重复执行算法会得到下述数组。
第一轮：nums = [1,5,4,2]
第二轮：nums = [1,4]
第三轮：nums = [1]
1 是最后剩下的那个数字，返回 1 。
```

示例 2：

```text
输入：nums = [3]
输出：3
解释：3 就是最后剩下的数字，返回 3 。
```

__提示：__

- `1 <= nums.length <= 1024`
- `1 <= nums[i] <= 10 ^ 9`
- `nums.length` 是 `2` 的幂

__思路:__

```text
1. 递归
每次将规模减半
按照题意模拟即可
时间复杂度为 O(N), 空间复杂度为 O(N), 时间复杂度为 O(N) + O(N/2) + O(N/4) + ... = O(N)
2. 原地修改
由于每次计算的时候 nums[i] 只和 nums[2 * i] 和 nums[2 * i + 1] 有关
修改 nums[i] 不会影响到 nums[2 * i] 和 nums[2 * i + 1]
所以可以直接在原地修改
分为奇偶按照题意模拟即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
    public int minMaxGame(int[] nums) {
        int n = nums.length;
        while (n > 1) {
            for (int i = 0, m = n >>> 1; i < m; i++) {
                if ((i & 1) == 0) nums[i] = Math.min(nums[i << 1], nums[(i << 1) + 1]);
                else nums[i] = Math.max(nums[i << 1], nums[(i << 1) + 1]);
            }
            n >>>= 1;
        }
        return nums[0];
    }
}
```

__Java__:

```Java
class Solution {
    public int minMaxGame(int[] nums) {
        int n = nums.length;
        while (n > 1) {
            for (int i = 0, m = n >>> 1; i < m; i++) {
                if ((i & 1) == 0) nums[i] = Math.min(nums[i << 1], nums[(i << 1) + 1]);
                else nums[i] = Math.max(nums[i << 1], nums[(i << 1) + 1]);
            }
            n >>>= 1;
        }
        return nums[0];
    }
}
```

__Python__:

```Python
class Solution:
    def minMaxGame(self, nums: List[int]) -> int:
```
