# 798 Smallest Rotation with Highest Score 得分最高的最小轮调

__Description__:
You are given an array nums. You can rotate it by a non-negative integer k so that the array becomes [nums[k], nums[k + 1], ... nums[nums.length - 1], nums[0], nums[1], ..., nums[k-1]]. Afterward, any entries that are less than or equal to their index are worth one point.

For example, if we have nums = [2,4,1,3,0], and we rotate by k = 2, it becomes [1,3,0,2,4]. This is worth 3 points because 1 > 0 [no points], 3 > 1 [no points], 0 <= 2 [one point], 2 <= 3 [one point], 4 <= 4 [one point].
Return the rotation index k that corresponds to the highest score we can achieve if we rotated nums by it. If there are multiple answers, return the smallest such index k.

__Example:__

Example 1:

Input: nums = [2,3,1,4,0]
Output: 3
Explanation: Scores for each k are listed below:
k = 0,  nums = [2,3,1,4,0],    score 2
k = 1,  nums = [3,1,4,0,2],    score 3
k = 2,  nums = [1,4,0,2,3],    score 3
k = 3,  nums = [4,0,2,3,1],    score 4
k = 4,  nums = [0,2,3,1,4],    score 3
So we should choose k = 3, which has the highest score.

Example 2:

Input: nums = [1,3,0,2,4]
Output: 0
Explanation: nums will always have 3 points no matter how it shifts.
So we will choose the smallest k, which is 0.

__Constraints:__

1 <= nums.length <= 10^5
0 <= nums[i] < nums.length

__题目描述__:
给定一个数组 A，我们可以将它按一个非负整数 K 进行轮调，这样可以使数组变为 A[K], A[K+1], A{K+2], ... A[A.length - 1], A[0], A[1], ..., A[K-1] 的形式。此后，任何值小于或等于其索引的项都可以记作一分。

例如，如果数组为 [2, 4, 1, 3, 0]，我们按 K = 2 进行轮调后，它将变成 [1, 3, 0, 2, 4]。这将记作 3 分，因为 1 > 0 [no points], 3 > 1 [no points], 0 <= 2 [one point], 2 <= 3 [one point], 4 <= 4 [one point]。

在所有可能的轮调中，返回我们所能得到的最高分数对应的轮调索引 K。如果有多个答案，返回满足条件的最小的索引 K。

__示例 :__

示例 1：

输入：[2, 3, 1, 4, 0]
输出：3
解释：
下面列出了每个 K 的得分：
K = 0,  A = [2,3,1,4,0],    score 2
K = 1,  A = [3,1,4,0,2],    score 3
K = 2,  A = [1,4,0,2,3],    score 3
K = 3,  A = [4,0,2,3,1],    score 4
K = 4,  A = [0,2,3,1,4],    score 3
所以我们应当选择 K = 3，得分最高。

示例 2：

输入：[1, 3, 0, 2, 4]
输出：0
解释：
A 无论怎么变化总是有 3 分。
所以我们将选择最小的 K，即 0。

__提示:__

A 的长度最大为 20000。
A[i] 的取值范围是 [0, A.length]。

__思路__:

差分数组
先移动数组看看

```text
[2, 3, 1, 4, 0]
A[0] = 2, 移动到 A[A[0]] = A[2], k = 3
A[1] = 3, 移动到 A[A[1]] = A[3], k = 3
A[2] = 1, 移动到 A[A[2]] = A[1], k = 1
A[3] = 4, 移动到 A[A[3]] = A[4], k = 4
A[4] = 0, 移动到 A[A[4]] = A[0], k = 4
```

这里的 k 表示移动之后 A[i] 位置就刚好可以得分, 如果 k 增大, A[i] 将不得分, k 减小, A[i] 继续得分
记录下不得分的临界值 end = (i - A[i] + n + 1) % n, n 为数组长度
得分的临界值 start = i
在开始的区间 -1, 结束的区间 +1
然后累加求出最值即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int bestRotation(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        vector<int> record(n);
        for (int i = 0; i < n; i++) --record[(i - nums[i] + 1 + n) % n];
        for (int i = 1; i < n; i++)
        {
            record[i] += record[i - 1] + 1;
            result = record[i] > record[result] ? i : result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int bestRotation(int[] nums) {
        int n = nums.length, record[] = new int[n], result = 0;
        for (int i = 0; i < n; i++) --record[(i - nums[i] + 1 + n) % n];
        for (int i = 1; i < n; i++) {
            record[i] += record[i - 1] + 1;
            result = record[i] > record[result] ? i : result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def bestRotation(self, nums: List[int]) -> int:
        record, k = [0] * (n := len(nums)), 0
        for i in range(n):
            record[(i - nums[i] + 1 + n) % n] -= 1
        for i in range(1, n):
            record[i] += record[i - 1] + 1
            k = i if record[i] > record[k] else k
        return k
```
